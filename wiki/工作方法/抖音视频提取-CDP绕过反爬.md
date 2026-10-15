---
date: 2026-09-27
tags: [工作方法, 抖音, 采集, 反爬, CDP]
review_status: unreviewed
sources:
  - wiki/log.md#2026-09-27-00-50-ingest
  - wiki/log.md#2026-08-18-00-12-ingest
  - ~/.workbuddy/skills/web-access/scripts/cdp_capture.mjs
  - ~/.workbuddy/skills/douyin-obsidian/scripts/douyin_downloader.py
---

# 抖音视频提取：CDP 真实浏览器绕过反爬

> **状态**：unreviewed / 最后更新 2026-10-15
> **来源**：[[wiki/log.md]]（2026-09-27 00:50 采购降本视频采集、2026-08-18 00:12 12 热词视频采集）、web-access skill 的 `cdp_capture.mjs`、douyin-obsidian skill 的 `douyin_downloader.py`

## 核心要点

- douyin-obsidian skill 的**默认链路（纯 `requests` 抓 SSR 网页）已经失效**：抖音把视频元数据从服务端渲染（SSR）HTML 里移走了，现在必须由客户端 JS 异步加载。
- **能成功的做法是"驱动真实浏览器"**：用 Chrome/Edge 的 CDP（Chrome DevTools Protocol）打开分享页，让页面自己发出**带签名的** `aweme/detail` 请求，再用 `Network.getResponseBody` 截获响应、截获音频直链。
- 两种方法**唯一的差别在"如何拿到音频源 + 元数据"**；拿到之后 `FFmpeg 转码 → 硅基流动 SenseVoiceSmall 转写` 这一步，两条路完全一致。
- 关键前提：浏览器必须带着**用户已登录的抖音 Cookie**（用户自己的浏览器配置），这是"签名请求能通过"的真正原因；纯无头 HTTP 客户端拿不到。

## 详细内容

### 一、失败的方法（旧默认链路）

位于 `douyin-obsidian/scripts/douyin_downloader.py` 的 `parse_share_url()`：

1. 用 Python `requests` + 移动端 UA，访问抖音分享短链，跟随重定向得到 `video_id`。
2. 拼出 `https://www.douyin.com/share/video/{id}`，再 `requests.get` 拉取页面 HTML。
3. 正则提取 `window._ROUTER_DATA` 这段 JSON，从中读 `loaderData["video_(id)/page"]["videoInfoRes"]["item_list"][0]`。

**为什么失败（实测现象）：**

- SSR HTML 里已经**没有 `videoInfoRes`** 字段了——抖音把元数据改成前端异步加载，服务端只吐一个壳。代码在 `pattern.search(response.text)` 后取不到数据，或直接 `KeyError: 'videoInfoRes'`。
- 直接打官方接口也被拦：
  - `iesdouyin.com/web/api/v2/aweme/iteminfo/` → 返回 `encrypt_data_miss`
  - `douyin.com/aweme/v1/web/aweme/detail/` → 返回 `Blocked by ArgusSecurityPlugin`
- 根因：这些接口要求 **X-Bogus / `_signature` 签名 + 会话 Cookie**，而签名只能在页面 JS 运行时里生成、Cookie 只有登录浏览器才有。裸 `requests` 两者都没有。

> 本质上：这是"无头 HTTP / 抓 SSR 网页"流派，对今天的抖音已经不适用。

### 二、成功的方法（CDP 真实浏览器）

**前置：启动带调试端口的浏览器（关键，且端口号易错）**

```bash
# 必须是 9222（浏览器调试端口），不是 3456（web-access 代理的 API 端口）
# 用独立 profile，切勿杀掉用户正在用的主浏览器
# ⚠️ 必须带 --remote-allow-origins=*：否则 cdp_capture.mjs 直连浏览器 WS 会收到 403，
#    现象是 "Received network error or non-101 status code" / WebSocket 握手失败。
#    若发现 9222 上已有旧实例但缺该 flag，先 taskkill 掉再用本命令重启（headed 模式，勿加 --headless）。
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" \
  --remote-debugging-port=9222 \
  --remote-allow-origins=* \
  --user-data-dir="C:/Users/Lenovo/.workbuddy/skills/web-access/edge-profile"
```

