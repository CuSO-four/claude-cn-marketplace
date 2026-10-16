# Doc2X MCP Skill

由 claude-cn-marketplace 社区收录，来源为 [NoEdgeAI/doc2x-mcp](https://github.com/NoEdgeAI/doc2x-mcp) 的官方 [Skill](https://github.com/NoEdgeAI/doc2x-mcp/blob/263da5a22f9c64129229a8704505cde85ad5bfc6/skills/doc2x-mcp/SKILL.md)。本目录仅提供中文说明，上游内容按固定 commit 直接引用，不复制或修改。

## 用途

指导 Claude Code 使用 Doc2X MCP tools 解析 PDF、扫描件和图片，导出 Markdown、LaTeX、DOCX，下载结果或提取 PDF v3 layout JSON。

## 安装与配置

`claude plugin install doc2x-mcp@claude-cn-marketplace` 安装的是 Skill 指令。还需按[官方 README](https://github.com/NoEdgeAI/doc2x-mcp#readme) 独立配置基于 stdio 的 MCP Server（npm 包 `@noedgeai-org/doc2x-mcp`，Node.js >=18）并设置自己的 `DOC2X_API_KEY`；安装 Skill 不会自动注册 MCP 工具。

## 许可、费用与数据

- 上游代码使用 MIT 许可，API 用量另行计费，不能将开源代码理解为 API 永久免费。
- 文档解析通过 Doc2X 云端 API 完成。请在使用前确认适合上传的文件范围与服务条款；不要在 issue、配置示例或仓库中放入真实密钥。
- 本条目不提供 API Key，不承诺已替用户运行解析任务。
