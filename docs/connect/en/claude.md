# Quotalogic plugin for Claude

**English** · [Русский](../ru/claude.md) · [中文](../cn/claude.md)

## What it is

The plugin teaches Claude to work with Quotalogic: load a price list into a catalog, change prices, show prices in another currency, build and send a commercial proposal. Claude picks the right scenario by itself, always shows what it is about to do, and writes nothing until you say yes.

The plugin also adds the connection to Quotalogic (for now, the staging server). After installing, you sign in to your Quotalogic account once. You do not need to connect Quotalogic separately.

## Choose your path

**1. Where do you use Claude?**

- **Code**: the `claude` program in a terminal, or the **Code** tab of the Claude Desktop app. See part 1.
- **Chat or Cowork**: claude.ai, or the **Chat** tab of the Claude Desktop app. See part 2.

**2. What plan do you have?**

- **Personal** (Pro, Max): you do everything yourself.
- **Company** (Team, Enterprise): for Chat and Cowork the organization Owner usually adds the plugin (part 3); you then install it or find it already installed.
- **Free**: plugins cannot be installed on this plan.

| Where you work | Personal plan | Company plan |
| --- | --- | --- |
| Code (terminal, Code tab) | part 1 | part 1, or the Owner rolls it out to everyone (part 3) |
| Chat and Cowork | part 2 | part 3 (Owner), then part 2 |

**Not yet tested by us:** everything in parts 2 and 3 (Chat, Cowork, organization settings) follows Anthropic's documentation; menu names may differ. In part 1 we tested installing, including from an archive, and that the connection appears in `/mcp` asking for sign-in.

## What you need

1. **A Quotalogic account.** The plugin adds the connection itself. If you already connected Quotalogic by hand earlier, leave it: Claude keeps only one connection to the same address.
2. **The plugin**, one of two ways:
   - **From an archive.** We send you a file; no GitHub access needed.
   - **From GitHub.** You need read access to the private repository `intergalactic-planb/plugins`.

Not sure? Take the archive: it is simpler. There are two archive files, because the layout inside differs:

- `quotalogic-plugins.zip` for part 1 (Code);
- `quotalogic-plugin-claude.zip` for parts 2 and 3 (Chat, Cowork, organization upload).

---

## Part 1. Code (terminal and Code tab)

Check that Claude is installed:

```bash
claude --version
```

### Option B: from the archive (simpler)

1. Unzip `quotalogic-plugins.zip` to a permanent place, for example your `Documents` folder. You get a folder named `quotalogic-plugins`.
   **Do not delete or move this folder after installing**: Claude reads the plugin from it.
2. In a terminal, add the folder (macOS example; use your own path if you unzipped elsewhere):

   ```bash
   claude plugin marketplace add ~/Documents/quotalogic-plugins
   ```

   You should see `Successfully added marketplace: quotalogic-plugins`.
3. Install the plugin:

   ```bash
   claude plugin install quotalogic@quotalogic-plugins
   ```

   You should see `Successfully installed plugin: quotalogic@quotalogic-plugins`.

**Note:** an archive is a snapshot of the day it was sent. When we improve the plugin, you keep the old version until you get a new archive.

### Option A: from GitHub