**捕获脚本**：`web-access/scripts/cdp_capture.mjs <分享链接> [输出目录]`

它做的事（与失败方法对照）：

1. 连 `http://127.0.0.1:9222/json/version` 拿浏览器 WS 地址，用 `WebSocket` 直连浏览器（**绕过 web-access 的 3456 代理**，代理的 WS 握手会失败，这里直接打浏览器）。
2. `Target.createTarget` 开一个新标签导航到分享链接，启用 `Page / Network / Runtime`。
3. 等约 **9 秒**——让 SPA 渲染、让页面**自己**发出带签名的 `aweme/v1/web/aweme/detail` XHR（这一步是裸 HTTP 永远做不到的）。
4. 扫 `Network.responseReceived` 事件：
   - 命中 `aweme/v1/web/aweme/detail` → `Network.getResponseBody` 取响应 → 解析 `aweme_detail`（标题 `desc`、作者 `author.nickname`、发布时间 `create_time`、时长 `duration`、数据 `stat`）。
   - 命中 `media-audio-und-mp4a` → 这是**音频直链**（不是整段视频），记下 URL。
5. 用 curl 带 Referer 下载音频（防 CDN 盗链）：
   ```bash
   curl -s -e "https://www.douyin.com/" -o audio.mp4 "<音频直链>"
   ```
6. FFmpeg 转成硅基流动偏好的 16kHz 单声道：
   ```bash
   ffmpeg -i audio.mp4 -ar 16000 -ac 1 audio.mp3
   ```
7. 硅基流动 SenseVoiceSmall 转写（复用 douyin-obsidian 的 `transcribe_single_audio` 逻辑）：
   ```bash
   # POST https://api.siliconflow.cn/v1/audio/transcriptions
   # form-data: file=audio.mp3, model=FunAudioLLM/SenseVoiceSmall
   # header: Authorization: Bearer <API_KEY>
   ```
   或直接复用 skill：`python douyin_downloader.py --link "<链接>" --action extract`（它内部就是 FFmpeg + 这个 API，只是前面取源那步我们替它绕过了）。

> 注意：第 7 步的 ASR 与旧方法**完全相同**——旧方法失败只失败在"取音频源"这一步，转写本身没问题。

### 三、两种方法本质区别（对照表）

| 维度 | 失败方法（默认链路） | 成功方法（CDP） |
|------|----------------------|----------------|
| 取源方式 | Python `requests` 抓 SSR 网页 | 真实浏览器（CDP）打开页面 |
| 元数据来源 | 解析 `window._ROUTER_DATA` 里的 `videoInfoRes` | 拦截页面自发的有签名 `aweme/detail` 响应 |
| 音频获取 | 下载整段视频（`play_addr` 去水印）再抽音轨 | 直接截获**音频直链** `media-audio-und-mp4a` |
| 是否需要签名/ Cookie | 无（被打回 `encrypt_data_miss` / Argus） | 有（页面 JS 生成签名 + 浏览器登录态 Cookie） |
| 是否执行 JS | 否（纯 HTTP） | 是（真实渲染 SPA） |
| 下游转写 | FFmpeg + SenseVoiceSmall | FFmpeg + SenseVoiceSmall（一样） |
| 浏览器端口 | 不涉及 | 必须 9222（不是代理 3456） |
| 代表性成功案例 | — | 2026-08-18 12 热词（Playwright 同族）、2026-09-27 采购降本（CDP） |

**一句话总结**：抖音的元数据接口和媒体 CDN 要求"已登录 + JS 运行时 + 合法签名"的浏览器上下文。能不能提取，取决于你**是不是在用真正的浏览器**——这正是"驱动真实浏览器"（Playwright 也好、CDP 也好）能成、而 `requests` 抓网页不能成的根本原因。

### 四、CDP 与 Playwright 的关系

- 两者是**同一流派**：都是"驱动真实浏览器"。2026-08-18 的 12 热词视频就是用 Playwright 驱动本机 Edge、拦截 `aweme/detail` 成功的（见 log）。
- CDP 是 Playwright 底层封装的协议。本次直接用裸 CDP（不依赖 Playwright 安装）连浏览器调试端口，顺手还绕开了 web-access 代理的 WS 握手问题。
- CDP 额外优势：能直接拿到**音频直链**而非整段视频，体积更小、且避开整段视频 CDN 更严的防盗链。

