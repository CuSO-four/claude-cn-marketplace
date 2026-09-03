# 贡献指南（Contributing）

感谢你愿意扩充这个中文资源库！本仓库的收录遵循几条硬性原则，请先读完再动手。

## 收录原则（宁缺毋滥）

1. **来源可信**：优先收录官方仓库（Anthropic / GitHub / Microsoft 等）、厂商官方、或高星且口碑好的社区项目；来源不明、维护停滞、star 刷量的不收录。
2. **结构必须合规**：要进入 marketplace 的插件必须满足以下任一官方形态：
   - **本地插件**：`plugins/<name>/.claude-plugin/plugin.json` +（可选）`skills/<skill>/SKILL.md`、`.mcp.json`、`agents/` 等；
   - **skill-bundle**（推荐，无需复制内容）：在 `marketplace.json` 用 `source: {url 或 git-subdir} + "strict": false + "skills": [相对路径…]` **直接引用上游仓库**；
   - 上游若本身就是 plugin/marketplace 仓库（有 `.claude-plugin/plugin.json`），用 `git-subdir` 指向其插件目录。
3. **只引用，不复制**：远程条目一律用 `source` 引用上游仓库并 **pin sha**，避免复制带来的许可与同步问题。本地内嵌仅限明确宽松许可（MIT / Apache-2.0）且需要微调适配的内容。
4. **中文说明**：marketplace.json 的 `description` 与插件 README 一律中文（专业名词附英文）。
5. **每条注明出处**：插件 `plugin.json`/README 里写清上游作者与"由 claude-cn-marketplace 社区收录"，并在仓库根 README 的收录表登记。

## 新增一个远程条目的步骤

以收录某仓库 `owner/repo` 的子目录 `path/to/plugin` 为例：

1. 确认该目录含 `.claude-plugin/plugin.json`（或 SKILL.md 目录树）；
2. 取该 ref 的 HEAD commit sha：
   ```bash
   curl -s https://api.github.com/repos/owner/repo/commits/main | grep -o '"sha": "[a-f0-9]*"' | head -1
   ```
   或到 GitHub 网页仓库页复制最新 commit hash；
3. 在 `.claude-plugin/marketplace.json` 的 `plugins[]` 追加：
   ```json
   {
     "name": "my-plugin",
     "description": "中文一句话说明",
     "author": { "name": "上游作者" },
     "category": "development",
     "source": {
       "source": "git-subdir",
       "url": "https://github.com/owner/repo.git",
       "path": "path/to/plugin",
       "ref": "main",
       "sha": "刚取到的 sha"
     },
     "homepage": "https://github.com/owner/repo",
     "tags": ["community-managed"]
   }
   ```
4. 在仓库根 README 的收录清单表格加一行；如有需要写中文说明文档。

## 刷新过期的 sha

上游更新后，引用该 commit 的安装可能失败。修复 = 重新取上游 HEAD sha（同上），更新 `marketplace.json` 后提交即可。远程 source 的 `sha` 是该条目的"安全锚点"，不能省略、不能猜测。

## 新增本地插件

```
plugins/<name>/
├── .claude-plugin/plugin.json   # {"name","description","author":{...}}
├── skills/<skill>/SKILL.md      # 可选：每个技能一个目录（frontmatter 需含 name/description）
├── .mcp.json                    # 可选：MCP 服务器（server 名为键；env 支持 ${ENV_VAR} 占位）
└── README.md                    # 中文说明
```

写完后在 `marketplace.json` 增加条目：`"source": "./plugins/<name>"`。

## 提交

- fork → 分支 → PR 即可；改动后请确保所有 JSON 语法正确。
- 尽量一次 PR 只收录一个条目，方便评审与追溯。

## 安全底线

- 涉及凭据、Token 的内容一律用 `${ENV_VAR}` 占位，**严禁**把真实 Key 提交进仓库；
- 收录带 `.mcp.json` 或可执行代码的插件时，在条目描述里注明来源与用途，并在根 README 的信任提示区登记。
