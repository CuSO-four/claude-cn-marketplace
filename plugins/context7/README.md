# context7（文档检索 MCP，hosted 零配置）

> 由 [claude-cn-marketplace](https://github.com/CuSO-four/claude-cn-marketplace) 社区收录；服务器由 [Upstash](https://context7.com) 官方托管。

让 Claude 查询 **10 万+ 开源库**（PyPI、npm、Maven、Go、Rust 等）的**当前版本官方文档**，回答与具体版本匹配的 API 用法 —— 避免模型"凭记忆"写出过时用法。国内可正常访问，无需本地运行任何进程，**开箱即用**。

## 安装

### 第一步：添加本 marketplace（只需一次）

```bash
claude plugin marketplace add CuSO-four/claude-cn-marketplace
```

### 第二步：安装插件

```bash
claude plugin install context7@claude-cn-marketplace
```

或输入 `/plugin` 打开插件浏览器，选择 `context7` 安装。无需配置任何 Token。

## 使用示例

向 Claude 提问时自然触发，例如：

- "用最新版 FastAPI 的推荐方式写一个带依赖注入的接口"
- "React 19 里 `<Form>` action 的正确用法"
- "pandas 2.x 中 `pd.read_csv` 的 dtype_backend 参数"

## 常见问题

**Q: 免费额度够用吗？**
A: 匿名使用有速率限制；注册 context7.com 获取 `CONTEXT7_API_KEY` 可提高限额（在系统环境变量设置后重启 Claude Code）。

**Q: 安装后没生效？**
A: 重启会话并用 `claude mcp list` 查看 `context7` 连接状态。

## 安全提示

- 该 MCP 是远程托管服务：你的代码、文件内容不会被发送，只有文档查询请求会离开本机
- 服务器与配置由第三方（Upstash）运营，安装前请自行评估
