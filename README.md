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
above. No install, no server.

1. Edit the title at the top left.
2. Use **+ Add row** / **− Remove last row** to shape how many layers (steps)
   the tree has.
3. Within a row, use **+ Add node to this row** to add branches; type a label
   directly into each node's text field.
4. Under each node, toggle which nodes in the *next* row it connects to.
5. Click **🎨 Style** to recolor the whole tree.
6. Click **⬇ Export** to download a standalone HTML snapshot of the current
   tree.

## Known limitations (current state)

- **No persistence** — refreshing the page loses all edits. There's no
  save/load, no localStorage, no accounts. Treat this as a working prototype,
  not a tool to trust with real work yet.
- **No "play mode"** — the tree only renders as a static diagram today; there's
  no click-through, one-question-at-a-time viewer for someone *following* a
  finished tree (e.g., a support agent working a troubleshooting script).
- **Export is HTML/SVG only** — no PNG or JSON export/import yet, so trees
  aren't portable back into the editor once exported.

See `PLAN.md` for the fuller roadmap (persistence, play mode, embeddable
widget, multi-user editing).

## License

MIT
