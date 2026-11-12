---
id: ARCH-modules
title: Claude Code 插件模块地图
kind: architecture
status: active
summary: 标明插件安装清单、命令与用法 skill 的职责，以及它们与云平台的边界。
owners: [owner]
last_reviewed: 2026-11-10
review_after: 2027-05-09
---

# Claude Code 插件模块地图

| 模块 | 职责 | 可以依赖 | 不许依赖 | 共享概念的权威文件 |
| --- | --- | --- | --- | --- |
| `.claude-plugin/` | 声明 marketplace、插件名称、版本与安装元数据 | Claude Code 插件清单格式 | 云任务执行、账户或计量实现 | [市场清单](../../.claude-plugin/marketplace.json) · [插件清单](../../.claude-plugin/plugin.json) |
| `commands/` | 接受用户参数，派发云任务或查询其结果 | Claude Code 发现的 Code Puppy MCP 工具及工具声明 | 平台内部存储；在本地维护 pipeline 种类清单 | [派发命令](../../commands/dispatch.md) · [状态命令](../../commands/status.md) |
| `skills/codepuppy/` | 给 Claude Code 提供云任务调用和错误说明 | 平台 MCP 工具声明、平台的计费与配额语义 | 在插件内执行平台的计费、额度或任务逻辑 | [用法 skill](../../skills/codepuppy/SKILL.md) |

## 依赖方向

Claude Code 加载本仓的插件清单、命令和用法 skill；命令调用由用户配置的 MCP 服务。
云任务执行和用量归属由 [alpha-platform](https://github.com/impharaon/alpha-platform) 决定。
