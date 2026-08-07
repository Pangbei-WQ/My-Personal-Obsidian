---
date: 2026-08-07
tags: [log, llm-wiki]
review_status: reviewed
---

# 操作日志

> Append-only。每次 Ingest / Query 回写 / Lint 操作后追加一条记录。
> 格式：`## [YYYY-MM-DD] 操作类型 | 简要描述`

---

## [2026-08-07] init | LLM Wiki 初始化

创建目录结构、AGENTS.md、index.md、log.md。Vault 现有笔记纳入索引。

**涉及文件：**
- `AGENTS.md`
- `wiki/index.md`
- `wiki/log.md`
- `raw/` 目录树
- `wiki/` 目录树

---

## [2026-08-07] ingest | 全量首次摄入（30+ 源文件 → 5 wiki 页面）

将所有现有笔记归档至 `raw/文档/`，执行首次全量 Ingest。

**来源文件（raw/文档/）：**
- `2026我最常用的AI产品.md`
- `全局工作规则.md`
- `Daily/`（19 篇日记）
- `供应链管理-刘宝红/`（6 本书笔记）
- `任职资格/P5认证材料/`（7 个文件）
- `任职资格/PMC经理任职资格/`（6 个文件）

**新建 wiki 页面：**
- `wiki/AI与工具/2026年AI产品推荐.md` — 14 大类 AI 产品清单
- `wiki/供应链管理/供应链管理知识体系.md` — 核心概念：三个三、三道防线、苹果案例
- `wiki/供应链管理/供应链职业发展路径.md` — 专家三段论、职业选择、认证
- `wiki/任职资格/任职资格体系总览.md` — P5 认证进度 + PMC 经理体系
- `wiki/工作方法/项目工作规则.md` — 个人工作规则摘要

**级联更新：**
- `wiki/index.md` — 重建领域索引和统计
- `wiki/log.md` — 本条记录

**缺失概念（待建）：**
- 个人AI工作流（Claude Code + Codex 详细配置）
- 供应链管理术语表
- 六个书籍详细摘要页面
- PMC 经理能力标准与供应链知识对应关系

**注意事项：**
- 所有新建 wiki 页面状态均为 `unreviewed`，需人工审核后改为 `reviewed`
- 书籍笔记为扫描 OCR 文本，格式较乱，后续可考虑重新整理
- Daily 日记仅归档，未做内容提取——其中有价值的 `#认证候选` 条目待后续提取
