# Codex 和 ChatGPT 版 Quotalogic 插件

[English](../en/codex-chatgpt.md) · [Русский](../ru/codex-chatgpt.md) · **中文**

## 这是什么

这个插件可以帮您导入价目表、修改价格、用另一种货币显示价格，或制作商业报价。导入前，助手会先展示将要导入的内容，并等待您单独确认。某些操作（例如创建商业报价）只需您直接提出请求即可。

在 Codex 中，插件还会自动添加与 Quotalogic 的连接（目前是测试服务器 staging）：安装后只需登录一次自己的 Quotalogic 账户。在浏览器版 ChatGPT 中，连接仍需单独设置。

## 您在哪里使用？

| 使用位置 | 阅读部分 |
| --- | --- |
| 终端中的 Codex，或 ChatGPT 桌面应用中的 Codex | [电脑上的 Codex](#电脑上的-codex) |
| 浏览器版 ChatGPT，普通 Chat 或云端 Work | [ChatGPT 网页版](#chatgpt-网页版) |

## 准备工作

1. **Quotalogic 账户。** 在 Codex 中连接由插件自动添加，无需另外连接 Quotalogic。在浏览器版 ChatGPT 中，请将 Quotalogic 作为单独的应用连接。
2. **插件本身。** 没有 GitHub 权限时，请向 Quotalogic 团队索取 `quotalogic-plugins.zip` 文件。如果有私有仓库的访问权限，可以从 GitHub 安装。在浏览器版 ChatGPT 中，由工作区管理员为团队添加插件。

## 电脑上的 Codex

本指南需要使用终端。运行 `codex --version`。如果找不到命令，请先安装 Codex CLI（[OpenAI 指南](https://learn.chatgpt.com/docs/cli)）。

### 从压缩包安装

无需 GitHub。

1. 把 `quotalogic-plugins.zip` 解压到一个固定位置，例如"文稿"（Documents）文件夹，会得到名为 `quotalogic-plugins` 的文件夹。
   **安装后请勿删除或移动此文件夹。**
2. 添加该文件夹。示例使用 macOS 的"文稿"文件夹；如果解压到其他位置，请换成您的路径。

   ```bash
   codex plugin marketplace add ~/Documents/quotalogic-plugins
   ```

3. 安装插件。

   ```bash
   codex plugin add quotalogic@quotalogic-plugins
   ```

4. 继续"登录 Quotalogic"。

更新时需要向 Quotalogic 团队索取新的压缩包。

### 从 GitHub 安装

1. 在浏览器中登录 GitHub 并打开[插件仓库](https://github.com/intergalactic-planb/plugins)。如果出现 404 错误，请向 Quotalogic 团队申请权限或索取压缩包。
2. 在终端中登录 GitHub（需要 [GitHub CLI](https://cli.github.com/)）：

   ```bash
   gh auth login
   gh auth setup-git
   ```

   第一条命令中依次选择 GitHub.com、HTTPS，并用有仓库权限的账户通过浏览器登录。第二条命令让 Git 使用此登录。`gh auth status` 会显示当前登录的账户。
3. 添加插件目录。

   ```bash
   codex plugin marketplace add intergalactic-planb/plugins
   ```

4. 安装插件。

   ```bash
   codex plugin add quotalogic@quotalogic-plugins
   ```

### 登录 Quotalogic

安装后只需一次，在终端中运行：

```bash
codex mcp login quotalogic
```

浏览器会打开：登录 Quotalogic，确认工作区，并确认授权。终端会显示 `Successfully logged in to MCP server 'quotalogic'`。

助手会在您确认授权时浏览器中打开的那个工作区内工作。要切换工作区，请在浏览器中切换后再次运行该命令。

**如果您以前手动连接过 Quotalogic**（例如名称为 `quotcat`），现在会有两个相同的连接。请删除手动添加的那个：

```bash
codex mcp remove quotcat
```

检查：`codex mcp list` 只显示一行 `quotalogic`，地址为 Quotalogic 的地址。

### 检查是否正常

1. 检查插件是否已安装。

   ```bash
   codex plugin list --marketplace quotalogic-plugins
   ```

   `quotalogic@quotalogic-plugins` 这一行应显示 `installed, enabled`。
2. 在单独的文件夹中打开一个**新的** Codex 对话。如果使用 ChatGPT 桌面应用，请先重启应用并选择 **Codex**。
3. 输入 `$`，选择导入场景 `quotalogic:catalog-import`。
4. 把一个小的测试价目表放进文件夹，然后写："准备把这个价目表导入新的 Quotalogic 目录并显示预览，在执行导入前停止。" 预览中应显示各列和货币。检查到此结束。

日常使用时，用自己的话描述任务即可，助手会自行选择场景。

### 更新

- 从 **GitHub** 安装的：运行

  ```bash
  codex plugin marketplace upgrade quotalogic-plugins
  codex plugin remove quotalogic@quotalogic-plugins
  codex plugin add quotalogic@quotalogic-plugins
  ```

- 从**压缩包**安装的：索取新文件。删除旧版本（见"删除"），解压新压缩包并重新安装。

已经打开的对话不会获得新版本：请开始新对话。

**不会自动更新。** Codex 和 ChatGPT 桌面应用没有插件自动更新功能；根据 OpenAI 的文档，目录只能通过 `codex plugin marketplace upgrade` 刷新。因此我们会单独通知新版本。例外是由管理员添加目录的 ChatGPT 网页版：它每天自动刷新（见"在 ChatGPT 网页版中更新"）。

### 删除

```bash
codex plugin remove quotalogic@quotalogic-plugins
codex plugin marketplace remove quotalogic-plugins
```

检查 `codex mcp list`：`quotalogic` 连接应已消失。您在 Quotalogic 中授予的访问权限仍然保留，可以在 Quotalogic 的连接管理页面撤销。如果是从压缩包安装的，现在可以删除 `quotalogic-plugins` 文件夹。

## ChatGPT 网页版

**通过浏览器访问目录尚未得到确认**，也未确认 ChatGPT 网页版是否会使用插件自带的连接。在测试场景之前，请单独连接 Quotalogic 应用，并确认 ChatGPT 能看到您的目录。

如果您是团队成员，请让 ChatGPT 工作区管理员添加插件并为您开通访问。安装管理员已添加的插件时，成员不需要 GitHub 权限。

如果您使用个人 ChatGPT 账户，请通过[电脑上的 Codex](#从压缩包安装) 从压缩包安装。

### 1. 管理员：添加插件

**从 GitHub 添加。** 用能读取私有仓库 `intergalactic-planb/plugins` 的账户登录 GitHub。更新时也会使用这个账户。

1. 打开 **Admin**，然后 **Plugins**，然后 **Add**，然后 **Import marketplace**。
2. 在 **Source** 中输入 `https://github.com/intergalactic-planb/plugins`。
3. **Path** 留空。
4. 在 **Branch, tag, or commit** 中输入 `main`。
5. 点击 **Import marketplace**，并在提示时允许 ChatGPT 访问 GitHub。查看 **Import results**。（[从 GitHub 导入](https://learn.chatgpt.com/docs/enterprise/plugin-management)）

**从 ZIP 添加。** 如果管理控制台中有 **Plugins**、**Add**、**Upload plugin**，OpenAI 允许上传插件文件。我们尚未测试用这种方式上传我们的 Codex 压缩包。（[在 ChatGPT 中上传 ZIP](https://help.openai.com/en/articles/20001256-plugins-in-chatgpt)）

然后打开插件卡片。在 **Installation policy** 中选择 **Available**（成员可自行安装）或 **Installed**（为他们安装）。同时为这些成员开通 Quotalogic 数据应用的访问。

### 2. 成员：安装

1. 在 **Plugins** 中打开以您的工作区命名的标签页，选择 Quotalogic 插件。
2. 如果插件尚未安装，点击 **+**。
3. 如果尚未设置目录访问，请通过数据应用登录 Quotalogic。
4. 开始新对话。

### 3. 检查

输入 `@`，选择 Quotalogic 插件或 `catalog-import`，并附上测试价目表。请求："准备把这个价目表导入新的 Quotalogic 目录并显示预览，在执行导入前停止。" 预览中应显示各列和货币。（[在 ChatGPT 中选择插件](https://learn.chatgpt.com/docs/plugins#choose-a-specific-plugin)）

插件上的 **Desktop only** 表示它需要桌面应用。如果在新对话的 `@` 菜单中没有 Quotalogic，请让管理员检查您对插件和 Quotalogic 连接的访问权限。

### 在 ChatGPT 网页版中更新

- GitHub 目录每天检查一次。要立即更新，在 **Admin**、**Plugins**、**Marketplaces** 中选择该目录并点击 **Sync now**。在报告中查看 Quotalogic 的结果。如果管理员失去了 GitHub 访问权限，请恢复权限或让另一位管理员重新导入。（[从 GitHub 更新](https://learn.chatgpt.com/docs/enterprise/plugin-management#keep-plugins-up-to-date)）
- 对于从 ZIP 上传的插件，打开其卡片并选择 **Upload new version**（如果有此按钮）。（[从 ZIP 更新](https://help.openai.com/en/articles/20001256-plugins-in-chatgpt)）

更新后请开始新对话。

### 从 ChatGPT 网页版删除

如果工作区规则允许，成员可以在 **Plugins** 中的卡片上删除 Quotalogic。管理员可以让插件对某些角色不可用。在 **Admin**、**Plugins**、**Marketplaces** 中删除整个目录会删除从中添加的所有插件。Quotalogic 数据应用会保留，需单独管理。

## 常见问题

| 您看到的情况 | 处理方法 |
| --- | --- |
| 从 GitHub 添加时出现访问错误或 `not found` | 检查终端是否使用有权限的账户登录，并已运行 `gh auth setup-git`。或者索取压缩包 |
| 从压缩包安装时找不到路径 | 检查文件夹是否还在、命令中的路径是否正确 |
| 从压缩包安装时要求 GitHub 权限 | 请向 Quotalogic 团队索取为无 GitHub 安装而构建的压缩包 |
| 输入 `$` 后没有 `quotalogic` | 重启应用，选择 Codex，开始新对话。在终端中用 `codex plugin list --marketplace quotalogic-plugins` 检查 |
| `codex mcp list` 中有两个相同的连接 | 删除手动添加的那个：`codex mcp remove quotcat`（使用列表中的名称，不是 `quotalogic`） |
| 助手说没有 Quotalogic 工具 | 在 Codex 中运行 `codex mcp login quotalogic`。在 ChatGPT 网页版中，请管理员检查数据应用是否对您的账户可用 |

## 团队内部 构建与测试

**构建压缩包**（从本仓库的克隆，基于 `main` 中已提交的内容）。对于 Codex，必须替换压缩包中的一个文件：仓库中的 Codex 目录指向 GitHub，不替换的话，从压缩包安装仍会访问私有仓库。已测试：替换后插件可以从文件夹安装，无需 GitHub；不替换时 Codex 会从 GitHub 下载。同一个压缩包也适用于 Claude Code。

```bash
T=$(mktemp -d) && cd "$T"
git -C ~/Projects/plugins archive --prefix=quotalogic-plugins/ main | tar -x
cat > quotalogic-plugins/.agents/plugins/marketplace.json <<'EOF'
{
  "name": "quotalogic-plugins",
  "interface": { "displayName": "Quotalogic Plugins" },
  "plugins": [
    {
      "name": "quotalogic",
      "source": { "source": "local", "path": "./" },
      "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
      "category": "Productivity"
    }
  ]
}
EOF
zip -qr ~/quotalogic-plugins.zip quotalogic-plugins
```

请在压缩包名称或给客户的消息中注明插件版本（见 `.claude-plugin/plugin.json`）。

**检查构建结果**：在干净的文件夹中使用单独的 Codex 设置，不影响您自己的设置：

```bash
mkdir -p ~/archive-test && cd ~/archive-test && unzip -q ~/quotalogic-plugins.zip
export CODEX_HOME=~/archive-test/codex-home && mkdir -p "$CODEX_HOME"
codex plugin marketplace add ~/archive-test/quotalogic-plugins
codex plugin add quotalogic@quotalogic-plugins
codex plugin list --marketplace quotalogic-plugins
codex mcp list
```

插件行的 `SOURCE` 列必须显示文件夹路径，而不是 GitHub 地址。`codex mcp list` 必须显示 `quotalogic`（Codex 从 `.mcp.json` 读取服务器；根目录的 `mcp.json` 不会被读取）。

**检查导入时的已知后端问题。** 仅有 `SUCCESS` 状态是不够的：后端可能在没有警告的情况下把小数价格少存一分（`19.99` 存为 `19.98`）。测试插件本身时请使用整数价格，例如：

```csv
SKU,Name,Price
WEB-05-1,Web Work Test,20
```

如需在测试工作区中尝试真实导入，请要求创建 EUR 目录 `Web Work Test`，将 `SKU` 对应编码、`Name` 对应名称、`Price` 对应主价格，显示对照表，并在单独确认前停止。确认后，请要求重新读取该商品，并与文件中的值进行核对。
