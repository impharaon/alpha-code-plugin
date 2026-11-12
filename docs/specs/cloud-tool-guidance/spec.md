---
id: SPEC-cloud-tool-guidance
title: Claude Code 云工具使用与故障指引
summary: 插件用户按服务端实时说明填写任务，并获得准确的额度、失败、取消和产物指引。
kind: spec
status: approved
version: 1
approved_by: backfill
approved_at: 2026-11-12
owners: [owner]
last_reviewed: 2026-11-12
---

# Claude Code 云工具使用与故障指引

插件的命令与技能向用户解释如何派发、查询和停止云任务，避免把额度耗尽误说成充值即可恢复，也避免让用户照抄已过时的任务格式。

## 来源

- [impharaon/alpha-work:INT-cloud/I-12](https://github.com/impharaon/alpha-work/blob/a4b8beeac0196f02780a8c26364529b07e313acb/docs/intents/INT-cloud.md#意图点)。
- 原跨仓结果：[alpha-work#118](https://github.com/impharaon/alpha-work/issues/118) 的 T12 插件示例；本仓 [#13](https://github.com/impharaon/alpha-code-plugin/issues/13)、[#10](https://github.com/impharaon/alpha-code-plugin/issues/10)。
- [#10 最终订正](https://github.com/impharaon/alpha-code-plugin/issues/10#issuecomment-5788995691)明确撤回了原票的错误载荷断言。
- 合并实现：[PR #11](https://github.com/impharaon/alpha-code-plugin/pull/11)、[PR #12](https://github.com/impharaon/alpha-code-plugin/pull/12)、[PR #15](https://github.com/impharaon/alpha-code-plugin/pull/15)。
- 当前文件：[用法技能](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/skills/codepuppy/SKILL.md)、[派发命令](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/commands/dispatch.md)、[状态命令](https://github.com/impharaon/alpha-code-plugin/blob/94bf64019d4c5e65d545719a098c7e4cb0170329/commands/status.md)。

## 用户与范围

- 用户：通过 Claude Code 使用 Code Puppy 云工具的用户。
- 核心路径：读取当次工具说明 → 填写任务 → 查询状态与产物；失败时按实际原因说明恢复办法。
- 包括：任务示例、工具用途、额度耗尽说明、模型面和云任务面的区别、产物描述符说明。
- 不包括：在插件内实现云任务或计费、维护第二份 kind 清单、承诺每个云任务都能成功。
- 假设与依赖：服务端工具说明是参数和 kind 的权威；账户规则由平台合同持有。
- 范围取舍：#10 明确“不描述未端到端跑过的失败载荷精确形状”；这项限制继续保留。#13 的历史 `cloud_dispatch` 在后续命名迁移后为 `dispatch`。

## 体验约定

- 用户路径与文案由上述技能和命令正文承载，无独立界面设计稿。
- `queued/running` 告知 phase；完成时取得产物；失败或取消时呈现真实错误和状态。
- 额度耗尽时说明等待窗口重置或显式打开钱包溢出，不能暗示单纯充值会恢复服务。

## 业务规则

- **BR-01** 调用参数与 kind 使用当次服务端工具说明，插件示例与该结构保持一致。—— impharaon/alpha-work:INT-cloud/I-12；#13。
- **BR-02** 额度、错误及产物说明不得与平台既有行为相反；模型 HTTP 402 与云任务失败终态分开表述。—— impharaon/alpha-work:INT-cloud/I-12；#10 最终订正。

## 用户会碰到的限制

| 用户碰到什么 | 多做、多懂什么 | 用户能接受吗 | 换来什么 | 出处 |
| --- | --- | --- | --- | --- |
| 订阅额度耗尽后无法继续 | 等待额度窗口重置，或显式启用钱包溢出 | 沿用平台既有计费边界，不以文档补录扩大付费授权 | 避免默认扣钱包或诱导无效充值 | #10 最终订正及现有技能「额度用完时怎么说」 |
| 产物不会直接内联所有字节 | 按描述符的取回地址认证下载 | 既有产物合同；本轮不加新步骤 | 插件说明与实际返回一致 | #10 最终订正 |

## 验收

这些编号属于本能力规格；来源列保留原票编号与最终修订。文本事实由当前仓库文件核对，未把文案检查扩称为云端运行验收。

| ID | Given / When / Then | 依据 |
| --- | --- | --- |
| AC-01 | 插件技能文案里引用云端派发说明的示例使用新形态(kind 单独成行、字段以 `input.` 前缀书写)，不再出现 `<kind>.<field> = …` 旧示例。 | #13 AC1、AC2；PR #15；当前技能「任务信封」 |
| AC-02 | 额度耗尽的指引不再说充值可恢复，改为等额度窗口重置或在账户页打开钱包溢出开关。 | #10 退出条件1；PR #11；当前状态命令与技能 |
| AC-03 | 模型面保留 HTTP 402；云任务派发入口不因额度耗尽而拒绝，运行中额度耗尽按任务失败说明；不编写未经实测的精确失败载荷承诺。 | [#10 对退出条件2的最终订正](https://github.com/impharaon/alpha-code-plugin/issues/10#issuecomment-5788995691) |
| AC-04 | 常用工具说明覆盖派发、状态、有界等待、产物、取消和云搜索；kind 与字段读取当前工具说明，不手抄固定清单。 | #10 退出条件3；PR #12；当前技能 |
| AC-05 | 产物返回结果及描述符，不宣称字节内联；文件内容按描述符地址认证取回。 | #10 最终订正；当前技能「进度与产物」 |

## 待决

无。本规格重建已交付的插件文案义务；云端实际权限、费用和跨客户端验收继续由所属平台规格与原有交付票承担。

## 批准记录

| 版本 | 日期 | 自查(模型) | 批准人 | 原话或链接 | 遗留去向 |
| --- | --- | --- | --- | --- | --- |
| 1 | 2026-11-12 | Codex / GPT-6，逐条对照最终订正与当前文案 | backfill | [本轮补录授权 U-01](https://github.com/impharaon/alpha-work/blob/a4b8beeac0196f02780a8c26364529b07e313acb/docs/intents/originals/INT-history-backfill.chat.md#u-01)，仅重建已交付文案 | 平台运行验收沿原票，无新增事项 |
