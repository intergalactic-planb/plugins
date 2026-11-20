# Claude 应用

[English](../en/claude-desktop.md) · [Русский](../ru/claude-desktop.md) · **中文**

终端中的 `claude` 请见：[终端中的 Claude](claude-terminal.md)。

使用插件需要 Claude 付费方案：Pro、Max、Team 或 Enterprise。`intergalactic-planb/plugins` 仓库必须是公开的。

## 安装

1. 在应用的侧边菜单中打开 **Plugins**。
2. 点击 **Add**，再点击 **Add marketplace**，然后点击 **Add from a repository**。
3. 在 **URL** 栏输入 `intergalactic-planb/plugins`，并选择 **Use "intergalactic-planb/plugins"** 这一行。
4. 在列表中找到 `Quotalogic`，点击 **Add to this computer**。旁边会出现 **Added**。

## 登录 Quotalogic

1. 打开 **Code** 标签页并开始新会话。
2. 输入 `/mcp` 并发送。`/mcp` 后面不要写任何内容。
3. 在 **MCP servers** 窗口中，点击 `quotalogic` 旁的 **Connect**。
4. 应用会询问 “Open mcp-staging.quotalogic.io to authorize quotalogic?”。点击 **OK**。
5. 在浏览器中登录 Quotalogic，检查工作区并确认。
6. 在新会话中询问：“显示我的目录”。会出现您的目录列表。

## 更新

1. **Plugins**，**Add**，**Manage marketplaces**，点击 `quotalogic-plugins` 旁的 **...**，再点击 **Check for updates**。
2. 关闭窗口，在 **Plugins** 中打开 Quotalogic 卡片，点击 **Update**。
3. 重启应用并开始新会话。

## 关闭和删除

在 **Plugins** 中打开 Quotalogic 卡片。右侧的开关可关闭插件。点击 **...**，再点击 **Remove** 即可删除。

## 无法使用时

| 您看到的情况 | 处理方法 |
| --- | --- |
| 添加时提示 “Marketplace source format is invalid” | 关闭窗口。**Add**，**Manage marketplaces**：如果已有 `quotalogic-plugins`，点击 **...**，**Check for updates**。如果列表中仍没有插件，点击 **...**，**Remove**，然后从第 2 步重新添加 |
| 列表中没有 `Quotalogic` | 刷新 marketplace：**Add**，**Manage marketplaces**，**...**，**Check for updates** |
| **MCP servers** 窗口中没有 `quotalogic` | 更新插件（见“更新”），重启应用，开始新会话 |
| “isn't a recognized /mcp action” | 只输入 `/mcp`，不要加服务器名称 |
| `/mcp` 打开的是 Settings，而不是 **MCP servers** | 打开之前出现过 **MCP servers** 窗口的会话，在那里输入 `/mcp` |
| `/mcp` 在 **Chat** 标签页中不起作用 | 在 **Code** 标签页中输入 `/mcp` |
