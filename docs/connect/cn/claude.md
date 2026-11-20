# Claude 版 Quotalogic 插件

[English](../en/claude.md) · [Русский](../ru/claude.md) · **中文**

## 这是什么

这个插件让 Claude 学会使用 Quotalogic：把价目表导入目录、修改价格、用另一种货币显示价格、制作并发送商业报价。Claude 会自己选择合适的场景，每次都会先说明要做什么，在您回答"是"之前不会写入任何数据。

插件还会自动添加与 Quotalogic 的连接（目前是测试服务器 staging）。安装后，您只需登录一次自己的 Quotalogic 账户，无需另外连接 Quotalogic。

## 选择您的路径

**1. 您在哪里使用 Claude？**

- **Code**：终端里的 `claude` 程序，或 Claude Desktop 应用的 **Code** 标签页。见第 1 部分。
- **Chat 或 Cowork**：claude.ai 网站，或 Claude Desktop 应用的 **Chat** 标签页。见第 2 部分。

**2. 您使用哪种订阅？**

- **个人订阅**（Pro、Max）：所有步骤都由您自己完成。
- **企业订阅**（Team、Enterprise）：在 Chat 和 Cowork 中，通常由组织所有者（Owner）添加插件（第 3 部分），您只需安装或在列表中找到它。
- **Free**：此套餐无法安装插件。

| 使用位置 | 个人订阅 | 企业订阅 |
| --- | --- | --- |
| Code（终端、Code 标签页） | 第 1 部分 | 第 1 部分，或由所有者为所有人统一部署（第 3 部分） |
| Chat 和 Cowork | 第 2 部分 | 第 3 部分（所有者），然后第 2 部分 |

**我们尚未实际测试的内容：** 第 2、3 部分（Chat、Cowork、组织设置）均依据 Anthropic 的文档编写，菜单名称可能不同。第 1 部分我们已测试安装（包括从压缩包安装），并确认连接会出现在 `/mcp` 中并提示需要登录。

## 准备工作

1. **Quotalogic 账户。** 连接由插件自动添加。如果您以前手动连接过 Quotalogic，保持不变即可：同一地址 Claude 只保留一个连接。
2. **插件本身**，两种方式任选其一：
   - **压缩包。** 我们把文件发给您，无需 GitHub 权限。
   - **GitHub。** 需要私有仓库 `intergalactic-planb/plugins` 的读取权限。

不确定选哪个？请选压缩包，更简单。压缩包有两个文件，因为内部结构不同：

- `quotalogic-plugins.zip`：用于第 1 部分（Code）；
- `quotalogic-plugin-claude.zip`：用于第 2、3 部分（Chat、Cowork、组织上传）。

---

## 第 1 部分 Code（终端和 Code 标签页）

检查 Claude 是否已安装：

```bash
claude --version
```

### 方式 B：从压缩包安装（更简单）

1. 把 `quotalogic-plugins.zip` 解压到一个固定位置，例如"文稿"（Documents）文件夹，会得到名为 `quotalogic-plugins` 的文件夹。
   **安装后请勿删除或移动此文件夹**：Claude 从这里读取插件。
2. 在终端中添加该文件夹（macOS 示例；如果解压到其他位置，请换成您的路径）：

   ```bash
   claude plugin marketplace add ~/Documents/quotalogic-plugins
   ```

   应显示 `Successfully added marketplace: quotalogic-plugins`。
3. 安装插件：

   ```bash
   claude plugin install quotalogic@quotalogic-plugins
   ```

   应显示 `Successfully installed plugin: quotalogic@quotalogic-plugins`。

**注意：** 压缩包是发送当天的版本快照。我们之后改进插件时，您会一直使用旧版本，直到收到新的压缩包。

### 方式 A：从 GitHub 安装

