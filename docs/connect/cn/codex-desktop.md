# ChatGPT 应用

[English](../en/codex-desktop.md) · [Русский](../ru/codex-desktop.md) · **中文**

适用于桌面版 ChatGPT 应用：**Codex** 和 **ChatGPT** 标签页。终端请见：[终端中的 Codex](codex-terminal.md)。

`intergalactic-planb/plugins` 仓库必须是公开的。

## 安装

1. 在应用的侧边菜单中打开 **Plugins**。
2. 点击 **Add**，再点击 **Add marketplace**。
3. 在 **Source** 栏输入 `intergalactic-planb/plugins`。**Git ref** 和 **Sparse paths** 留空。点击 **Add marketplace**。
4. 在 **Personal** 标签页的 **Quotalogic Plugins** 部分，点击 `quotalogic` 右侧的 **+**。

## 登录 Quotalogic

1. 点击 **+** 后会打开 Quotalogic 登录页面。登录，检查工作区并确认。
2. 应用会回到插件列表，并显示“Quotalogic plugin installed”。
3. 点击 **Try now**，或打开 **Codex** 或 **ChatGPT** 标签页并询问：“显示我的目录”。会出现您的目录列表。

## 关闭和删除

1. 在 **Plugins** 中打开 Quotalogic 卡片，点击 **Remove**。
2. 如需同时删除 marketplace：**Plugins**，**Store** 标签页，删除 `Quotalogic Plugins`。

## 在 ChatGPT 组织工作区中配置 Quotalogic

如果成员能看到 Quotalogic 技能，但 ChatGPT 无法读取其目录，请为该成员开通独立的 **Quotalogic MCP** 连接。

### 工作区管理员

1. 如果工作区尚未添加插件，请打开 **Admin Console → Plugins → Add → Import marketplace**。在 **Source** 中输入 `https://github.com/intergalactic-planb/plugins`，将 **Path** 留空，在 **Branch, tag, or commit** 中输入 `main`。如果出现提示，请授权访问 GitHub。
2. 在 **Admin Console → Plugins** 中打开 **Quotalogic**。选择 **Available** 允许成员自行安装，或选择 **Installed** 自动安装。
3. 如果尚未添加 **Quotalogic MCP**，请在浏览器中切换到组织工作区，再打开 **Plugins → + → Add custom MCP server**。名称填 `Quotalogic MCP`，测试环境地址填 `https://mcp-staging.quotalogic.io/mcp`，身份验证选择 **OAuth**。点击 **Create as a plugin**，确认 ChatGPT 已找到工具。
4. 在 **Admin Console → Plugins** 中打开 **Quotalogic MCP**，为同一批成员设置 **Installation policy**。**Enterprise/Edu** 支持按角色授权。**Business** 的安装策略适用于整个工作区。

### 工作区成员

1. 切换到组织工作区。在 **Plugins** 中打开组织工作区标签页。如果尚未安装 **Quotalogic**，请先安装。
2. 打开 **Quotalogic MCP**，点击 **Connect**，使用自己的 Quotalogic 账号登录。核对 Quotalogic 工作区并确认授权。
3. 新建聊天，通过 `@` 选择 **Quotalogic MCP**，然后询问“显示我的目录”。如果 ChatGPT 返回了您所在工作区的目录，说明连接正常。

如果技能包标有 **Desktop only**，请在桌面应用中使用该技能包。独立的 **Quotalogic MCP** 连接可在浏览器中提供目录访问。

[导入 marketplace](https://help.openai.com/en/articles/20001504-importing-and-syncing-plugin-marketplaces-from-github) · [在 ChatGPT 中连接 MCP](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)
