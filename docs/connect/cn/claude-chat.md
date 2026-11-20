# Claude 聊天

[English](../en/claude-chat.md) · [Русский](../ru/claude-chat.md) · **中文**

适用于 Claude 应用的 **Chat** 标签页。**Code** 标签页请见：[Claude 应用](claude-desktop.md)。

使用插件需要 Claude 付费方案：Pro、Max、Team 或 Enterprise。`intergalactic-planb/plugins` 仓库必须是公开的。

## 安装

1. 在应用的侧边菜单中打开 **Plugins**。
2. 点击 **Add**，再点击 **Add marketplace**，然后点击 **Add from a repository**。
3. 在 **URL** 栏输入 `intergalactic-planb/plugins`，并选择 **Use "intergalactic-planb/plugins"** 这一行。
4. 在列表中找到 `Quotalogic` 并打开它的卡片。

## 登录 Quotalogic

1. 在插件卡片的 **Connectors** 部分，点击 `quotalogic` 旁的 **Connect**。
2. 在 **Add custom connector** 窗口中点击 **Continue**。
3. 在下一个窗口中不要更改任何设置（已选中 **Sign in now** 和 **Register automatically**），点击 **Add**。
4. 在 “Finish connecting a connector?” 页面点击 **Continue connecting**。
5. 在浏览器中登录 Quotalogic，检查工作区并确认。
6. 插件卡片会显示 **Added** 和 **Connected**。**Finish setup** 按钮会自动消失。
7. 开始新的聊天并询问：“显示我的目录”。会出现您的目录列表。

## 更新

在 **Plugins** 中打开 Quotalogic 卡片，点击 **Update**。更新后开始新的聊天。

## 关闭和删除

在 **Plugins** 中打开 Quotalogic 卡片，点击 **...**，再点击 **Remove**。`quotalogic` 连接（Custom connector）仍保留在 **Plugins** 列表中：如不再需要，请打开它的 **...** 单独删除。

## 无法使用时

| 您看到的情况 | 处理方法 |
| --- | --- |
| 添加时提示 “Marketplace source format is invalid” | 关闭窗口。**Add**，**Manage marketplaces**：如果已有 `quotalogic-plugins`，点击 **...**，**Check for updates**。如果列表中仍没有插件，点击 **...**，**Remove**，然后从第 2 步重新添加 |
| 卡片中 `quotalogic` 显示 **Not connected** | 点击 **Connect**，并按“登录 Quotalogic”操作 |
| 聊天中提示没有 Quotalogic 工具 | 在 **Plugins** 中打开 Quotalogic 卡片，确认 `quotalogic` 为 **Connected**。如果不是，点击 **Connect**。然后开始新的聊天 |
