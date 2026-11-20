# Claude in a terminal

[English](claude-terminal.md) · [Русский](../ru/claude-terminal.md) · [中文](../cn/claude-terminal.md)

For `claude` in a terminal. For the Claude app: [Claude app](claude-desktop.md).

Plugins need a paid Claude plan: Pro, Max, Team or Enterprise. The `intergalactic-planb/plugins` repository must be public.

## Install

In a terminal run, one after the other:

```bash
claude plugin marketplace add intergalactic-planb/plugins
```

```bash
claude plugin install quotalogic@quotalogic-plugins
```

The message `SSH not configured, cloning via HTTPS` is not an error.

## Sign in to Quotalogic

1. Start `claude` and type `/mcp`.
2. With the down arrow choose `plugin:quotalogic:quotalogic` (it is at the end of the list) and press Enter.
3. Choose **Authenticate**.
4. In the browser sign in to Quotalogic, check the workspace and confirm.
5. Return to the terminal: the status is `connected`. Ask: "Show my catalogs". A list of your catalogs appears.

## In the Claude app

A plugin installed through the terminal is also available in the Claude app, in the **Code** tab. There is no need to install it there again.

## Update

```bash
claude plugin marketplace update quotalogic-plugins
```

Then close `claude` (`/exit`) and start it again.

## Turn off and remove

```bash
claude plugin disable quotalogic@quotalogic-plugins
```

```bash
claude plugin marketplace remove quotalogic-plugins
```

The second command removes the plugin too. To reset the Quotalogic sign-in, type `/mcp`, choose `plugin:quotalogic:quotalogic`, then **Clear authentication**.

## If it does not work

| What you see | What to do |
| --- | --- |
| `plugin:quotalogic:quotalogic` is not in `/mcp` | Close `claude` (`/exit`) and start it again. If that does not help: `claude plugin marketplace update quotalogic-plugins` and start `claude` again |
| Status `needs sign-in` | Choose the server, then **Authenticate** |
| The menu shows **Re-authenticate**, not **Authenticate** | You are already signed in. To sign in again, choose **Re-authenticate** or **Clear authentication** |
| The marketplace cannot be added | Check that the repository is public and repeat the command |