### 五、适用边界与风险

- ⚠️ **这是临时绕过方案，不是永久解法**。抖音反爬是动态变化的，签名算法、接口路径随时可能变；脚本留在 `web-access/skills/tmp/` 下，未进 vault。
- ⚠️ **依赖用户登录态**：必须用用户自己的浏览器配置（带抖音 Cookie）。无登录态时签名/接口仍可能失败。
- ⚠️ **9 秒等待是脆弱的**：靠固定 sleep 等页面加载，网络慢或接口延迟会漏抓。更稳的做法是监听 `Network.loadingFinished` 或轮询直到拿到 `aweme_detail`。
- ✅ 下游 FFmpeg / ASR 链路稳定，可长期复用，无需改动。
- ✅ **2026-10-15 增补：无登录态的 Playwright 无头浏览器验证成功**（见第六节）——推翻了"必须用户 Cookie"的旧结论，这是当前**首选方案**（无需用户开调试端口、无需登录态）。

## 六、无登录态 Playwright 无头方案（2026-10-15 验证成功）

> 案例：[[wiki/AI与工具/普通人如何用AI实现跃迁-刘小排访谈-抖音视频]]（35:32 长视频，从下载到 14,711 字转录全程自动化）。

要点：

1. **不需要用户 Cookie/登录态**：playwright 全新 context（无任何 cookie）打开 `https://www.douyin.com/video/<id>`，页面自发签名的 `aweme/detail` 照样返回全量 `aweme_detail`。旧结论"必须登录浏览器 Cookie"（第五节 ⚠️）至少在 2026-10-15 不成立。
2. 隐身处理：`launch(args=["--disable-blink-features=AutomationControlled"])` + `add_init_script("Object.defineProperty(navigator,'webdriver',{get:()=>undefined})")`。
3. 捕获：`page.on("response")` 里判断 URL 含 `aweme` 且 `detail`，`resp.text()` 存盘——比固定 sleep 稳（轮询 `<video>` 就绪后多等几秒再收尾）。
4. **视频直链用 API 响应里的 `play_addr.url_list[0]`，不要用页面 `<video>` 的 src**：那是占位视频（`uuu_265.mp4`）或 MSE blob。下载带 `Referer: https://www.douyin.com/` + 桌面 UA，190MB 长视频直链可下。
5. 下游不变：ffmpeg（`-ac 1 -ar 16000`）→ douyin-obsidian skill 的 `DouyinProcessor.extract_text_from_audio`（>1h 或 >50MB 自动 9 分钟分段）。
6. ⚠️ 版本坑：pip playwright 与 `ms-playwright` 已装内核 build 号不匹配会报 executable 不存在；用 `executable_path` 直接指向已有 `chromium-<build>/chrome-win64/chrome.exe` 绕过，不必重装内核。
7. `iesdouyin.com/share/video/<id>/` 的 `_ROUTER_DATA` 也已不含 `videoInfoRes`——纯 requests 彻底无解，别再试。

流程对照表新增一列：**无头 Playwright（2026-10-15）**——取源=无登录 context 打开页面；元数据=拦截带签名响应；媒体=API `play_addr` 整段视频；签名/Cookie=页面 JS 现场生成（无需登录态）；JS=是。

## 相关页面

- [[采购面试-降本怎么回答]] — 本方法产出的最新一篇（TCO 降本专项）
- [[采购面试-制造业三类能力题]] — 同批求职视频（早期用默认链路成功的案例，反爬升级前的窗口期）
- [[wiki/工作方法/LLM-Wiki知识库搭建指南]] — 本仓库的 Ingest 总流程（抖音属"收件箱→raw→wiki"的一环）
- [[wiki/AGENTS.md]] — 操作契约（raw 只读、review_status 不得设 reviewed 等）

## 待补充

- [ ] 把 9s 固定等待改成"监听 `Network.loadingFinished` / 轮询 aweme_detail"以提升稳定性
- [ ] 评估把 CDP 兜底直接合进 douyin-obsidian skill（替换/补充现有 Playwright 兜底），形成官方可复用方案
- [ ] 记录一份"浏览器调试端口启动 → 跑脚本 → 收尾关端口"的标准操作清单，降低每次重复成本
