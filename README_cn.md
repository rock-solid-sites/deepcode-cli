# deepcode-modes

这是 [Deep Code CLI](https://github.com/lessweb/deepcode-cli) 的一个分支，增加了系统提示词替换以及基于 [claude-code-modes](https://github.com/nklisch/claude-code-modes) 的行为调优层。

Deep Code 是一个终端 AI 编程助手，针对 `deepseek-v4` 模型系列进行了优化，支持深度思考、推理强度控制以及 Agent Skills。本分支保留了所有这些能力，并增加了替换系统提示词的功能——让你能够像 Claude Code Modes 为 Claude Code 所做的那样，按任务粒度控制 Deep Code 的行为。

## 状态

开发中。当前代码是上游 Deep Code CLI 的基线版本，分支特有功能正在积极开发。目前，本分支的行为与上游 Deep Code CLI 完全一致，唯一例外是 MCP 配置文档使用英文编写。

## 安装

```sh
npm install -g @vegamo/deepcode-cli
```

（待新功能实现且包发布后，安装将切换到本分支自己的 npm 包。目前请安装上游版本，并将本仓库视为分支开发的真实来源。）

在任何项目目录中运行 `deepcode` 即可开始。

## 配置

创建 `~/.deepcode/settings.json`：

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

该配置文件与 [Deep Code VSCode 扩展](https://github.com/lessweb/deepcode) 共享——一次配置，随处使用。

完整的配置参考请见 [docs/configuration.md](docs/configuration.md)，MCP 服务器设置请见 [docs/mcp.md](docs/mcp.md)。

## 主要特性

### Skills

Deep Code 支持 agent skills：

- **用户级 Skills**：从 `~/.agents/skills/` 发现并激活。
- **项目级 Skills**：从 `./.agents/skills/` 加载，用于项目特定的工作流，同时兼容旧的 `./.deepcode/skills/` 路径。

### MCP 支持

Deep Code 可通过模型上下文协议（Model Context Protocol）连接外部服务（GitHub、浏览器、文件系统、数据库等）。配置详情请见 [docs/mcp.md](docs/mcp.md)。

### 针对 DeepSeek 优化

- 专门针对 DeepSeek 模型性能进行了调优。
- 通过使用[上下文缓存](https://api-docs.deepseek.com/guides/kv_cache)降低成本。
- 原生支持[思考模式](https://api-docs.deepseek.com/guides/thinking_mode)和思考强度控制。

## 本分支新增功能

这些功能正在积极开发中。进度请关注本仓库的提交历史。

### 系统提示词替换

提供一种机制，可以用用户提供的内容替换 Deep Code 的基础系统提示词。原始提示词默认只能追加——`AGENTS.md` 内容和匹配的 skills 会附加在后面。本分支使基础提示词本身变为可配置。

### Modes 风格的行为调优

一个 CLI 包装器，从行为轴片段（主动性 / 质量 / 范围）和修饰符组合生成系统提示词。本分支直接从 [claude-code-modes](https://github.com/nklisch/claude-code-modes) 导入轴片段、修饰符和预设，并根据 Deep Code 与 Claude Code 工具面的差异进行调整。预设（`safe`、`create`、`extend`、`refactor`、`explore`、`none`）与 claude-code-modes 的集合保持一致，使得两个工具的工作词汇相同。

## 键盘快捷键

| 按键 | 操作 |
|---|---|
| `Enter` | 发送提示词 |
| `Shift+Enter` | 插入换行（也可用 `Ctrl+J`） |
| `Ctrl+V` | 从剪贴板粘贴图片 |
| `Esc` | 中断当前模型响应 |
| `/` | 打开 skills / 命令菜单 |
| `/new` | 开始新对话 |
| `/resume` | 选择之前的对话继续 |
| `/skills` | 列出可用 skills |
| `/mcp` | 管理 MCP 服务器连接 |
| `/exit` | 退出 Deep Code |
| `Ctrl+D` 两次 | 退出 Deep Code |

## 支持的模型

- `deepseek-v4-pro`（推荐）
- `deepseek-v4-flash`
- 任何其他兼容 OpenAI API 的模型

## 与上游的关系

本分支跟踪 [lessweb/deepcode-cli](https://github.com/lessweb/deepcode-cli) 的核心 Deep Code CLI 功能。上游的改进会定期合并进来。分支特有的工作（系统提示词替换、Modes 风格行为调优）仅存在于本仓库。

如果你想要的是没有本分支新增功能的权威 Deep Code CLI，请直接使用上游版本。

## 常见问题

### Deep Code 有 VSCode 扩展吗？

有的。Deep Code 有一个 [VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=vegamo.deepcode-vscode)，它与 CLI 共享 `~/.deepcode/settings.json` 配置文件。本分支目前尚未开发为兼容该扩展——分支特有功能仅针对 CLI。

### Deep Code 支持理解图片吗？

Deep Code 支持多模态输入——你可以用 `Ctrl+V` 从剪贴板粘贴图片。但是，`deepseek-v4` 目前不支持多模态。如需多模态输入，推荐使用火山引擎的 `Doubao-Seed-2.0-pro` 模型。

### 任务完成后如何发送 Slack 消息？

编写一个调用 Slack webhook 的 shell 通知脚本，然后在 `~/.deepcode/settings.json` 中将 `notify` 字段设置为该脚本的完整路径。详情请见 [docs/configuration.md](docs/configuration.md)。

### 如何启用网页搜索？

Deep Code 内置了一个网页搜索工具，可满足大多数使用场景。如果想改用自定义脚本，请将 `~/.deepcode/settings.json` 中的 `webSearchTool` 字段设置为脚本的完整路径。

### 支持编码计划（第三方模型提供商）吗？

支持。将 `~/.deepcode/settings.json` 中的 `env.BASE_URL` 设置为任何兼容 OpenAI API 的端点即可。示例请见 [docs/configuration.md](docs/configuration.md)。

### 如何配置 MCP？

完整的 MCP 设置说明请见 [docs/mcp.md](docs/mcp.md)。

## 贡献

欢迎提交 issue 和 pull request。对于分支特有功能，请在提交 PR 之前先在 issue 中讨论——本项目有明确的发展方向，并非所有新增功能都适合。

对于底层 Deep Code CLI 的问题（非分支特有功能），请向上游提交：
<https://github.com/lessweb/deepcode-cli/issues>

## 许可证

MIT。请参见 [LICENSE](LICENSE)。

## 致谢

- [Deep Code CLI](https://github.com/lessweb/deepcode-cli) by lessweb —— 本分支所基于的上游项目。
- [claude-code-modes](https://github.com/nklisch/claude-code-modes) by nklisch —— 本分支计划功能所直接借鉴的行为调优模型。