# 终端中的 Codex

[English](../en/codex-terminal.md) · [Русский](../ru/codex-terminal.md) · **中文**

适用于终端中的 `codex`。ChatGPT 应用请见：[ChatGPT 应用](codex-desktop.md)。

`intergalactic-planb/plugins` 仓库必须是公开的。

## 安装

在终端中依次执行：

```bash
codex plugin marketplace add intergalactic-planb/plugins
```

```bash
codex plugin add quotalogic@quotalogic-plugins
```

## 登录 Quotalogic

1. 执行：

   ```bash
   codex mcp login quotalogic
   ```

2. 浏览器会打开。登录 Quotalogic，检查工作区并确认。
3. 终端会显示 `Successfully logged in to MCP server 'quotalogic'`。
4. 检查登录：`codex mcp list`。`quotalogic` 在 `Auth` 列显示 `OAuth`。
5. 启动 `codex` 并询问：“显示我的目录”。会出现您的目录列表。

## 更新

```bash
codex plugin marketplace upgrade
```

```bash
codex plugin add quotalogic@quotalogic-plugins
```

然后关闭 `codex` 并重新启动。

## 关闭和删除

如果还要重置登录，请先执行：

```bash
codex mcp logout quotalogic
```

然后删除插件和 marketplace：

```bash
codex plugin remove quotalogic@quotalogic-plugins
```

```bash
codex plugin marketplace remove quotalogic-plugins
```

## 无法使用时

| 您看到的情况 | 处理方法 |
| --- | --- |
| `codex mcp list` 中 `quotalogic` 的 `Auth` 列显示 `Not logged in` | `codex mcp login quotalogic` |
| Codex 提示没有 Quotalogic 工具 | 检查 `codex mcp list`：`quotalogic` 必须是 `enabled` 且为 `OAuth`。否则请登录（见“登录 Quotalogic”）。然后重新启动 `codex` |
| `codex plugin list` 输出的列表很长 | 只显示需要的 marketplace：`codex plugin list --marketplace quotalogic-plugins` |
| `codex mcp list` 中有两行 Quotalogic 地址 | 其中一行是您之前手动添加的。删除它：`codex mcp remove <name>`，其中 `<name>` 取自 `codex mcp list`（不是 `quotalogic`） |
