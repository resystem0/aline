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
1. **Persistence** — save/load trees (localStorage first, then account-based storage). Losing work on refresh is the single biggest gap in the current prototype.
2. **Export formats** — SVG already works; add PNG export (canvas render of the SVG) and a JSON export/import so trees are portable and re-editable.
3. **"Play mode"** — a read-only interactive viewer where an end user (support agent, new hire) clicks through the tree answering yes/no or multi-choice at each node, rather than seeing the whole tree at once. This is what turns it from an authoring tool into something the *audience* of the tree actually uses day to day — likely the highest-value addition.
4. **Multi-choice branches** — check whether `toggleConnection` currently supports more than binary yes/no; troubleshooting/qualification trees often need 3+ branches per node.

## Phased roadmap
**Phase 1 — Make the prototype durable (1 week)**
- Add localStorage persistence (list of saved trees, load/rename/delete).
- Add JSON export/import for portability.
- Fix/verify multi-branch support per node (not just binary).

**Phase 2 — Add "Play mode" (1-2 weeks)**
- New view mode: render one node at a time with its choices as buttons; track path taken; option to show a summary/breadcrumb of the path at the end.
- This is the feature that makes the tool useful to non-builders (the people *following* the tree, not just the person who made it) — critical for the support-script and troubleshooting use cases.
- Shareable read-only link per tree (needs a lightweight backend — Supabase: `trees(id, owner_id, data jsonb, is_public)`).

**Phase 3 — Embeddability + monetization (1 week)**
- Embed widget: `<iframe>` or small JS snippet to drop a published tree's Play mode into any website/help center.
- Free tier: N trees, local-only. Paid tier: shareable/embeddable trees, team collaboration on a shared tree, analytics on which paths users take through a published tree (valuable for support teams optimizing their scripts).

**Phase 4 — Collaboration (stretch)**
- Multi-user editing on the same tree (real-time via Supabase Realtime or similar) — worth it only once there's evidence teams (not just individuals) are the buyer.

## Key risks
- Crowded-ish space (Lucidchart, Miro, Whimsical, various flowchart tools, and dedicated "decision tree" SaaS like DecisionRules) — differentiation has to be the "Play mode" + embeddability angle (a *runnable*, embeddable tree for an end audience), not "yet another node editor."
- Without Play mode this is just a diagramming tool competing with much bigger general-purpose players — Phase 2 is not optional, it's the actual product wedge.

## Immediate next step
Confirm current branch-count limits in `toggleConnection`/`addNode` (binary vs. N-way), then build Play mode against the existing data model before investing in persistence/backend infrastructure — Play mode is the differentiator and should be de-risked first.
