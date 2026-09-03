# MCP 精选清单（中文）

> 结构化数据见同目录 [registry.json](./registry.json)（两者需同步维护）。
> 安装命令均在 **Claude Code 终端**执行。每条命令执行后需**重启会话**生效。

## 快速选择指南

| 你的需求 | 选它 | 配置 |
|---|---|---|
| 让 Claude 读写本地文件 | filesystem | 零配置（给个目录） |
| 网页抓取转干净文本 | fetch / firecrawl | fetch 零配置；firecrawl 需 Key |
| 跨会话长期记忆 | memory | 零配置（建议设 `MEMORY_FILE_PATH` 绝对路径） |
| 查最新版库文档，防过时 API | context7 | 零配置 |
| 操作 GitHub | github-remote（OAuth）或 github-stdio（PAT） | 按提示授权 |
| 让 Claude 操作浏览器 | playwright | 先装浏览器内核 |
| 复杂推理拆解 | sequential-thinking | 零配置 |

---

## 一、官方参考实现（本地运行，零 Key）

来源：[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)（官方维护 7 个参考服务器，此处收录其中 6 个）。参考实现适合学习与本地任务，生产环境请自行评估。

| 名称 | 说明 | 安装命令 |
|---|---|---|
| filesystem | 在指定目录范围内读写本地文件（**路径必须是绝对路径**） | `claude mcp add filesystem npx -y @modelcontextprotocol/server-filesystem C:/Users/<你的用户名>/Documents` |
| memory | 持久化知识图谱记忆，跨会话记住偏好与项目背景 | `claude mcp add memory npx -y @modelcontextprotocol/server-memory` |
| fetch | 抓取网页转 Markdown，适合读文档 | `claude mcp add fetch npx -y @modelcontextprotocol/server-fetch` |
| git | Git 只读分析（历史/差异/搜索），需 Python 环境（uvx） | `claude mcp add git uvx mcp-server-git --repository C:/Users/<你的用户名>/projects/my-repo` |
| time | 当前时间与时区转换 | `claude mcp add time npx -y @modelcontextprotocol/server-time` |
| sequential-thinking | 结构化思维链，把复杂问题拆成可见步骤 | `claude mcp add sequential-thinking npx -y @modelcontextprotocol/server-sequential-thinking` |

> ⚠️ **memory 已知坑**：它读取的是 `MEMORY_FILE_PATH`（不是 `MEMORY_FILE`），且务必设为**绝对路径**，否则知识图谱会写进 npx 临时缓存被清理。

---

## 二、官方远程服务器（HTTP，无需本地进程，免升级）

| 名称 | 说明 | 安装命令 |
|---|---|---|
| github-remote | GitHub 官方 MCP 远程版，浏览器 OAuth 授权 | `claude mcp add github --transport http https://api.githubcopilot.com/mcp/` |
| context7 | 10 万+ 开源库文档检索，匿名可用 | `claude mcp add context7 --transport http https://mcp.context7.com/mcp` |
| notion-remote | Notion 官方 MCP 远程版（OAuth） | `claude mcp add notion --transport http https://mcp.notion.com/mcp` |

> 也可通过本仓库 marketplace 的 **context7 / github-remote 插件**安装（`/plugin` 浏览器操作，工具以 `mcp__plugin_…` 前缀命名），与上面 `claude mcp add` 二选一即可。

---

## 三、社区/服务型（需 API Key，或需自建）

> Key 只保存在本机 `~/.claude.json`，不会上传。申请链接见表格。

| 名称 | 说明 | 安装命令 | 所需 Key | Key 申请 |
|---|---|---|---|---|
| github-stdio | GitHub 官方 MCP **自建版**：下载官方预编译二进制本机运行 + PAT。大陆网络下比远程端点更稳的选择 | `claude mcp add github <exe 绝对路径> --env GITHUB_PERSONAL_ACCESS_TOKEN=ghp_你的token` | `GITHUB_PERSONAL_ACCESS_TOKEN`（勾 `repo`；只读公开数据勾 `public_repo`） | [github.com/settings/tokens](https://github.com/settings/tokens) |
| firecrawl | 高质量网页抓取/爬站（JS 渲染、反爬处理） | `claude mcp add firecrawl npx -y firecrawl-mcp` | `FIRECRAWL_API_KEY` | [firecrawl.dev](https://firecrawl.dev) |
| exa | AI 语义搜索与深度研究（网页/论文/公司） | `claude mcp add exa npx -y exa-mcp-server` | `EXA_API_KEY` | [exa.ai](https://exa.ai) |
| apify | 数千现成爬虫与网页自动化 | `claude mcp add apify npx -y apify-mcp-server` | `APIFY_TOKEN` | [apify.com](https://apify.com) |
| playwright | 微软官方浏览器自动化（Chromium） | `claude mcp add playwright npx -y @playwright/mcp` | 无（首次需 `npx playwright install chromium`） | — |
| antvis-chart | 蚂蚁 AntV 图表可视化（25+ 图表，含 skills） | 见上游 README | 视部署方式 | [github.com/antvis/mcp-server-chart](https://github.com/antvis/mcp-server-chart) |

---

## 常用维护命令

```bash
claude mcp list                 # 查看已配置的 MCP 服务器与连接状态
claude mcp get <name>           # 查看单个服务器配置
claude mcp remove <name>        # 移除
claude mcp add <name> ... --scope user   # 全局安装（所有项目可用）
```

配置默认写在当前项目的 `.mcp.json`（可提交 git 共享给队友）或 `~/.claude.json`（用户级）。需要"所有项目都能用"，加 `--scope user`。