1. **仓库权限。** 打开 https://github.com/intergalactic-planb/plugins 。页面能打开说明有权限；出现 404 错误说明没有权限：请向我们申请权限，或使用压缩包。
2. **在电脑上登录 GitHub**（需要 [GitHub CLI](https://cli.github.com/)）：

   ```bash
   gh auth login
   gh auth setup-git
   ```

   第一条命令中依次选择 GitHub.com、HTTPS，然后通过浏览器登录。`gh auth status` 应显示 `Logged in to github.com`。
3. 添加插件目录（每台电脑一次）：

   ```bash
   claude plugin marketplace add intergalactic-planb/plugins
   ```

4. 安装插件：

   ```bash
   claude plugin install quotalogic@quotalogic-plugins
   ```

**Claude Desktop 的 Code 标签页**与终端读取相同的设置：在终端安装的插件，在应用中也已可用。添加插件目录（第 3 步）只能在终端完成；之后也可以用按钮安装：输入框旁的 **+**，然后 **Add plugins**。

### 登录 Quotalogic

安装后只需一次。

1. 新建一个空文件夹并在其中启动 Claude。不要在包含代码或密钥的文件夹中启动 Claude。

   ```bash
   mkdir -p ~/quotalogic && cd ~/quotalogic && claude
   ```

   如果安装时 Claude 正在运行，请退出（`/exit`）后重新启动。
2. 输入 `/mcp`，选择 `plugin:quotalogic:quotalogic`，然后选择 **Authenticate**。
3. 浏览器会打开：登录 Quotalogic，确认工作区，并确认授权。
4. 在 `/mcp` 中该连接显示为 **connected**。

Claude 会在您确认授权时浏览器中打开的那个工作区内工作。要切换工作区，请在浏览器中切换后再次执行 **Authenticate**。登录在不使用的情况下最多保持 30 天。

### 检查是否正常

1. 询问 "Show my Quotalogic catalogs"（或用中文"显示我的 Quotalogic 目录"），应返回您工作区中的目录列表。
2. 输入 `/`，列表中应有四行以 `/quotalogic:` 开头：`catalog-import`、`catalog-edit`、`currency-conversion`、`offer`。
3. 把一个小的价目表放进文件夹，用自己的话提出请求，例如"用 prices.xlsx 创建目录"。Claude 会说明它如何理解各列和货币，并等待您的"是"。在您回答"是"之前不会写入任何数据。

### 更新

- **方式 A：**

  ```bash
  claude plugin update quotalogic@quotalogic-plugins
  ```

  新版本会在下次启动 Claude 时加载。

  **自动更新：** 在 Claude 中输入 `/plugin`，打开 **Marketplaces** 标签页，选择 `quotalogic-plugins`，然后 **Enable auto-update**。对于私有仓库，只有完成方式 A 的第 2 步（`gh auth login` 和 `gh auth setup-git`）后才有效；仅在环境变量中设置令牌是不够的。没有保存登录时，更新会静默失败，继续使用旧版本。Claude 会在会话中第一条消息几分钟后检查更新；新版本在 `/reload-plugins` 后或下次启动时生效。
- **方式 B：** 不会自动更新。收到新压缩包后，删除插件和文件夹（见"删除"），解压新压缩包并重新安装。

### 删除

```bash
claude plugin uninstall quotalogic@quotalogic-plugins
claude plugin marketplace remove quotalogic-plugins
```

与 Quotalogic 的连接会随插件一起删除。您在 Quotalogic 中授予的访问权限仍然保留，可以在 Quotalogic 的连接管理页面撤销。如果是从压缩包安装的，现在可以删除 `quotalogic-plugins` 文件夹。

---

## 第 2 部分 Chat 和 Cowork（claude.ai 和应用的 Chat 标签页）

适用于个人订阅。如果您使用 Team 或 Enterprise，请先阅读第 3 部分：插件可能已由所有者添加，或您的组织关闭了上传功能。

### 方式 B：上传压缩包

1. 向我们索取 `quotalogic-plugin-claude.zip`（`.claude-plugin` 文件夹直接位于压缩包顶层，没有外层文件夹）。
2. 在 Claude 中打开 **Customize**，然后 **Plugins**，点击 **Add**，再点 **Upload plugin**，选择该文件。大小上限为 200 MB。
3. 插件出现在列表中，将其开启。
4. 连接 Quotalogic（见下文）。

### 方式 A：从 GitHub 添加

1. 在 **Customize**、**Plugins** 中点击 **Add**，然后 **Add marketplace**。
2. 输入 `intergalactic-planb/plugins`。
3. 根据提示连接您的 GitHub 账户，并授予 Claude GitHub 应用访问该仓库的权限。该账户需要有仓库权限。
4. 找到 `quotalogic` 并安装。
5. 连接 Quotalogic（见下文）。

### 连接 Quotalogic

在 Chat 和 Cowork 中，插件自带的连接不会自动开启。

1. 在 **Customize**、**Plugins** 中打开该插件，进入 **Connectors** 标签页。
2. 点击 Quotalogic 旁的 **Connect**，登录 Quotalogic 并确认授权。
3. 在 Team 和 Enterprise 中，所有者为组织添加该连接后才会出现此按钮（第 3 部分）。

如果 Quotalogic 已在 **Settings**、**Connectors** 中连接，则无需再次连接。

### 企业订阅

插件可能已经出现在 **Customize**、**Plugins** 列表中。如果没有，并且 **Add** 没有任何选项，说明您的组织关闭了上传功能：请联系所有者（第 3 部分）。

### 检查

在新对话中附上一个小的价目表，并写："用这个文件创建目录"。Claude 应说明它如何理解各列，并等待您的"是"。如果它说没有 Quotalogic 工具，说明尚未登录连接：见上文"连接 Quotalogic"。

---

## 第 3 部分 组织所有者（Team 和 Enterprise）

只有组织**所有者**（Owner）才能部署插件，普通成员无法操作。

### 用于 Chat 和 Cowork

1. 打开 **Organization settings**，然后 **Plugins & skills**，点击 **Add**。
2. 任选其一：
   - **Upload plugin**：上传 `quotalogic-plugin-claude.zip`（无需 GitHub，适合试用）。
   - **Sync from GitHub**：连接仓库 `intergalactic-planb/plugins`。仓库必须是私有或内部仓库（我们的是私有仓库）。更新通过组织的 GitHub 连接进行，成员无需自己登录 GitHub。点击 **Re-sync** 时同步；如果开启 **Sync automatically**，主分支每次变更都会自动同步（需要在 GitHub 中配置 webhook）。
3. 设置 **Default access**：
   - **Not available**：所有人都不可用；
   - **Available to install**：每个人自行安装；
   - **Installed by default**：为所有人安装，可以删除；
   - **Required**：为所有人安装，不能删除。

   在 Enterprise 中可以只授予指定的用户组。
4. 在插件的 **Connectors** 标签页中，为组织添加 Quotalogic 连接。之后每位成员用自己的 Quotalogic 账户连接。
5. 如需禁止成员添加自己的插件，请在 **Policy** 标签页关闭 **User-created skills**。如果关闭 **Skills** 本身，所有组织插件都会停止工作。

### 用于 Code（终端和 Code 标签页）

在 **Organization settings**、**Claude Code**、**Managed settings** 中为所有人部署插件。每位成员需要私有仓库的读取权限，或使用压缩包。插件会在成员下一次 Claude 会话开始时出现。要让所有人保持自动更新，请在目录上设置 `"autoUpdate": true`；之后成员无法关闭自动更新：

```json
{
  "extraKnownMarketplaces": {
    "quotalogic-plugins": {
      "source": { "source": "github", "repo": "intergalactic-planb/plugins" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": { "quotalogic@quotalogic-plugins": true }
}
```

此格式来自 Anthropic 关于组织插件的文档（`code.claude.com/docs/en/plugins/org.md`），我们尚未测试。

组织也可以禁止成员添加自己的插件目录（`strictKnownMarketplaces` 设置）。此时第 1 部分的方式 A 和 B 对他们无效：请联系所有者。

---

## 常见问题

| 您看到的情况 | 处理方法 |
| --- | --- |
| `Repository not found` 或访问错误（方式 A） | 没有仓库权限，或未登录 GitHub：见方式 A 第 1、2 步。或者使用压缩包 |
| `Plugin "quotalogic" not found in marketplace` | 输入错误。名称必须是 `quotalogic@quotalogic-plugins` |
| 方式 B：找不到路径 | 检查文件夹是否还在、命令中的路径是否正确 |
| 输入 `/` 后没有 `/quotalogic:` 开头的行 | 重启 Claude 或输入 `/reload-plugins`。错误信息见 `/plugin` 的 **Errors** 标签页 |
| Claude 说没有 Quotalogic 工具 | 尚未登录：Code 中见第 1 部分"登录 Quotalogic"，Chat 中见第 2 部分"连接 Quotalogic"。`/mcp` 中应显示 **connected** |
| Chat 中没有 **Add** 或 **Upload plugin** | 您的组织关闭了上传，或您的套餐不支持插件：请联系所有者 |
| Chat 中上传压缩包被拒绝 | `.claude-plugin` 文件夹必须位于压缩包顶层（没有外层文件夹），且顶层不能有 `bin` 文件夹。请使用 `quotalogic-plugin-claude.zip` |
| Code 中 `/plugin` 打不开或无法添加目录 | 您的组织禁止添加自己的目录：请联系所有者 |

## 团队内部 构建与测试

**从本仓库的克隆构建两个压缩包**（基于 `main` 中已提交的内容）：

1. `quotalogic-plugins.zip`：用于第 1 部分和 Codex。构建命令见 [codex-chatgpt.md](codex-chatgpt.md#团队内部-构建与测试)；其中为 Codex 替换的文件对 Claude 没有影响。
2. `quotalogic-plugin-claude.zip`：用于第 2、3 部分。内容必须直接位于压缩包顶层：

   ```bash
   git -C ~/Projects/plugins archive --format=zip main -o ~/quotalogic-plugin-claude.zip
   ```

Anthropic 文档的要求：顶层有一个 `.claude-plugin/plugin.json`，不超过 200 MB，顶层没有包含程序的 `bin` 文件夹。**我们尚未测试将压缩包上传到 claude.ai 或组织设置。** 请告知客户插件版本（见 `.claude-plugin/plugin.json`）。

**以用户身份测试且不影响自己的设置**（第 1 部分）：只安装到测试文件夹。

```bash
mkdir -p ~/quotalogic-plugin-test && cd ~/quotalogic-plugin-test
claude plugin marketplace add intergalactic-planb/plugins --scope local
claude plugin install quotalogic@quotalogic-plugins --scope local
```

设置会写入该文件夹的 `.claude/settings.local.json`。要测试尚未推送的版本，请用克隆路径（例如 `~/Projects/plugins`）代替 `intergalactic-planb/plugins`。清理时删除测试文件夹即可。