1. **Repository access.** Open https://github.com/intergalactic-planb/plugins. If the page opens, you have access. A 404 error means no access: ask us for access, or use the archive.
2. **Sign in to GitHub on your computer** (needs [GitHub CLI](https://cli.github.com/)):

   ```bash
   gh auth login
   gh auth setup-git
   ```

   In the first command choose GitHub.com, then HTTPS, then sign in through the browser. `gh auth status` should show `Logged in to github.com`.
3. Add the plugin catalog (once per computer):

   ```bash
   claude plugin marketplace add intergalactic-planb/plugins
   ```

4. Install the plugin:

   ```bash
   claude plugin install quotalogic@quotalogic-plugins
   ```

**The Code tab of Claude Desktop** reads the same settings as the terminal: a plugin installed in the terminal is already there in the app. Adding the plugin catalog (step 3) can only be done in the terminal; after that you can also install with buttons: **+** next to the message box, then **Add plugins**.

### Sign in to Quotalogic

Once, after installing.

1. Create a separate empty folder and start Claude there. Do not start Claude in a folder with code or secrets.

   ```bash
   mkdir -p ~/quotalogic && cd ~/quotalogic && claude
   ```

   If Claude was running while you installed, exit (`/exit`) and start it again.
2. Type `/mcp`, choose `plugin:quotalogic:quotalogic`, then **Authenticate**.
3. A browser opens: sign in to Quotalogic, check the workspace, and confirm access.
4. In `/mcp` the connection now shows **connected**.

Claude works in the workspace that was open in the browser when you confirmed access. To switch, change the workspace in the browser and run **Authenticate** again. The sign-in lasts up to 30 days without use.

### Check that it works

1. Ask "Show my Quotalogic catalogs". You should get the list from your workspace.
2. Type `/`. The list should contain four lines starting with `/quotalogic:`: `catalog-import`, `catalog-edit`, `currency-conversion`, `offer`.
3. Put a small price list in the folder and ask in your own words: "Create a catalog from prices.xlsx". Claude shows how it understood the columns and the currency and waits for your yes. Nothing is written before you say yes.

### Update

- **Option A:**

  ```bash
  claude plugin update quotalogic@quotalogic-plugins
  ```

  The new version loads the next time Claude starts.

  **Auto-update:** in Claude type `/plugin`, open the **Marketplaces** tab, choose `quotalogic-plugins`, then **Enable auto-update**. For the private repository this works only after step 2 of option A (`gh auth login` and `gh auth setup-git`); a token in an environment variable alone is not enough. Without a saved sign-in the update silently does not happen and the old version stays. Claude checks for updates some minutes after the first message in a session; the new version applies after `/reload-plugins` or on the next start.
- **Option B:** nothing updates by itself. When you get a new archive, remove the plugin and the folder (see Remove), unzip the new archive, and install again.

### Remove

```bash
claude plugin uninstall quotalogic@quotalogic-plugins
claude plugin marketplace remove quotalogic-plugins
```

The connection to Quotalogic goes away together with the plugin. The access you granted stays in Quotalogic itself; revoke it on the page with your connections in Quotalogic. If you installed from the archive, you can now delete the `quotalogic-plugins` folder.

---

## Part 2. Chat and Cowork (claude.ai and the Chat tab of the app)

For personal plans. On Team or Enterprise, read part 3 first: the plugin may already be added by the Owner, or uploads may be turned off for your organization.

### Option B: upload the archive

1. Get the file `quotalogic-plugin-claude.zip` from us (the `.claude-plugin` folder is at the top of the archive, with no outer folder).
2. In Claude open **Customize**, then **Plugins**, click **Add**, then **Upload plugin**, and choose the file. The limit is 200 MB.
3. The plugin appears in the list; turn it on.
4. Connect Quotalogic (see below).

### Option A: from GitHub

1. In **Customize**, then **Plugins**, click **Add**, then **Add marketplace**.
2. Enter `intergalactic-planb/plugins`.
3. When asked, connect your GitHub account and give the Claude GitHub app access to this repository. The account needs access to the repository.
4. Find `quotalogic` and install it.
5. Connect Quotalogic (see below).

### Connect Quotalogic

In Chat and Cowork the connection from the plugin does not turn on by itself.

1. Open the plugin in **Customize**, then **Plugins**, and its **Connectors** tab.
2. Click **Connect** next to Quotalogic, sign in to Quotalogic, and confirm access.
3. On Team and Enterprise the button appears after the Owner adds this connector for the organization (part 3).

If Quotalogic is already connected in **Settings**, then **Connectors**, you do not need to connect it again.

### Company plan

The plugin may already be in your list in **Customize**, then **Plugins**. If it is not there and **Add** offers nothing, your organization turned uploads off: ask the Owner (part 3).

### Check

In a new chat attach a small price list and write: "Create a catalog from this file". Claude should show how it understood the columns and wait for your yes. If it says there are no Quotalogic tools, the connection is not signed in: see Connect Quotalogic above.

---

## Part 3. Organization Owner (Team and Enterprise)

Only the organization **Owner** can roll out the plugin. A regular member cannot.

### For Chat and Cowork

1. Open **Organization settings**, then **Plugins & skills**, and click **Add**.
2. Choose one:
   - **Upload plugin**: upload `quotalogic-plugin-claude.zip` (no GitHub needed; good for a trial).
   - **Sync from GitHub**: connect the repository `intergalactic-planb/plugins`. The repository must be private or internal (ours is private). Updates go through the organization's GitHub connection; members do not need their own GitHub sign-in. Sync runs with **Re-sync**, or by itself on every change of the main branch if you turn on **Sync automatically** (this needs a webhook in GitHub).
3. Set **Default access**:
   - **Not available**: nobody;
   - **Available to install**: everyone installs it themselves;
   - **Installed by default**: installed for everyone, can be removed;
   - **Required**: installed for everyone, cannot be removed.

   On Enterprise you can give the plugin to selected user groups.
4. Add the Quotalogic connector from the plugin for the organization (on the plugin's **Connectors** tab). After that each member connects it with their own Quotalogic account.
5. To stop members from adding their own plugins, turn off **User-created skills** on the **Policy** tab. Turning off **Skills** itself stops all organization plugins.

### For Code (terminal and Code tab)

Roll out the plugin to everyone in **Organization settings**, then **Claude Code**, then **Managed settings**. Each member needs read access to the private repository, or use the archive. The plugin appears at the start of the member's next Claude session. To keep it updated for everyone, set `"autoUpdate": true` on the catalog; members then cannot turn auto-update off:

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

This format comes from Anthropic's documentation on organization plugins (`code.claude.com/docs/en/plugins/org.md`); we have not tested it.

An organization can also forbid members to add their own plugin catalogs (the `strictKnownMarketplaces` setting). Then options A and B of part 1 do not work for them: ask the Owner.

---

## Troubleshooting

| What you see | What to do |
| --- | --- |
| `Repository not found` or an access error (option A) | No access to the repository, or not signed in to GitHub: steps 1 and 2 of option A. Or use the archive |
| `Plugin "quotalogic" not found in marketplace` | A typo. The name must be exactly `quotalogic@quotalogic-plugins` |
| Option B: path not found | Check that the folder is still there and the path in the command is right |
| No `/quotalogic:` lines after typing `/` | Restart Claude or type `/reload-plugins`. Errors are in `/plugin`, tab **Errors** |
| Claude says there are no Quotalogic tools | Not signed in: in Code see Sign in to Quotalogic (part 1), in Chat see Connect Quotalogic (part 2). `/mcp` should show **connected** |
| No **Add** or **Upload plugin** in Chat | Your organization turned uploads off, or your plan does not allow plugins: ask the Owner |
| The archive upload is refused in Chat | The `.claude-plugin` folder must be at the top of the archive (no outer folder), and there must be no top-level `bin` folder. Use `quotalogic-plugin-claude.zip` |
| In Code `/plugin` does not open or the catalog cannot be added | Your organization forbids your own catalogs: ask the Owner |

## For the team: building and testing

**Two archives from a clone of this repository** (built from what is committed to `main`):

1. `quotalogic-plugins.zip`: for part 1 and for Codex. Build it with the commands in [codex-chatgpt.md](codex-chatgpt.md#for-the-team-building-and-testing); the Codex file swap there changes nothing for Claude.
2. `quotalogic-plugin-claude.zip`: for parts 2 and 3. The content must be at the top of the archive:

   ```bash
   git -C ~/Projects/plugins archive --format=zip main -o ~/quotalogic-plugin-claude.zip
   ```

Requirements from Anthropic's documentation: one `.claude-plugin/plugin.json` at the top, at most 200 MB, no top-level `bin` folder with programs. **We have not yet tested uploading our archive to claude.ai or to organization settings.** Tell the customer the plugin version (from `.claude-plugin/plugin.json`).

**Test as a user without touching your own settings** (part 1): install only into a test folder.

```bash
mkdir -p ~/quotalogic-plugin-test && cd ~/quotalogic-plugin-test
claude plugin marketplace add intergalactic-planb/plugins --scope local
claude plugin install quotalogic@quotalogic-plugins --scope local
```

Settings go to `.claude/settings.local.json` of that folder. To test an unpushed version, use the path of your clone (for example `~/Projects/plugins`) instead of `intergalactic-planb/plugins`. To clean up, delete the test folder.
