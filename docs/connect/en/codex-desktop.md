# ChatGPT app

[English](codex-desktop.md) · [Русский](../ru/codex-desktop.md) · [中文](../cn/codex-desktop.md)

For the desktop ChatGPT app: the **Codex** and **ChatGPT** tabs. For a terminal: [Codex in a terminal](codex-terminal.md).

The `intergalactic-planb/plugins` repository must be public.

## Install

1. In the side menu of the app, open **Plugins**.
2. Click **Add**, then **Add marketplace**.
3. In the **Source** field enter `intergalactic-planb/plugins`. Leave **Git ref** and **Sparse paths** empty. Click **Add marketplace**.
4. On the **Personal** tab, in the **Quotalogic Plugins** section, click **+** to the right of `quotalogic`.

## Sign in to Quotalogic

1. After **+** the Quotalogic sign-in page opens. Sign in, check the workspace and confirm.
2. The app returns to the plugin list and shows "Quotalogic plugin installed".
3. Click **Try now** or open the **Codex** or **ChatGPT** tab and ask: "Show my catalogs". A list of your catalogs appears.

## Turn off and remove

1. Open the Quotalogic card in **Plugins** and click **Remove**.
2. To remove the marketplace too: **Plugins**, the **Store** tab, remove `Quotalogic Plugins`.

## Set up Quotalogic for a ChatGPT workspace

If a member sees the Quotalogic skills but ChatGPT cannot read their catalogs, give them access to the separate **Quotalogic MCP** connection.

### For workspace admins

1. If the workspace does not have the plugin yet, open **Admin Console → Plugins → Add → Import marketplace**. Set **Source** to `https://github.com/intergalactic-planb/plugins`, leave **Path** empty, and set **Branch, tag, or commit** to `main`. Authorize GitHub access if prompted.
2. In **Admin Console → Plugins**, open **Quotalogic**. Choose **Available** to let members install it or **Installed** to install it automatically.
3. If **Quotalogic MCP** is missing, switch to the organization workspace in your browser and open **Plugins → + → Add custom MCP server**. Use `Quotalogic MCP` as the name, `https://mcp-staging.quotalogic.io/mcp` as the staging URL, and **OAuth** for authentication. Select **Create as a plugin** and check that ChatGPT found the tools.
4. In **Admin Console → Plugins**, open **Quotalogic MCP** and set its **Installation policy** for the same members. With **Enterprise/Edu**, you can grant access by role. With **Business**, the installation policy applies to the whole workspace.

### For workspace members

1. Switch to the organization workspace. In **Plugins**, open the workspace tab and install **Quotalogic** if it is not already installed.
2. Open **Quotalogic MCP**, select **Connect**, and sign in with your Quotalogic account. Review the Quotalogic workspace and confirm access.
3. Start a new chat, select **Quotalogic MCP** using `@`, and ask “Show my catalogs”. The connection works when ChatGPT returns catalogs from your workspace.

If the skills package is marked **Desktop only**, use that package in the desktop app. The separate **Quotalogic MCP** connection can provide catalog access in the browser.

[Import a marketplace](https://help.openai.com/en/articles/20001504-importing-and-syncing-plugin-marketplaces-from-github) · [Connect MCP in ChatGPT](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)
