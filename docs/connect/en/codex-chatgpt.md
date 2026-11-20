# Quotalogic plugin for Codex and ChatGPT

**English** · [Русский](../ru/codex-chatgpt.md) · [中文](../cn/codex-chatgpt.md)

## What it is

The plugin helps you load a price list, change prices, show them in another currency, or build a commercial proposal. Before an import the assistant shows what it is about to load and waits for a separate yes. For some actions, such as creating a commercial proposal, your direct request is enough.

In Codex the plugin also adds the connection to Quotalogic (for now, the staging server): after installing, you sign in to your Quotalogic account once. In ChatGPT in the browser the connection is still set up separately.

## Where do you work?

| Where you work | What to read |
| --- | --- |
| Codex in a terminal, or Codex in the ChatGPT desktop app | [Codex on your computer](#codex-on-your-computer) |
| ChatGPT in the browser, regular Chat or cloud Work | [ChatGPT web](#chatgpt-web) |

## What you need

1. **A Quotalogic account.** In Codex the plugin adds the connection itself; do not connect Quotalogic separately. In ChatGPT in the browser connect Quotalogic as a separate app.
2. **The plugin.** Without GitHub access, ask the Quotalogic team for the file `quotalogic-plugins.zip`. With access to the private repository you can install from GitHub. In ChatGPT in the browser the workspace admin adds the plugin for the team.

## Codex on your computer

This guide uses a terminal. Run `codex --version`. If the command is not found, install the Codex CLI first ([OpenAI guide](https://learn.chatgpt.com/docs/cli)).

### Install from the archive (no GitHub)

1. Unzip `quotalogic-plugins.zip` to a permanent place, for example your `Documents` folder. You get a folder named `quotalogic-plugins`.
   **Do not delete or move this folder after installing.**
2. Add the folder. The example uses the `Documents` folder on macOS; if you unzipped elsewhere, use your own path.

   ```bash
   codex plugin marketplace add ~/Documents/quotalogic-plugins
   ```

3. Install the plugin.

   ```bash
   codex plugin add quotalogic@quotalogic-plugins
   ```

4. Continue with Sign in to Quotalogic.

To update, you need a new archive from the Quotalogic team.

### Install from GitHub

1. Sign in to GitHub in the browser and open the [plugin repository](https://github.com/intergalactic-planb/plugins). If you see a 404 error, ask the Quotalogic team for access or for the archive.
2. Sign in to GitHub from the terminal (needs [GitHub CLI](https://cli.github.com/)):

   ```bash
   gh auth login
   gh auth setup-git
   ```

   In the first command choose GitHub.com, then HTTPS, and sign in through the browser with an account that has access to the repository. The second command lets Git use this sign-in. `gh auth status` shows which account you are signed in with.
3. Add the plugin catalog.

   ```bash
   codex plugin marketplace add intergalactic-planb/plugins
   ```

4. Install the plugin.

   ```bash
   codex plugin add quotalogic@quotalogic-plugins
   ```

### Sign in to Quotalogic

Once, after installing, in the terminal:

```bash
codex mcp login quotalogic
```

A browser opens: sign in to Quotalogic, check the workspace, and confirm access. The terminal shows `Successfully logged in to MCP server 'quotalogic'`.

The assistant works in the workspace that was open in the browser when you confirmed access. To switch, change the workspace in the browser and run the command again.

**If you connected Quotalogic by hand earlier** (for example under the name `quotcat`), you now have two identical connections. Remove the manual one:

```bash
codex mcp remove quotcat
```

Check: `codex mcp list` shows one line `quotalogic` with the Quotalogic address.

### Check that it works

1. Check that the plugin is installed.

   ```bash
   codex plugin list --marketplace quotalogic-plugins
   ```

   The line `quotalogic@quotalogic-plugins` should say `installed, enabled`.
2. Open a **new** Codex chat in a separate folder. In the ChatGPT desktop app, restart the app first and choose **Codex**.
3. Type `$` and choose the import scenario `quotalogic:catalog-import`.
4. Put a small test price list in the folder and write: "Prepare an import of this price list into a new Quotalogic catalog and show the preview. Stop before running the import." The preview should show the columns and the currency. The check ends here.

In everyday work just describe the task in your own words; the assistant picks the scenario itself.

### Update

- Installed from **GitHub**: run

  ```bash
  codex plugin marketplace upgrade quotalogic-plugins
  codex plugin remove quotalogic@quotalogic-plugins
  codex plugin add quotalogic@quotalogic-plugins
  ```

- Installed **from the archive**: ask for a new file. Remove the old version (see Remove), unzip the new archive, and install again.

A chat that is already open does not get the new version: start a new one.

**Nothing updates by itself.** Codex and the ChatGPT desktop app have no plugin auto-update; according to OpenAI's documentation the catalog is refreshed only by `codex plugin marketplace upgrade`. So we announce new versions separately. The exception is ChatGPT web with a catalog added by an admin: it refreshes daily (see Update in ChatGPT web).

### Remove

```bash
codex plugin remove quotalogic@quotalogic-plugins
codex plugin marketplace remove quotalogic-plugins
```

Check `codex mcp list`: the `quotalogic` connection should be gone. The access you granted stays in Quotalogic itself; revoke it on the page with your connections in Quotalogic. If you installed from the archive, you can now delete the `quotalogic-plugins` folder.

## ChatGPT web

**Access to catalogs through the browser is not confirmed yet**, and it is not confirmed that ChatGPT web uses the connection bundled in the plugin. Before testing the scenarios, connect the Quotalogic app separately and check that ChatGPT sees your catalogs.

If you are a team member, ask the admin of your ChatGPT workspace to add the plugin and give you access. Members do not need GitHub access to install a plugin the admin already added.

If you use a personal ChatGPT account, install from the archive with [Codex on your computer](#install-from-the-archive-no-github).

### 1. Admin: add the plugin

**From GitHub.** Sign in to GitHub with an account that can read the private repository `intergalactic-planb/plugins`. The same account is used for updates.

1. Open **Admin**, then **Plugins**, then **Add**, then **Import marketplace**.
2. In **Source** enter `https://github.com/intergalactic-planb/plugins`.
3. Leave **Path** empty.
4. In **Branch, tag, or commit** enter `main`.
5. Click **Import marketplace** and let ChatGPT access GitHub when asked. Check **Import results**. ([Import from GitHub](https://learn.chatgpt.com/docs/enterprise/plugin-management))

**From a ZIP.** If the admin console has **Plugins**, then **Add**, then **Upload plugin**, OpenAI allows uploading a plugin file. We have not tested uploading our Codex archive this way. ([ZIP upload in ChatGPT](https://help.openai.com/en/articles/20001256-plugins-in-chatgpt))

Then open the plugin card. In **Installation policy** choose **Available** so members can install it themselves, or **Installed** to install it for them. Give the same members access to the Quotalogic data app.

### 2. Member: install

1. In **Plugins** open the tab named after your workspace and choose the Quotalogic plugin.
2. Click **+** if the plugin is not installed yet.
3. If access to catalogs is not set up yet, sign in to Quotalogic through the data app.
4. Start a new chat.

### 3. Check

Type `@`, choose the Quotalogic plugin or `catalog-import`, and attach a test price list. Ask: "Prepare an import of this price list into a new Quotalogic catalog and show the preview. Stop before running the import." The preview should show the columns and the currency. ([Choosing a plugin in ChatGPT](https://learn.chatgpt.com/docs/plugins#choose-a-specific-plugin))

**Desktop only** on a plugin means it needs the desktop app. If Quotalogic is missing from the `@` menu in a new chat, ask the admin to check your access to the plugin and to the Quotalogic connection.

### Update in ChatGPT web

- A GitHub catalog is checked once a day. To update now, in **Admin**, then **Plugins**, then **Marketplaces**, choose the catalog and click **Sync now**. Check the result for Quotalogic in the report. If the admin lost GitHub access, restore it or let another admin import again. ([Updates from GitHub](https://learn.chatgpt.com/docs/enterprise/plugin-management#keep-plugins-up-to-date))
- For a plugin uploaded from a ZIP, open its card and choose **Upload new version**, if available. ([Updates from a ZIP](https://help.openai.com/en/articles/20001256-plugins-in-chatgpt))

Start a new chat after updating.

### Remove from ChatGPT web

A member removes Quotalogic on its card in **Plugins**, if workspace rules allow it. The admin can make the plugin unavailable for roles. Deleting the whole catalog in **Admin**, then **Plugins**, then **Marketplaces** removes every plugin added from it. The Quotalogic data app stays and is managed separately.

## Troubleshooting

| What you see | What to do |
| --- | --- |
| Access error or `not found` when adding from GitHub | Check that the terminal is signed in with an account that has access, and that you ran `gh auth setup-git`. Or ask for the archive |
| Path not found when installing from the archive | Check that the folder is still there and the path in the command is right |
| Installing from the archive asks for GitHub access | Ask the Quotalogic team for an archive built for installing without GitHub |
| `quotalogic` is missing after typing `$` | Restart the app, choose Codex, and start a new chat. In the terminal check `codex plugin list --marketplace quotalogic-plugins` |
| Two identical connections in `codex mcp list` | Remove the manual one: `codex mcp remove quotcat` (the name from the list, not `quotalogic`) |
| The assistant says there are no Quotalogic tools | In Codex run `codex mcp login quotalogic`. In ChatGPT web the admin can check that the data app is available to your account |

## For the team: building and testing

**Build the archive** from a clone of this repository (built from what is committed to `main`). For Codex one file in the archive must be replaced: in the repository the Codex catalog points to GitHub, so without the swap an install from the archive still goes to the private repository. Tested: with the swap the plugin installs from the folder without GitHub; without it Codex downloads from GitHub. The same archive also works for Claude Code.

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

Put the plugin version (from `.claude-plugin/plugin.json`) in the archive name or in your message to the customer.

**Check the build** in a clean folder with separate Codex settings, so yours stay untouched:

```bash
mkdir -p ~/archive-test && cd ~/archive-test && unzip -q ~/quotalogic-plugins.zip
export CODEX_HOME=~/archive-test/codex-home && mkdir -p "$CODEX_HOME"
codex plugin marketplace add ~/archive-test/quotalogic-plugins
codex plugin add quotalogic@quotalogic-plugins
codex plugin list --marketplace quotalogic-plugins
codex mcp list
```

The `SOURCE` column of the plugin line must show the folder path, not the GitHub address. `codex mcp list` must show `quotalogic` (Codex reads the server from `.mcp.json`; a root `mcp.json` is not picked up).

**Known backend bug when checking an import.** A `SUCCESS` status is not enough: the backend can store a fractional price one cent lower (`19.99` as `19.98`) without a warning. To test the plugin itself, use a whole-number price, for example:

```csv
SKU,Name,Price
WEB-05-1,Web Work Test,20
```

To try a real import in a test workspace, ask to create a catalog `Web Work Test` in EUR, map `SKU` to the code, `Name` to the name, `Price` to the main price, show the table, and stop before a separate yes. After the yes, ask it to read the product again and compare the values with the file.
