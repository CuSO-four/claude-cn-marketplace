<div align="center">

# claude-cn-marketplace

**中文社区精选的 Claude Code 资源库 —— MCP 服务器与 Skills 一键安装**

本仓库是一个标准的 **Claude Code Plugin Marketplace**。添加后，你可以在 Claude Code 里直接浏览并安装其中收录的插件（含 MCP 服务器与 Skills），全部内容可追溯、可审计。

</div>

---

## ⚠️ 信任与安全提示（先读）

- 插件可能包含 **Skills 指令、MCP 服务器配置、乃至可执行代码**。安装任何插件前，请先浏览该插件的来源仓库与本仓库中对应的说明文件。
- 本仓库**只收录、不修改**上游内容：远程条目以固定 commit（sha）直接引用上游仓库，本地条目标注原始出处。收录 ≠ 背书，请自行评估第三方来源。
- 涉及凭据的配置一律使用 `${ENV_VAR}` 占位，不会（也不应）出现真实 Key。

---

## 快速开始

### 1. 添加本 marketplace（一次即可）

```bash
claude plugin marketplace add CuSO-four/claude-cn-marketplace
```

### 2. 安装插件

```bash
# 命令行方式
claude plugin install <插件名>@claude-cn-marketplace

# 或图形化方式：在 Claude Code 中输入 /plugin → Discover 里浏览安装
```

### 3. 安装 MCP 服务器

- marketplace 内带 MCP 的插件（如 github-remote、context7）装完即注册工具（`mcp__plugin_…` 前缀），重启会话生效；
- 其它精选 MCP（官方参考实现、需 API Key 的服务等）见 **[MCP 精选清单](mcp/README.md)**，每条都有一行 `claude mcp add` 命令。

---

## 收录清单

### 插件（Plugins / Skills）

| 插件 | 类型 | 说明 | 来源 | 安装 |
|---|---|---|---|---|
| [context7](plugins/context7/README.md) | MCP（远程 HTTP） | 10 万+ 开源库最新文档检索，**零配置**、匿名可用，防"过时 API" | [context7.com](https://context7.com) | `claude plugin install context7@claude-cn-marketplace` |
| [github-remote](plugins/github-remote/README.md) | MCP（远程 HTTP） | GitHub 官方 MCP 服务器：仓库 / Issue / PR / 代码搜索 | [github/github-mcp-server](https://github.com/github/github-mcp-server) | `claude plugin install github-remote@claude-cn-marketplace` |
| zh-office | Skills（docx/pdf/pptx/xlsx） | Anthropic 官方办公文档技能四件套：让 Claude 直接创建编辑 Word / PDF / PowerPoint / Excel | [anthropics/skills](https://github.com/anthropics/skills) | `claude plugin install zh-office@claude-cn-marketplace` |
| desktop-commander | MCP（本地 stdio） | 终端命令、进程、文件操作：让 Claude 安全地操作你的电脑 | [DesktopCommanderMCP](https://desktopcommander.app) | `claude plugin install desktop-commander@claude-cn-marketplace` |

> 插件内的 MCP 服务器装好后，工具以 `mcp__plugin_<插件>_<服务器>__<工具>` 形式出现；想用普通 `claude mcp add` 形式（不带插件前缀）则参考 [MCP 精选清单](mcp/README.md)，两者效果等价、二选一即可。

### MCP 精选（claude mcp add 直装）

见 **[mcp/README.md](mcp/README.md)** —— 收录官方参考实现 6 个（filesystem/memory/fetch/git/time/sequential-thinking）、官方远程 3 个（GitHub/Context7/Notion）、服务型 6 个（github-stdio/firecrawl/exa/apify/playwright/antvis 图表），按"是否需要 Key"分级，每条附安装命令与中文说明。

---

## 这个仓库是什么原理？

Claude Code 支持从 GitHub 仓库安装插件资源，分两步：

1. **添加 marketplace**：`claude plugin marketplace add CuSO-four/claude-cn-marketplace` 会把本仓库克隆到 `~/.claude/plugins/marketplaces/claude-cn-marketplace/` 并读取 `.claude-plugin/marketplace.json` 里的目录；
2. **安装插件**：`/plugin` 浏览器或 `claude plugin install` 会按条目里的 `source` 安装——本地条目直接可用；远程条目按固定 commit（sha）拉取对应仓库的子目录，**不随上游变动而漂移**。

你（或任何用户）也可以 fork 本仓库按 [CONTRIBUTING.md](CONTRIBUTING.md) 收录新条目，或直接提 PR。

## 常见问题

**Q: 添加 marketplace 或安装时网络很慢？**
A: marketplace 安装需要从 GitHub 拉取本仓库/上游仓库。网络受限地区请自行配置代理或镜像后重试；装一次即可长期使用。

**Q: 上游更新了，我收录的条目会过时吗？**
A: 远程条目固定了 commit（sha），不会"悄悄漂移"；sha 过期后安装会失败而不是装到错误版本。仓库维护者按 [CONTRIBUTING.md](CONTRIBUTING.md) 刷新 sha 即可（欢迎 PR 提醒）。

**Q: 插件和 `claude mcp add` 有什么区别？**
A: 插件方式随插件整体启用/禁用、工具带 `mcp__plugin_…` 前缀、可批量分发；`claude mcp add` 直接注册到你的 MCP 配置（`--scope user` 则全局）。MCP 想"跟着项目走"用 `.mcp.json`（可提交 git）；想"跟着人走"用插件或 user scope。

**Q: 想收录一个新插件/技能？**
A: 见 [CONTRIBUTING.md](CONTRIBUTING.md) —— 结构合规 + sha 固定 + 中文说明即可。

---

## 许可

本仓库（marketplace 配置、文档、收录结构）采用 [MIT License](./LICENSE)。
各收录条目的内容与配置归其上游作者所有，出处见各条目说明与"收录清单"表格。
