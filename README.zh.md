# dsh-context-mode

[English](README.md)

将 [context-mode](https://github.com/mksglu/context-mode) 通过 `@deepseek-ai/dsh-mcp-client` 挂载为 DeepSeek Harness (DSH) 的 `context_mode` 工具命名空间（`mcp__context_mode__*`）的 bundle 插件。

context-mode 使用本地 SQLite FTS5 沙箱，把大量工具输出挡在 LLM 上下文窗口之外：运行代码和 shell 命令、索引结果，然后只搜索匹配的片段。免费、本地、安装后可离线工作。

## 前置条件

安装 context-mode，使 `context-mode` 在 `PATH` 中可用：

```sh
npm i -g context-mode
```

需要 Node 22.5.0 或更高版本（可用 `context-mode doctor` 检查）。在 Windows 上需要 Git for Windows 提供 Git Bash shell 工具。

## 在 DSH 中安装

```sh
dsh plugin --profile web add github:twentythreenineteen/dsh-context-mode
```

然后在 DSH 插件页面启用 `dsh-context-mode`（或将其加入 profile 的 `package.json` 中的 `dsh.profile.bundles`）。

## 配置

bundle patch 插入 `id: context-mode-mcp` 行，配置如下：

- `serverName`: `context_mode`
- `transport`: `stdio`
- `command`: `context-mode`
- `args`: `[]`（无参数运行 `context-mode` 即在 stdio 上启动 MCP 服务）
- `toolCallTimeoutMs`: `120000`
- `failOnStartupError`: `false`

这会以 `mcp__context_mode__*` 形式提供 11 个工具：`ctx_execute`、`ctx_execute_file`、`ctx_index`、`ctx_search`、`ctx_fetch_and_index`、`ctx_batch_execute`、`ctx_stats`、`ctx_doctor`、`ctx_upgrade`、`ctx_purge`、`ctx_insight`。

如需自定义（例如用显式的 Node 入口替代 PATH shim），在 profile 的 `cordis.patch.yml` 中重述 `id: context-mode-mcp` 的完整 `config`：

```yaml
- id: context-mode-mcp
  config:
    serverName: context_mode
    transport: stdio
    command: node
    args:
      - C:/Users/you/AppData/Roaming/npm/node_modules/context-mode/cli.bundle.mjs
    toolCallTimeoutMs: 120000
    failOnStartupError: false
```

## 许可证

MIT
