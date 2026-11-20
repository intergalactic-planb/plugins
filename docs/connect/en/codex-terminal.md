# Codex in a terminal

[English](codex-terminal.md) · [Русский](../ru/codex-terminal.md) · [中文](../cn/codex-terminal.md)

For `codex` in a terminal. For the ChatGPT app: [ChatGPT app](codex-desktop.md).

The `intergalactic-planb/plugins` repository must be public.

## Install

In a terminal run, one after the other:

```bash
codex plugin marketplace add intergalactic-planb/plugins
```

```bash
codex plugin add quotalogic@quotalogic-plugins
```

## Sign in to Quotalogic

1. Run:

   ```bash
   codex mcp login quotalogic
   ```

2. The browser opens. Sign in to Quotalogic, check the workspace and confirm.
3. The terminal prints `Successfully logged in to MCP server 'quotalogic'`.
4. Check the sign-in: `codex mcp list`. In the `Auth` column `quotalogic` shows `OAuth`.
5. Start `codex` and ask: "Show my catalogs". A list of your catalogs appears.

## Update

```bash
codex plugin marketplace upgrade
```

```bash
codex plugin add quotalogic@quotalogic-plugins
```

Then close `codex` and start it again.

## Turn off and remove

If you also want to reset the sign-in, first run:

```bash
codex mcp logout quotalogic
```

Then remove the plugin and the marketplace:

```bash
codex plugin remove quotalogic@quotalogic-plugins
```

```bash
codex plugin marketplace remove quotalogic-plugins
```

## If it does not work

| What you see | What to do |
| --- | --- |
| In `codex mcp list` the `Auth` column of `quotalogic` shows `Not logged in` | `codex mcp login quotalogic` |
| Codex says there are no Quotalogic tools | Check `codex mcp list`: `quotalogic` must be `enabled` and `OAuth`. If not, sign in (section "Sign in to Quotalogic"). Then start `codex` again |
| `codex plugin list` prints a very long list | Show only the marketplace you need: `codex plugin list --marketplace quotalogic-plugins` |
| `codex mcp list` shows two lines with the Quotalogic address | You added one of them by hand earlier. Remove it: `codex mcp remove <name>`, where `<name>` comes from `codex mcp list` (not `quotalogic`) |
