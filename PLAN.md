# Aline (Decision Tree Builder) — Build Plan

**One-liner:** A visual, drag-friendly decision-tree editor for building branching flows — support scripts, onboarding decision guides, chatbot logic, training/troubleshooting trees — that exports clean, embeddable output.

## Why this ranks #4
- Working prototype (`decision-tree-builder.html`, 787 lines) already has the hard interactive parts done: node add/delete, auto-layout (`layoutPositions`), connection toggling, label editing, live SVG rendering (`buildSvgMarkup`/`renderSvg`), and a settings/color system.
- Clear, recurring use cases with willing-to-pay buyers: customer support teams (troubleshooting scripts), HR/onboarding (decision guides), sales (qualification flows), chatbot/IVR designers (flow logic before wiring to an actual bot).
- Smaller ceiling than #1-3 (single-purpose tool, not a platform) but also the fastest path to a shippable, monetizable v1 since the core editor already works — lowest execution risk of the five.

## Current state (from prototype inspection)
- Single HTML file, fully client-side, no backend, no persistence beyond in-memory state (confirm: no `localStorage` calls seen in the function list, unlike Tappist — likely loses work on refresh today).
- Core functions present: `addNode`, `deleteNode`, `updateLabel`, `toggleConnection`, `seedTree`, `layoutPositions`, `renderEditor`, `renderSvg`, `buildSvgMarkup`, `applyColors`, `initSettingsInputs`.
- Output is SVG — good for embedding/export, but no interactive "play through the tree" mode yet (useful for support agents actually using a finished tree, not just building one).

## MVP definition (what's missing to ship v1)
1. ~~**Persistence**~~ — **done.** localStorage-backed multi-tree save/load/rename/delete, autosaved 400ms after each edit (see `📁 Trees` panel and `saveStatus` indicator in `index.html`).
2. **Export formats** — SVG/HTML export already worked; ~~JSON export/import~~ **done** (round-trips through the same tree schema, imports as a new saved tree). PNG export still not implemented.
3. ~~**"Play mode"**~~ — **done.** Full-screen click-through viewer (`▶ Play`): one node at a time, choices rendered as buttons from `children`, breadcrumb of the path taken, Back/Restart, auto-enters when there's a single root.
4. **Multi-choice branches** — `toggleConnection` already supported N-way branching (checkbox per connection, not radio), so this was already true going in; no change needed.

## Phased roadmap
**Phase 1 — Make the prototype durable — done**
- localStorage persistence: multiple named trees, load/rename/delete via the Trees panel.
- JSON export/import for portability (import creates a new tree rather than overwriting the open one).
- Confirmed multi-branch support per node was already in place.

**Phase 2 — Add "Play mode" — done (local only)**
- Play mode implemented: one node at a time, choices as buttons, path breadcrumb, Back/Restart.
- **Still open:** a shareable *read-only link* per tree (needs a lightweight backend — Supabase: `trees(id, owner_id, data jsonb, is_public)`) so Play mode can be sent to someone who isn't opening this editor themselves. This is the next real gap — everything above runs only inside the local browser tab.

**Phase 3 — Embeddability + monetization (1 week)**
- Embed widget: `<iframe>` or small JS snippet to drop a published tree's Play mode into any website/help center.
- Free tier: N trees, local-only. Paid tier: shareable/embeddable trees, team collaboration on a shared tree, analytics on which paths users take through a published tree (valuable for support teams optimizing their scripts).

**Phase 4 — Collaboration (stretch)**
- Multi-user editing on the same tree (real-time via Supabase Realtime or similar) — worth it only once there's evidence teams (not just individuals) are the buyer.

## Key risks
- Crowded-ish space (Lucidchart, Miro, Whimsical, various flowchart tools, and dedicated "decision tree" SaaS like DecisionRules) — differentiation has to be the "Play mode" + embeddability angle (a *runnable*, embeddable tree for an end audience), not "yet another node editor."
- Without Play mode this is just a diagramming tool competing with much bigger general-purpose players — Phase 2 is not optional, it's the actual product wedge.

## Immediate next step
Persistence, JSON export/import, and local Play mode are done. The next real gap is the shareable read-only link from Phase 2 — without it, Play mode only helps the person who built the tree, not the audience it's meant for (a support agent or new hire who doesn't have this editor open). That needs a minimal backend (Supabase `trees` table + a static Play-mode-only route), which is also the prerequisite for Phase 3's embed widget.
