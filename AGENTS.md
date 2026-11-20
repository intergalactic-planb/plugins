# Repository instructions

The `quotalogic` plugin for Claude, Codex and ChatGPT: skills in `skills/`, the server connection in `.mcp.json`, install guides in `docs/connect/`. The server and the rules for writing skills live in the `mcp` repository (cloned next to this one as `../mcp`, see its `AGENTS.md`, section "Agent skills").

## Every change bumps the version

Any change to this repository, however small (a skill, a reference, `.mcp.json`, a manifest, the README, a guide in `docs/`), bumps the plugin version in the same change. Installed plugins update only when the version changes, so a change without a bump never reaches users.

1. Raise the patch number (`0.5.29` → `0.5.30`) unless the owner asks for another.
2. Set the same value in both manifests:
   - `.claude-plugin/plugin.json`
   - `.codex-plugin/plugin.json`
3. Check that the two match:

   ```bash
   grep -h '"version"' .claude-plugin/plugin.json .codex-plugin/plugin.json
   ```

   Both lines must be identical. No other file holds a version; do not add one.

## Install guides in three languages

`docs/connect/` holds the guides for customers in three languages, one folder each: `en`, `ru`, `cn` (Simplified Chinese), with the same files and the same sections in the same order. Guides are for customers: short steps, no reasoning, no notes about what was or was not tested. Customers install only from this GitHub repository; the guides offer no other way.

- A change to a guide in one language is made in the other two in the same change: same steps, commands, warnings, tables and links. Never leave one language ahead of the others.
- A new guide is added in all three folders, and listed in the table in `README.md` and in the language line at the top of each guide.
- Commands, file names, menu names of the apps and code blocks stay the same in all languages; only the prose is translated.
- Keep links between guides working: a link to a heading uses that heading's anchor in the same language.

## Naming

The product is called Quotalogic everywhere in this repository. Never mention Quotcat (as a name, a server name like `quotcat`, or a note that the two are the same) in any file or language. Where a guide needs the name of a connection the user added by hand, use a placeholder (`<name>`) and tell the user where to read it (`codex mcp list`). Check before you finish:

```bash
grep -rni quotcat --exclude-dir=.git --exclude=AGENTS.md .
```

The command must print nothing.

## Language

Everything outside `docs/connect/` is English only: skills, references, manifests, `.mcp.json`, README. Not one Russian word (examples, trigger phrases, sample replies). Before you finish, scan for Cyrillic outside `docs/connect/`:

```bash
git ls-files -co --exclude-standard | grep -v '^docs/connect/' | xargs perl -CSD -ne 'print "$ARGV:$.\n" if /\p{Cyrillic}/; close ARGV if eof'
```

The command must print nothing.

## Before you finish

1. Version bumped and identical in both manifests.
2. If a guide changed: the same change in `en`, `ru` and `cn`.
3. No Cyrillic outside `docs/connect/`.
4. No mention of Quotcat.
5. `claude plugin validate .` passes (warnings about author and icon are known).
6. If `../mcp` is cloned: run `bun test` there; it checks that every tool named in the skills exists on the server.
