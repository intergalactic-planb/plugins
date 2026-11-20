# Claude app

[English](claude-desktop.md) · [Русский](../ru/claude-desktop.md) · [中文](../cn/claude-desktop.md)

For `claude` in a terminal: [Claude in a terminal](claude-terminal.md).

Plugins need a paid Claude plan: Pro, Max, Team or Enterprise. The `intergalactic-planb/plugins` repository must be public.

## Install

1. In the side menu of the app, open **Plugins**.
2. Click **Add**, then **Add marketplace**, then **Add from a repository**.
3. In the **URL** field enter `intergalactic-planb/plugins` and choose the line **Use "intergalactic-planb/plugins"**.
4. Find `Quotalogic` in the list and click **Add to this computer**. **Added** appears next to it.

## Sign in to Quotalogic

1. Open the **Code** tab and start a new session.
2. Type `/mcp` and send it. Write nothing after `/mcp`.
3. In the **MCP servers** window click **Connect** next to `quotalogic`.
4. The app asks "Open mcp-staging.quotalogic.io to authorize quotalogic?". Click **OK**.
5. In the browser sign in to Quotalogic, check the workspace and confirm.
6. In a new session ask: "Show my catalogs". A list of your catalogs appears.

## Update

1. **Plugins**, **Add**, **Manage marketplaces**, click **...** next to `quotalogic-plugins`, then **Check for updates**.
2. Close the window, open the Quotalogic card in **Plugins** and click **Update**.
3. Restart the app and start a new session.

## Turn off and remove

Open the Quotalogic card in **Plugins**. The switch on the right turns the plugin off. **...**, then **Remove** deletes it.

## If it does not work

| What you see | What to do |
| --- | --- |
| "Marketplace source format is invalid" when adding | Close the window. **Add**, **Manage marketplaces**: if `quotalogic-plugins` is already there, click **...**, **Check for updates**. If the plugin is still not in the list, click **...**, **Remove** and add it again from step 2 |
| `Quotalogic` is not in the list | Refresh the marketplace: **Add**, **Manage marketplaces**, **...**, **Check for updates** |
| `quotalogic` is not in the **MCP servers** window | Update the plugin (section "Update"), restart the app, start a new session |
| "isn't a recognized /mcp action" | Type only `/mcp`, without a server name |
| `/mcp` opens Settings instead of **MCP servers** | Open the session where the **MCP servers** window opened before and type `/mcp` there |
| `/mcp` does not work in the **Chat** tab | Type `/mcp` in the **Code** tab |
