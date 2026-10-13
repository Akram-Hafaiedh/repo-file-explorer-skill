# repo-file-explorer

A Claude skill that clones a GitHub repository and publishes a **read-only, single-file HTML file explorer** as an artifact.

## What it does

- Embeds the project's text files in the page (inline JSON). Skips `.git`, `node_modules`, `.next`, lockfiles and binary files.
- Collapsible folder tree on the left (folders first, alphabetical, open file highlighted).
- File viewer on the right with line numbers, monospace font and scrolling.
- Top bar with the open file's path and a **Copy** button.
- Filter box that searches file names.
- A **modified** badge on files Claude has changed.
- Automatic light/dark mode, usable on mobile, no external dependencies.
- After each round of edits, republishes the same page (same URL) with updated files and marks what changed.

## Usage

Once installed, just ask:

> Clone https://github.com/owner/repo and publish a read-only file explorer of it.

The skill works best where Claude can use the Artifact tool and clone from GitHub.

## Install

### Option A: Claude.ai (web / desktop / mobile)

1. Download this repo, or grab `repo-file-explorer.zip` from the [Releases](../../releases) page. If you downloaded the repo, zip the folder `plugins/repo-file-explorer/skills/repo-file-explorer` (the zip must contain that `repo-file-explorer` folder with `SKILL.md` inside).
2. In Claude, open **Settings → Capabilities → Skills** (some versions: **Settings → Features**) and click **Upload skill**.
3. Select the zip. If it is rejected, re-zip with `SKILL.md` at the top level of the archive instead of inside a folder.

Custom skills are per-user and don't sync across surfaces: install it on each surface where you want it.

### Option B: Claude Code, as a plugin

```bash
claude plugin marketplace add Akram-Hafaiedh/repo-file-explorer-skill
claude plugin install repo-file-explorer@repo-file-explorer-skill
```

Or inside a session: `/plugin marketplace add Akram-Hafaiedh/repo-file-explorer-skill`, then `/plugin install repo-file-explorer@repo-file-explorer-skill`.

### Option C: Claude Code, manual copy

```bash
git clone https://github.com/Akram-Hafaiedh/repo-file-explorer-skill.git
cp -r repo-file-explorer-skill/plugins/repo-file-explorer/skills/repo-file-explorer ~/.claude/skills/
```

## Repo layout

```
.claude-plugin/marketplace.json          # makes this repo a Claude Code marketplace
plugins/repo-file-explorer/
  .claude-plugin/plugin.json             # plugin manifest
  skills/repo-file-explorer/SKILL.md     # the skill itself (single source of truth)
```

## Contributing

Issues and pull requests are welcome. Please keep `SKILL.md` as the only copy of the instructions.

## License

MIT. See [LICENSE](LICENSE).
