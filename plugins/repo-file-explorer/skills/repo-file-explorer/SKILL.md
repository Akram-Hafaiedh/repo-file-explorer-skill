---
name: repo-file-explorer
description: Clone a GitHub repo and publish a read-only, single-file HTML file explorer as an artifact; republish the same URL after edits, tagging changed files.
---

# Repo file explorer

Turn a GitHub repository into a self-contained, read-only file explorer published as an artifact, and keep it in sync as files change.

## Inputs
- Repository URL (required). If the person didn't give one, or it is still a placeholder like `<URL_DU_DEPOT>` or `<REPO_URL>`, ask for it before doing anything else.
- Optional: extra paths/globs to exclude, branch to use.

## Steps
1. Clone the repo into the workspace (shallow clone is fine). If the clone fails (private repo, network allowlist), say so plainly and ask the person for an alternative (attach a zip, grant access).
2. Collect files:
   - Exclude: `.git`, `node_modules`, `.next`, lockfiles (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `Cargo.lock`, `poetry.lock`, etc.), build output dirs, and binary files (detect via NUL bytes / failed UTF-8 decode, plus common image/font/archive extensions).
   - Skip single files above ~500 KB and note them in the page. Keep the whole page under the 16 MB artifact limit; if it would exceed it, drop the largest files and tell the person.
3. Build ONE self-contained HTML file (no external scripts, styles, fonts, or CDNs). Embed the files as inline JSON (`{ path: content }`). When embedding, escape `</script>` and `<!--` inside the JSON so the page can't break.
4. UI requirements:
   - Left: collapsible folder tree; folders before files; alphabetical; the open file is highlighted.
   - Right: file content with line numbers, monospace font, scrolling (horizontal and vertical).
   - Top bar: path of the open file + a "Copy" button (clipboard API with a textarea fallback; copies raw content without line numbers).
   - Filter input that searches file names (auto-expands matching folders).
   - A "modified" badge next to files that changed since the first publish.
   - Automatic light/dark mode via `prefers-color-scheme` (CSS variables), readable on mobile (tree becomes a drawer or stacked view, 16px gutters, no horizontal page scroll).
   - Render file content with `textContent`, never `innerHTML`.
5. Load the `artifact-design` skill before writing the page, write the HTML to a file, and publish with the Artifact tool. Use a short 2-4 word title (e.g. the repo name + "Explorer").

## After every round of changes
- Keep a list of modified paths (compare against the original clone, e.g. `git status` / `git diff --name-only`).
- Regenerate the embedded JSON with the current file contents and republish to the SAME artifact URL (never create a new one). Mark changed files with the "modified" badge.
- Deliver modified files in full (with SendUserFile), never as a diff.
- Reply with one line on what changed; don't paste the URL again, the card already carries it.

## Notes
- The page is read-only: no editing, no network calls.
- Reply in the language the person writes in.
