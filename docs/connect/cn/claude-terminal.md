# 终端中的 Claude

[English](../en/claude-terminal.md) · [Русский](../ru/claude-terminal.md) · **中文**

适用于终端中的 `claude`。Claude 应用请见：[Claude 应用](claude-desktop.md)。

**Chat** 标签页请见：[Claude 聊天](claude-chat.md)。

使用插件需要 Claude 付费方案：Pro、Max、Team 或 Enterprise。`intergalactic-planb/plugins` 仓库必须是公开的。

## 安装

在终端中依次执行：

```bash
claude plugin marketplace add intergalactic-planb/plugins
```

```bash
claude plugin install quotalogic@quotalogic-plugins
```

提示 `SSH not configured, cloning via HTTPS` 不是错误。

## 登录 Quotalogic

1. 启动 `claude` 并输入 `/mcp`。
2. 用向下箭头选择 `plugin:quotalogic:quotalogic`（在列表末尾），按 Enter。
3. 选择 **Authenticate**。
4. 在浏览器中登录 Quotalogic，检查工作区并确认。
5. 回到终端：状态为 `connected`。询问：“显示我的目录”。会出现您的目录列表。

## 在 Claude 应用中

通过终端安装的插件在 Claude 应用的 **Code** 标签页中同样可用，无需在应用中再次安装。

## 更新

```bash
claude plugin marketplace update quotalogic-plugins
```

然后关闭 `claude`（`/exit`）并重新启动。

## 关闭和删除

```bash
claude plugin disable quotalogic@quotalogic-plugins
```

```bash
claude plugin marketplace remove quotalogic-plugins
```

第二条命令会同时删除插件。要重置 Quotalogic 登录，输入 `/mcp`，选择 `plugin:quotalogic:quotalogic`，然后选择 **Clear authentication**。

## 无法使用时

| 您看到的情况 | 处理方法 |
| --- | --- |
| `/mcp` 中没有 `plugin:quotalogic:quotalogic` | 关闭 `claude`（`/exit`）并重新启动。如果仍不行：执行 `claude plugin marketplace update quotalogic-plugins`，再启动 `claude` |
| 状态为 `needs sign-in` | 选择该服务器，然后选择 **Authenticate** |
| 菜单显示 **Re-authenticate** 而不是 **Authenticate** | 您已登录。要重新登录，请选择 **Re-authenticate** 或 **Clear authentication** |
| 无法添加 marketplace | 确认仓库是公开的，然后重新执行命令 |
