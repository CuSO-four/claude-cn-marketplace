# github-remote（GitHub 官方 MCP，远程版）

> 由 [claude-cn-marketplace](https://github.com/CuSO-four/claude-cn-marketplace) 社区收录；插件内容与 MCP 配置来自 Anthropic 官方插件目录（`anthropics/claude-plugins-public`）中的 github 插件。

GitHub 官方 MCP 服务器的**远程 HTTP 模式**：不需要在本地安装二进制，通过官方托管的 `https://api.githubcopilot.com/mcp/` 端点工作。

## 能做什么

- 仓库管理：创建/查看仓库、分支、文件
- Issue / Pull Request 全流程：创建、评论、审查、合并
- 代码搜索与仓库搜索
- 与 GitHub API 的完整交互

## 安装

### 第一步：添加本 marketplace（只需一次）

```bash
claude plugin marketplace add CuSO-four/claude-cn-marketplace
```

### 第二步：安装插件

```bash
claude plugin install github-remote@claude-cn-marketplace
```

或输入 `/plugin` 打开插件浏览器，选择 `github-remote` 安装。

### 第三步：配置 Token

插件内的 MCP 配置会读取 `GITHUB_PERSONAL_ACCESS_TOKEN` 环境变量。任选一种方式：

1. **设置系统环境变量**（推荐，重启 Claude Code 生效）
   ```bash
   # Windows（PowerShell）
   setx GITHUB_PERSONAL_ACCESS_TOKEN "ghp_你的_token"
   ```
2. 或启动 Claude Code 前在会话中导出该变量

Token 需要至少 `repo`（仓库读写）权限；只想读公开数据可勾选 `public_repo`。在 GitHub → Settings → Developer settings → Personal access tokens 创建。

## 常见问题

**Q: 安装后工具没生效？**
A: 重启 Claude Code 会话，并用 `claude mcp list` 确认 `github-remote` 的连接状态。

**Q: 网络访问该远程端点不稳定？**
A: 可改用自建 stdio 版（本机运行 github-mcp-server 二进制 + PAT），参见仓库根目录 [MCP 精选清单](../../mcp/README.md) 中的 github-mcp-server 条目。

## 安全提示

- 插件内的 MCP 服务器是**第三方托管的远程端点**，连接前请确认你的 Token 只授予必要的最小权限
- 本 marketplace 只做收录与中文说明，不修改上游配置；MCP 端点由 GitHub 官方运营
