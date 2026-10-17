# 项目长期记忆

## 工作方式偏好（用户明确要求，2026-10-17）

- **先思路后产物**：任何任务都不要直接产出最终文件——先在对话里给出思路、方案、产物大纲/预览，聊到用户确认没问题之后再落地生成。适用于文档、报告、简历、面试准备等一切产物类任务。
- 用户惯用苏格拉底式梳理：一次只问一个问题，每条问题前先说明上一条判断的更新；回答前先复述理解确认再动手。

## 关键约束（必须遵守）

- 本仓库同时由 **Claude（Claudian 插件）** 管理。我（WorkBuddy）工作时**绝对不得修改**以下内容：
  - `.claude/` — Claude Code 的 AI 配置（settings.json / settings.local.json）
  - `.claudian/` — Claudian 插件运行时数据（`sessions/` 历史会话记录、`backups/` 备份、`claudian-settings.json`）
  - `.obsidian/` — Obsidian 的配置与插件
- 工作规则见 `AGENTS.md`，需 100% 遵守：`raw/` 只读；`wiki/` 全权维护；`review_status` 绝不可设为 `reviewed`（仅人工可设）；每次改动需更新 `wiki/index.md` 与 `wiki/log.md`。

## 本机环境坑
- WorkBuddy 内置 Python：bash 中 `python` 指向 versions/3.13.12 基础环境，`pip` 指向 envs/default 环境，两者不一致；pip 装的包要用 C:/Users/Lenovo/.workbuddy/binaries/python/envs/default/Scripts/python.exe 才能导入。POSIX 路径 /c/... 传给原生 python 会失败，须用 C:/ 形式
