---
id: SPEC-claude-code-installation
title: Claude Code 插件安装与云工具命名
summary: 用户安装 Code Puppy 插件并单独连接 MCP 服务，获得统一名称的命令、技能和云工具。
kind: spec
status: approved
version: 1
approved_by: backfill
approved_at: 2026-11-12
owners: [owner]
last_reviewed: 2026-11-12
---

# Claude Code 插件安装与云工具命名

Claude Code 用户按安装说明取得 Code Puppy 命令、技能和云工具。插件不再捆绑 MCP 服务，避免模型看到重复的品牌前缀；漏配服务时，说明会引导用户完成连接。

## 来源

- [impharaon/alpha-work:INT-cloud/I-11](https://github.com/impharaon/alpha-work/blob/a4b8beeac0196f02780a8c26364529b07e313acb/docs/intents/INT-cloud.md#意图点)。
- 原跨仓结果：[alpha-work#118](https://github.com/impharaon/alpha-work/issues/118)，T05 插件消费者；本仓完成票 [#14](https://github.com/impharaon/alpha-code-plugin/issues/14)。沿用原父级，不另建同义 Story。
- 合并实现：[PR #16](https://github.com/impharaon/alpha-code-plugin/pull/16)、[PR #17](https://github.com/impharaon/alpha-code-plugin/pull/17)；[最终验收](https://github.com/impharaon/alpha-code-plugin/issues/14#issuecomment-6648632006)。
- 历史第一次验收被撤回，原因及 owner 原话见[重开记录](https://github.com/impharaon/alpha-code-plugin/issues/14#issuecomment-6648438308)。
- 当前文件：[安装说明](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/README.md#安装)、[插件清单](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/.claude-plugin/plugin.json)、[市场清单](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/.claude-plugin/marketplace.json)。

## 用户与范围

- 用户：接入 Code Puppy 云端服务的 Claude Code 用户；实际用户数未量。
- 核心路径：设置密钥环境变量 → 安装 marketplace 与插件 → 添加用户级 MCP 服务 → 调用云工具。
- 包括：插件、命令、技能及环境变量的统一命名；用户级 MCP 配置；缺少工具时的安装指引。
- 不包括：云端任务或账户逻辑、插件仓库改名、模型网关的全账号兼容性。
- 假设与依赖：可用的 Claude Code、允许云工具的凭证及可访问的服务入口。历史取证使用 Claude Code 2.1.318，不等于所有后续版本都已验证。
- 范围取舍：恢复独立 MCP 配置是 #14 的最终修订；早期捆绑配置导致重复前缀的方案不再是当前范围。服务端域名和仓库名在原判据中明确豁免。

## 体验约定

- 用户路径与提示：[README 的安装命令](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/README.md#安装)和[缺工具时的指引](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/skills/codepuppy/SKILL.md)为现有文本入口。
- 成功状态：连接后模型可见 `mcp__codepuppy__*`；缺少或错误密钥不能连通。实际派发与取消按服务器结果呈现。
- 文案和设计稿：本功能是 CLI 安装及文字说明，没有独立 UI 设计稿；不回补虚构的历史画面。

## 业务规则

- **BR-01** 插件与命令使用 Code Puppy 名称，云工具使用独立用户级 MCP 服务的命名空间。—— impharaon/alpha-work:INT-cloud/I-11；#14 最终修订。
- **BR-02** MCP 配置保存环境变量引用而非密钥值；无有效凭证时连接失败。—— #14 实测记录与当前安装命令。

## 用户会碰到的限制

| 用户碰到什么 | 多做、多懂什么 | 用户能接受吗 | 换来什么 | 出处 |
| --- | --- | --- | --- | --- |
| 安装插件后还要添加 MCP 服务 | 多执行一条安装命令，并设置环境变量 | #14 的最终修订采用此方式；不补写 owner 对额外步骤的单独评价 | 满足 owner 明确提出的工具名不能重复品牌前缀 | [重开与修订](https://github.com/impharaon/alpha-code-plugin/issues/14#issuecomment-6648438308) |

## 验收

保留 #14 的原 AC 编号。AC-01 的“服务器键”指最终修订后的用户级 MCP 服务；AC-03 的豁免和 AC-04 的最终实测以所引裁决为准。

| ID | Given / When / Then（历史要求） | 依据 |
| --- | --- | --- |
| AC-01 | marketplace 名、插件名、MCP 服务器键均为 `codepuppy`;凭据变量为 `CODEPUPPY_API_KEY`;技能目录为 `skills/codepuppy/`;命令命名空间为 `/codepuppy:*`。 | [#14](https://github.com/impharaon/alpha-code-plugin/issues/14)；PR #16、#17 |
| AC-02 | 说明、技能与命令文本中的云端工具名使用新短名(`dispatch`、`status`、`await`、`artifacts`、`cancel`、`web_search` 等),密钥示例使用新前缀 `sk-codepuppy-`。 | #14 AC2；PR #16 |
| AC-03 | 仓内对旧名(`alpha-cloud`、`alpha-code-plugin`、`ALPHA_API_KEY`、`cloud_dispatch` 一类、`sk-alpha-`)零命中。 | #14 AC3；[2026-11-04 裁决](https://github.com/impharaon/alpha-code-plugin/issues/14#issuecomment-6647627352)豁免服务端域名与仓库名，禁止扩大豁免 |
| AC-04 | 在 Claude Code 中安装后,`tools/list` 出现 `mcp__codepuppy__dispatch`,并能完成一次派发。 | [最终安装、派发及取消证据](https://github.com/impharaon/alpha-code-plugin/issues/14#issuecomment-6648632006)，替代已撤回的第一次完成声明 |

## 待决

无新增核心范围决策。本次核对文件和历史证据，未重跑付费调用、安装或生产连通性；历史记录中的代理首次超时及可选模型兼容性不扩为本规格的新承诺。

## 批准记录

| 版本 | 日期 | 自查(模型) | 批准人 | 原话或链接 | 遗留去向 |
| --- | --- | --- | --- | --- | --- |
| 1 | 2026-11-12 | Codex / GPT-6，历史票、最终修订、合并记录与当前文件自查 | backfill | [本轮补录授权 U-01](https://github.com/impharaon/alpha-work/blob/a4b8beeac0196f02780a8c26364529b07e313acb/docs/intents/originals/INT-history-backfill.chat.md#u-01)；重建已交付行为，不新增产品决定 | 原 #14 的已知限制保留；无新增工作 |
