# dsh-context-mode

[中文](README.zh.md)

DeepSeek Harness (DSH) bundle plugin that mounts [context-mode](https://github.com/mksglu/context-mode) via `@deepseek-ai/dsh-mcp-client` under the `context_mode` tool namespace (`mcp__context_mode__*`).

context-mode keeps bulky tool output out of the LLM context window using a local SQLite FTS5 sandbox: run code and shell commands, index results, then search only the matching slices. It is free, local, and works offline after install.

## Prerequisites

Install context-mode so `context-mode` is available on `PATH`:

```sh
npm i -g context-mode
```

Requires Node 22.5.0 or later (verify with `context-mode doctor`). On Windows, Git for Windows is needed for the Git Bash shell tools.

## Install in DSH

```sh
dsh plugin --profile web add github:twentythreenineteen/dsh-context-mode
```

Then enable `dsh-context-mode` in the DSH Plugins page (or add it to `dsh.profile.bundles` in your profile `package.json`).

## Configuration

The bundle patch inserts row `id: context-mode-mcp` with:

- `serverName`: `context_mode`
- `transport`: `stdio`
- `command`: `context-mode`
- `args`: `[]` (running `context-mode` with no args starts the MCP server on stdio)
- `toolCallTimeoutMs`: `120000`
- `failOnStartupError`: `false`

This surfaces 11 tools as `mcp__context_mode__*`: `ctx_execute`, `ctx_execute_file`, `ctx_index`, `ctx_search`, `ctx_fetch_and_index`, `ctx_batch_execute`, `ctx_stats`, `ctx_doctor`, `ctx_upgrade`, `ctx_purge`, `ctx_insight`.

To customize (e.g., pin an explicit Node entry point instead of the PATH shim), restate the full `config` for `id: context-mode-mcp` in your profile's `cordis.patch.yml`:

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

## License

MIT
