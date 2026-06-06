---
name: glance-view
description: Turn a result, dataset, comparison, analysis, or long research synthesis into a custom interactive HTML view instead of a wall of text. Use when the output is multi-dimensional, explorable, or decision-supporting — data analysis, option comparisons, dashboards, charts, or a big synthesis made digestible. Renders a self-contained page into .tmp/glance/ and links it in chat. Invoke with /glance-view, or when the user asks to "visualise", "render", "make a view/page/dashboard of", or "show me" something with structure.
---

# Glance

Render the answer instead of typing it. When text would be a wall, Glance builds a bespoke, self-contained, on-brand HTML page for the task and drops it into the workspace's `.tmp/glance/` to open in preview.

**Read `reference/components.md` (the build playbook) and `reference/design.md` (the active design system) before building. The playbook's non-negotiables, gate, library palette, task→component map, and output rules govern everything below.**

## Workflow

1. **Detect the workspace name.** Find the nearest `CLAUDE.md` up the tree and use its project/workspace name (or the top `# heading`); fall back to the current directory's folder name, title-cased. This goes in the artifact header so the user always knows what they're looking at.
2. **Gate (components.md §4).** Decide: render, plain text, or hybrid. If it's a fact or a ≤2-sentence answer with nothing to sort/compare/tune/trace, **just answer in text** — say so briefly and stop. Don't pay the render tax for a one-liner.
3. **Read the reference.** Load `reference/components.md` + `reference/design.md`. Use the §1 starter `<head>` block verbatim as your skin.
4. **Plan before emitting** (internal): interpret intent → pick the right component(s) from the task→component map → gather the **real** data from context/files (never fabricate; remove anything you can't ground) → brainstorm features → cut to the few that serve the goal.
5. **Build ONE self-contained `index.html`** — the default skin, only curated libraries (components.md §2), required header/footer (§5), responsive, interactive bits actually working, empty states handled.
6. **Write it** to `.tmp/glance/{NN}_{slug}/index.html` (next zero-padded number, kebab slug). Update the gallery at `.tmp/glance/index.html`.
7. **Reply with one line + the clickable link.** Don't duplicate the artifact's content as text. If hybrid, add the 1–3 line takeaway or decision only.

## Hard rules

- **Real data only.** No placeholders, no simulated rows/numbers/quotes. No data for an element → remove the element.
- **Always skinned** with `design.md` via the §1 starter. Green is for CTAs/accents only.
- **Right tool for the task** — don't default to a chart when a sortable table or comparison matrix is the answer.
- **Artifact-first chat.** Render → link → brief. No walls of text restating the page.
- **Self-anneal:** if you can preview + screenshot the result (preview MCP), glance at it and fix obvious breakage before handing off. Frontier models rarely break HTML, but a quick check is cheap.

## To re-skin
Swap `reference/design.md` for another design-tokens file and update the §1 starter tokens in `components.md` to match. Everything inherits it.
