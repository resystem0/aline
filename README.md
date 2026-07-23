# Aline — Decision Tree Builder

A visual, row-based decision-tree editor for building branching flows: support
scripts, onboarding decision guides, chatbot logic, training/troubleshooting
trees. Runs entirely in the browser — no backend, no build step, no
dependencies.

**Live app:** https://resystem0.github.io/aline/

## What it does

- Build a tree row by row: each row is a layer of nodes, each node can connect
  to any subset of nodes in the row below it (multi-branch, not just
  yes/no).
- Live SVG canvas that re-renders as you edit — click a row band or a node on
  the canvas to jump straight to it in the editor panel.
- Full style control (title bar colors, canvas background, connector color,
  node fill/border/text, corner radius) via the settings popover.
- One-click export: produces a standalone, self-contained HTML file with all
  colors resolved to literal values — safe to email, embed, or drop into a
  help center article with no dependency on this app.

## Usage

Open `index.html` directly in a browser, or use the hosted copy at the link
above. No install, no server, no account.

1. Edit the title at the top left.
2. Use **+ Add row** / **− Remove last row** to shape how many layers (steps)
   the tree has.
3. Within a row, use **+ Add node to this row** to add branches; type a label
   directly into each node's text field.
4. Under each node, toggle which nodes in the *next* row it connects to.
5. Click **🎨 Style** to recolor the whole tree.
6. Click **▶ Play** to try the tree the way its audience would: one question
   at a time, click a choice to advance, **← Back** to undo a step, breadcrumb
   across the top shows the path taken.
7. Click **📁 Trees** to see every tree you've built, switch between them,
   rename, or delete one. Every edit autosaves automatically (see "Saved" /
   "Saving…" indicator next to the row count) — refreshing or closing the tab
   never loses work.
8. **JSON ⬇** downloads the tree as re-importable JSON; **JSON ⬆** loads one
   back in (as a new saved tree, so it never overwrites what you had open).
   **⬇ Export** downloads a standalone, self-contained HTML snapshot for
   sharing — colors resolved to literal values, no dependency on this app.

## How it works

- **Storage**: everything lives in the browser's `localStorage` — no
  backend, no network calls, no accounts. Each tree is a small JSON record
  (`{title, colors, layers}`) keyed by an id; the currently open tree is
  autosaved 400ms after your last edit. Clearing site data / browser storage
  for this origin deletes your trees, so export anything you want to keep
  outside the browser.
- **Play mode**: walks `layers[0..N]` one row at a time, treating each
  node's `children` as the choices offered at that step. A leaf node (no
  children) ends the walk. If a tree has more than one node in its first
  row, Play mode asks which one to start from; with a single root it jumps
  straight in.

## Known limitations (current state)

- **Single browser only** — trees live in that browser's localStorage; there's
  no sync/account, so a tree built on one device/browser doesn't show up on
  another. Use JSON export/import to move a tree between them.
- **No sharing / embed link yet** — Play mode only runs inside this editor;
  there's no lightweight hosted link you can send someone who isn't opening
  this app themselves.
- **Export is standalone-only** — the HTML/JSON exports are snapshots for
  sharing or backup, not a live sync target; re-importing JSON always creates
  a new tree rather than updating one in place.

See `PLAN.md` for the fuller roadmap (shareable links, embeddable widget,
multi-user editing).

## License

MIT
