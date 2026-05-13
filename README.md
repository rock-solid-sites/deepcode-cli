# deepcode-modes

> **⚠️ Archived.** This fork was an experiment that has concluded.
> Substantive contributions are being offered upstream:
> - [PR: systemPromptFile config key](https://github.com/lessweb/deepcode-cli/pull/N)
> - [PR: English translations of docs](https://github.com/lessweb/deepcode-cli/pull/N)
>
> See upstream at https://github.com/lessweb/deepcode-cli

A fork of [Deep Code CLI](https://github.com/lessweb/deepcode-cli) that adds
system-prompt replacement and a behavioral tuning layer modeled on
[claude-code-modes](https://github.com/nklisch/claude-code-modes).

Deep Code is a terminal AI coding assistant optimized for the `deepseek-v4`
model family, with support for deep thinking, reasoning effort control, and
Agent Skills. This fork preserves all of those capabilities and adds the
ability to replace the system prompt — letting you control how Deep Code
behaves on a per-task basis the way Claude Code Modes does for Claude Code.

## Status

**Archived.** The fork's substantive work (systemPromptFile feature, English
doc translations) is being offered upstream as separate pull requests. No
further development is planned on this fork.

## Installation

```sh
npm install -g @vegamo/deepcode-cli
```

(Installation will switch to the fork's own npm package once the new
features are implemented and the package is published. For now, install
upstream and treat this repo as the source-of-truth for ongoing fork
work.)

Run `deepcode` inside any project directory to start.

## Configuration

Create `~/.deepcode/settings.json`:

```json
{
  "env": {
    "MODEL": "deepseek-v4-pro",
    "BASE_URL": "https://api.deepseek.com",
    "API_KEY": "sk-..."
  },
  "thinkingEnabled": true,
  "reasoningEffort": "max"
}
```

The configuration file is shared with the [Deep Code VSCode
extension](https://github.com/lessweb/deepcode) — configure once, use
everywhere.

See [docs/configuration.md](docs/configuration.md) for the full
configuration reference, and [docs/mcp.md](docs/mcp.md) for MCP server
setup.

## Key Features

### Skills

Deep Code supports agent skills:

- **User-level Skills**: discovered and activated from `~/.agents/skills/`.
- **Project-level Skills**: loaded from `./.agents/skills/` for
  project-specific workflows, with legacy `./.deepcode/skills/`
  compatibility.

### MCP Support

Deep Code can connect to external services (GitHub, browsers, file systems,
databases) via the Model Context Protocol. See
[docs/mcp.md](docs/mcp.md) for setup details.

### Optimized for DeepSeek

- Specifically tuned for DeepSeek model performance.
- Reduces costs by using
  [Context Caching](https://api-docs.deepseek.com/guides/kv_cache).
- Natively supports
  [Thinking Mode](https://api-docs.deepseek.com/guides/thinking_mode)
  and Thinking Effort Control.

## What This Fork Added

These features were implemented during the fork's active development cycle
and are now offered upstream:

### System-prompt replacement

A mechanism for replacing Deep Code's base system prompt with operator-
provided content via a `systemPromptFile` config key. The base prompt is
otherwise additive-only — `AGENTS.md` content and matched skills append
after it. This fork made the base prompt configurable via a file path.

### Modes-style behavioral tuning (planned, not implemented)

A CLI wrapper (`deepcode-mode`) that would assemble system prompts from
behavioral axis fragments (agency / quality / scope) and modifiers,
modeled on [claude-code-modes](https://github.com/nklisch/claude-code-modes).
This feature was designed but not implemented before the fork concluded.

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `Enter` | Send the prompt |
| `Shift+Enter` | Insert a newline (also `Ctrl+J`) |
| `Ctrl+V` | Paste an image from the clipboard |
| `Esc` | Interrupt the current model turn |
| `/` | Open the skills / commands menu |
| `/new` | Start a fresh conversation |
| `/resume` | Choose a previous conversation to continue |
| `/skills` | List available skills |
| `/mcp` | Manage MCP server connections |
| `/exit` | Quit Deep Code |
| `Ctrl+D` twice | Quit Deep Code |

## Supported Models

- `deepseek-v4-pro` (recommended)
- `deepseek-v4-flash`
- Any other OpenAI-compatible model

## Relationship to Upstream

This fork was based on [lessweb/deepcode-cli](https://github.com/lessweb/deepcode-cli)
and tracked it for the core Deep Code CLI functionality. Fork-specific
work (systemPromptFile config key, English doc translations) has been
offered upstream as pull requests.

If you're looking for the canonical Deep Code CLI, use upstream directly.

## FAQ

### Does Deep Code have a VSCode extension?

Yes. Deep Code has a [VSCode
extension](https://marketplace.visualstudio.com/items?itemName=vegamo.deepcode-vscode)
that shares the `~/.deepcode/settings.json` configuration file with the
CLI. This fork is not currently being developed to be compatible with
the extension — the fork-specific features target the CLI only.

### Does Deep Code support understanding images?

Deep Code supports multimodal input — you can paste images from the
clipboard with `Ctrl+V`. However, `deepseek-v4` does not currently
support multimodal. For multimodal input, the Volcano Ark
`Doubao-Seed-2.0-pro` model is recommended.

### How do I send a Slack message when a task completes?

Write a shell notification script that calls a Slack webhook, then set
the `notify` field in `~/.deepcode/settings.json` to the full path of
the script. See [docs/configuration.md](docs/configuration.md) for
details.

### How do I enable web search?

Deep Code includes a built-in web search tool that works for most use
cases. To use a custom script instead, set the `webSearchTool` field in
`~/.deepcode/settings.json` to the full path of your script.

### Does it support coding plans (third-party model providers)?

Yes. Set `env.BASE_URL` in `~/.deepcode/settings.json` to any
OpenAI-compatible API endpoint. See
[docs/configuration.md](docs/configuration.md) for examples.

### How do I configure MCP?

See [docs/mcp.md](docs/mcp.md) for full MCP setup instructions.

## Contributing

This fork is archived and no longer accepting contributions. The
substantive work has been offered upstream.

For issues with the underlying Deep Code CLI, please file them with
upstream: <https://github.com/lessweb/deepcode-cli/issues>

## License

MIT. See [LICENSE](LICENSE).

## Acknowledgments

- [Deep Code CLI](https://github.com/lessweb/deepcode-cli) by lessweb —
  the upstream project this fork builds on.
- [claude-code-modes](https://github.com/nklisch/claude-code-modes) by
  nklisch — the behavioral tuning model this fork's planned features
  draw from directly.
