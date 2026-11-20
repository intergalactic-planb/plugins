# Claude chat

[English](claude-chat.md) · [Русский](../ru/claude-chat.md) · [中文](../cn/claude-chat.md)

For the **Chat** tab of the Claude app. For the **Code** tab: [Claude app](claude-desktop.md).

Plugins need a paid Claude plan: Pro, Max, Team or Enterprise. The `intergalactic-planb/plugins` repository must be public.

## Install

1. In the side menu of the app, open **Plugins**.
2. Click **Add**, then **Add marketplace**, then **Add from a repository**.
3. In the **URL** field enter `intergalactic-planb/plugins` and choose the line **Use "intergalactic-planb/plugins"**.
4. Find `Quotalogic` in the list and open its card.

## Sign in to Quotalogic

1. In the plugin card, in the **Connectors** section, click **Connect** next to `quotalogic`.
2. In the **Add custom connector** window click **Continue**.
3. In the next window change nothing (**Sign in now** and **Register automatically** are selected) and click **Add**.
4. On the "Finish connecting a connector?" page click **Continue connecting**.
5. In the browser sign in to Quotalogic, check the workspace and confirm.
6. The plugin card shows **Added** and **Connected**. The **Finish setup** button disappears by itself.
7. Start a new chat and ask: "Show my catalogs". A list of your catalogs appears.

## Update

Open the Quotalogic card in **Plugins** and click **Update**. After the update start a new chat.

## Turn off and remove

Open the Quotalogic card in **Plugins**, click **...**, then **Remove**. The `quotalogic` connection (Custom connector) stays in the **Plugins** list: open its **...** and remove it separately if you no longer need it.

## If it does not work

| What you see | What to do |
| --- | --- |
| "Marketplace source format is invalid" when adding | Close the window. **Add**, **Manage marketplaces**: if `quotalogic-plugins` is already there, click **...**, **Check for updates**. If the plugin is still not in the list, click **...**, **Remove** and add it again from step 2 |
| `quotalogic` shows **Not connected** in the card | Click **Connect** and follow "Sign in to Quotalogic" |
| "No Quotalogic tools" in the chat | Open the Quotalogic card in **Plugins** and check that `quotalogic` is **Connected**. If not, click **Connect**. Then start a new chat |
