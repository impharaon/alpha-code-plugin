# alpha-code-plugin document index

<!-- documentation-contract: v4 -->

## Authority

- Active work: [Repository Issues](https://github.com/impharaon/alpha-code-plugin/issues)
  and [Alpha Delivery](https://github.com/users/impharaon/projects/2).
- Durable portfolio intent: [`GOALS.md`](../GOALS.md) and
  [Alpha goals](https://github.com/impharaon/alpha-work/blob/main/GOALS.md).
- Documentation rules: [Alpha Documentation Contract](https://github.com/impharaon/alpha-work/blob/main/governance/documentation-standard.md).
- Repository doc-governance rules: [`AGENTS.md`](../AGENTS.md).

The root [`README.md`](../README.md) is the current installation and security
guide. The plugin manifest is version metadata; this repository has no tag or
GitHub Release history yet, so it does not claim a release changelog. Plugin and
Worker contracts live beside their implementation until a
standalone durable document is needed. The [module map](architecture/modules.md) records which plugin files own each entry point. Release verification must link its Issue
or PR. Do not create local requirements, Sprint plans, `latest/`, or `done/`.

## 文档索引

意图统一在 [Alpha intents](https://github.com/impharaon/alpha-work/blob/a4b8beeac0196f02780a8c26364529b07e313acb/docs/intents/README.md)；本仓能力规格见 [specs/](specs/)。规格保留已交付事实和验收边界；当前开发状态仍看 Issues / Project。

<!-- docs-index:start -->

<!-- 由 docs_index.py 从各文件 frontmatter 生成,不要手改;改文件的 summary 后重跑 --write。 -->

### architecture

| id | 文件 | 做什么 | 状态 |
| --- | --- | --- | --- |
| `ARCH-modules` | [architecture/modules.md](architecture/modules.md) | 标明插件安装清单、命令与用法 skill 的职责，以及它们与云平台的边界。 | active |

### specs

| id | 文件 | 做什么 | 状态 |
| --- | --- | --- | --- |
| `SPEC-claude-code-installation` | [specs/claude-code-installation/spec.md](specs/claude-code-installation/spec.md) | 用户安装 Code Puppy 插件并单独连接 MCP 服务，获得统一名称的命令、技能和云工具。 | approved |
| `SPEC-cloud-tool-guidance` | [specs/cloud-tool-guidance/spec.md](specs/cloud-tool-guidance/spec.md) | 插件用户按服务端实时说明填写任务，并获得准确的额度、失败、取消和产物指引。 | approved |

<!-- docs-index:end -->
