# Glance — Build Playbook (shared reference)

This is the single source of truth for **how** a Glance artifact is built. Both `/glance-view` (v1, fresh views) and `/glance-board` (v2, the dashboard) read this file. The active design system lives next to this file in `design.md` — swap that file to re-skin everything.

---

## 0. Non-negotiables (read first)

1. **Ground everything in real data.** Never invent numbers, rows, quotes, or status. If you don't have the data for an element, remove the element — don't fake it, don't "// example" it. An empty state is honest; fabricated data is a critical failure.
2. **Gate before you render.** If the answer is a fact, a short explanation, or two sentences with nothing to sort / compare / tune / trace → just answer in text. Generate UI only when the output is multi-dimensional, interactive, explorable, or decision-supporting. (See §4.)
3. **Always skin with the active design system** (`design.md`). Never ship unstyled or generic-Bootstrap output.
4. **Single self-contained file** for `/glance-view` (everything inline or via the curated CDNs in §2). Portable, opens anywhere.
5. **Link the artifact in chat; don't duplicate it as a wall of text.** Once it's rendered, the chat reply is one line + the link, unless it's a hybrid or there's something specific the user needs to read.

---

## 1. The default skin (paste this `<head>` starter)

The default theme is a clean, calm palette: a light canvas, a dark surface for emphasis, and a single green accent, set in Inter. Start every artifact from this exact block, then build the bespoke layout on top. Tokens come straight from `design.md`; do not re-derive them by hand.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Source+Code+Pro:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    /* accent */
    --green:#10b981; --green-deep:#059669; --green-pressed:#047857; --on-primary:#04231b;
    --green-dark:#047857; --green-mid:#34d399; --green-soft:#d1fae5;
    --teal-deep:#0c2926; --teal:#134e4a; --teal-mid:#115e59;
    /* status accents: tags/encoding only, never large fills */
    --purple:#7c3aed; --orange:#ea580c; --pink:#db2777; --blue:#2563eb;
    --warn-bg:#fef3c7; --warn-tx:#92400e;
    /* surfaces */
    --canvas:#ffffff; --canvas-dark:#0c2926; --surface:#f8faf9; --surface-soft:#f1f5f4; --surface-feature:#ecfdf5;
    --hairline:#e3e8e6; --hairline-soft:#eef2f1; --hairline-strong:#cbd5d2; --hairline-dark:#1e3a36;
    /* text */
    --ink:#0c2926; --charcoal:#1e3a36; --slate:#3f5a55; --steel:#5f7a74; --stone:#8aa39d; --muted:#aab9b5;
    --on-dark:#ffffff; --on-dark-muted:#9db8b2;
    /* radii */
    --r-xs:4px; --r-sm:6px; --r-md:8px; --r-lg:12px; --r-xl:16px; --r-xxl:24px; --r-full:9999px;
    /* spacing (4px base) */
    --s-xxs:4px; --s-xs:8px; --s-sm:12px; --s-md:16px; --s-lg:20px; --s-xl:24px; --s-xxl:32px; --s-3xl:40px; --s-section:64px;
    /* elevation */
    --sh-1:0 1px 2px rgba(12,41,38,.04); --sh-2:0 4px 12px rgba(12,41,38,.08);
    --sh-3:0 12px 24px -4px rgba(12,41,38,.12); --sh-4:0 16px 48px -8px rgba(12,41,38,.16);
    /* fonts */
    --font:"Inter",-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;
    --mono:"Source Code Pro","SF Mono",Menlo,Consolas,monospace;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:var(--font);color:var(--ink);background:var(--surface);line-height:1.55;-webkit-font-smoothing:antialiased}
  h1,h2,h3,h4{font-weight:500;letter-spacing:-.02em;line-height:1.2;color:var(--ink)}
  a{color:var(--green-dark);text-decoration:none} a:hover{text-decoration:underline}
  code,pre{font-family:var(--mono)}
  ::selection{background:var(--green);color:var(--on-primary)}
  /* core components */
  .g-btn{font:600 14px/1.3 var(--font);background:var(--green);color:var(--on-primary);border:none;border-radius:var(--r-full);padding:10px 22px;cursor:pointer}
  .g-btn.sec{background:transparent;color:var(--ink);border:1px solid var(--hairline-strong)}
  .g-card{background:var(--canvas);border:1px solid var(--hairline);border-radius:var(--r-lg);padding:var(--s-xl)}
  .g-band{background:var(--teal-deep);color:var(--on-dark);border-radius:var(--r-lg);padding:var(--s-section)}
  .g-badge{display:inline-block;font:600 13px/1.4 var(--font);background:var(--green-soft);color:var(--green-dark);border-radius:var(--r-full);padding:4px 10px}
  .g-eyebrow{font:600 11px/1.4 var(--font);letter-spacing:1px;text-transform:uppercase;color:var(--steel)}
  table.g-table{width:100%;border-collapse:collapse;background:var(--canvas);border:1px solid var(--hairline);border-radius:var(--r-md);font-size:14px}
  table.g-table th,table.g-table td{padding:12px 16px;border-bottom:1px solid var(--hairline-soft);text-align:left}
  table.g-table th{font-weight:600;color:var(--steel);cursor:pointer;user-select:none;font-size:13px}
  table.g-table td.num,table.g-table th.num{text-align:right;font-variant-numeric:tabular-nums}
  table.g-table tbody tr:hover{background:var(--surface)}
  pre.g-code{background:var(--canvas-dark);color:var(--on-dark);border-radius:var(--r-md);padding:var(--s-md);overflow:auto;font-size:14px}
</style>
```

**Skin rules (from `design.md`):** the `--green` accent is for primary CTAs and key highlights only, never body text or large fills. Pills (`--r-full`) for all buttons; 12px (`--r-lg`) for cards. Dark bands frame primary actions. Status accents (purple/orange/pink/blue) are for tags/encoding only. Flat by default; reserve shadows for elevated or floating elements.

---

## 2. Curated library palette (the allow-list)

Reliability comes from a **fixed, trusted set** the model writes well — not improvising obscure libs. Use these via CDN (you're always online locally). Load only what the artifact actually needs.

| Need | Library | CDN |
|---|---|---|
| Charts (bar/line/scatter/pie/radar) | **Chart.js 4** | `https://cdn.jsdelivr.net/npm/chart.js@4` |
| Advanced viz (heatmap, graph, sankey, treemap) | ECharts 5 | `https://cdn.jsdelivr.net/npm/echarts@5/dist/echarts.min.js` |
| Diagrams (flow/tree/sequence/ER) | **Mermaid 11** | `https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js` |
| Markdown → HTML | **marked** | `https://cdn.jsdelivr.net/npm/marked@13/marked.min.js` |
| Code syntax highlighting | highlight.js 11 | js `…/gh/highlightjs/cdn-release@11/build/highlight.min.js` + a CSS theme |
| Math | KaTeX 0.16 | css+js from `https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/` |
| Icons | Lucide | `https://cdn.jsdelivr.net/npm/lucide@latest` |
| Fonts | Google Fonts | Inter + Source Code Pro (in the starter block) |

- **Tables/grids:** hand-build (vanilla) using `table.g-table` — sortable headers + a filter input + CSV export. No grid library needed; it's more reliable and self-contained. Pattern in §6.
- Chart default colors: pull from the palette — series in `--green`, `--teal`, `--purple`, `--orange`, `--blue`; gridlines `--hairline`; text `--steel`.
- **Don't** add libraries outside this list without a clear reason. Breadth here is the enemy of reliability.

---

## 3. Task → component map (pick by the question, not the data)

| The user wants to… | Use | Quality bar |
|---|---|---|
| See exact values | **sortable/filterable table** | sticky header, right-aligned nums, filter box, CSV export, totals row |
| Compare categories | bar chart | horizontal if labels long / >7 cats |
| See a trend over time | line chart | never bars for a slope you want to read |
| See correlation | scatter | add trend line; size/colour a 3rd dim |
| See part-to-whole | stacked bar / treemap | not pie past ~3 slices |
| Compare options/vendors | **comparison matrix** | freeze first column, ✓/✗ icons, flag the leader, "recommended" badge + one-line why |
| Tune a parameter | **live sliders/inputs** that re-render | direct manipulation > narrating a counterfactual |
| Make a decision | **weighted scorecard** w/ editable weights | turns an opaque pick into an inspectable, contestable judgment |
| Trace a sequence/history | timeline | relative timestamps, grouped by day |
| See relationships/structure | tree / graph (Mermaid or ECharts) | cap node count; collapse deep trees |
| Read a long synthesis | sectioned doc w/ nav | headings, anchor nav, pull-quotes, callouts — break the wall |
| Monitor status | dashboard cards | status pills, sparklines, "last updated" |

---

## 4. The gate — render, text, or hybrid

**Render UI** when: data has structure to act on (sort/compare/filter), it's interactive/explorable, it supports a decision, or a long text answer would be a wall the user has to mentally reconstruct.

**Just answer in text** when: it's a single fact, a short explanation, a yes/no + reasoning, a quick code snippet, or anything you'd say in ≤2 sentences with nothing to sort/compare/tune/trace. Latency (~30–90s to build a page) is the tax — don't pay it for a one-liner.

**Hybrid** when: the artifact is the deliverable but there's a key takeaway or a decision the user must act on — render the artifact, link it, and put the 1–3 line takeaway in chat. Default to artifact-first; keep the chat short.

When you choose text over UI, just say so briefly and answer. Don't over-explain the choice.

---

## 5. Required page structure

Every `/glance-view` artifact has:

- **Header** — left: the **workspace name** (auto-detected, see SKILL) as an eyebrow + the artifact title. right: timestamp. This is how the user knows what they're looking at at a glance.
- **Body** — bespoke to the task, built from §3 components, skinned per §1.
- **Footer** — `workspace · "Glance" · ISO timestamp`, muted.
- **Responsive** — works desktop + mobile (stack grids, collapse nav). Test mentally at ~400px.
- **Light by default**; use the teal-deep band/dark sections for emphasis, not the whole page.
- **Empty/error states** — if a section has no real data, show a quiet "No data" rather than omit silently or fake it.

---

## 6. Built-in affordances (add when relevant)

- **Tables:** clickable headers to sort; a filter `<input>`; a "Copy CSV" button (serialise rows to CSV → `navigator.clipboard.writeText`).
- **Print/Save:** a small "Print / Save PDF" button (`window.print()`), with a `@media print` block that hides controls.
- **Pin to keep:** a one-line note in the footer — `Pinned artifacts: move this folder out of .tmp to keep it.` (`.tmp` is fair game to wipe.)
- Keep all controls keyboard-reachable; rely on the token contrast (don't put green text on white).

### Sortable table pattern (vanilla, reliable)
```js
document.querySelectorAll('table.g-table th').forEach((th,i)=>{
  let asc=true;
  th.addEventListener('click',()=>{
    const tb=th.closest('table').tBodies[0];
    const rows=[...tb.rows];
    const num=th.classList.contains('num');
    rows.sort((a,b)=>{let x=a.cells[i].textContent.trim(),y=b.cells[i].textContent.trim();
      return num?(asc?x-y:y-x):(asc?x.localeCompare(y):y.localeCompare(x));});
    asc=!asc; rows.forEach(r=>tb.appendChild(r));
  });
});
```

---

## 7. Output & file rules

- Find or create `.tmp/` in the workspace root. All Glance output lives under **`.tmp/glance/`**.
- One artifact per folder: **`.tmp/glance/{NN}_{slug}/index.html`** (`NN` = zero-padded next number, `slug` = kebab title). Drop extra assets in that folder if ever needed.
- For distinct streams of work you may add a sub-namespace: `.tmp/glance/{project}/{NN}_{slug}/` — but 99% of the time the flat form is right.
- Maintain **`.tmp/glance/index.html`** — a gallery that lists artifacts (title, prompt, timestamp, link) newest-first, skinned per §1, so the user can toggle between the day's outputs. Update it whenever you create an artifact.
- **Always print the clickable path in chat** as a markdown link so the user can open it (e.g. `[Open →](.tmp/glance/03_q2-revenue/index.html)`). Writing the file surfaces it in the IDE preview automatically; the link is the backup + the signal.
- After rendering: **one-line chat reply + the link.** No text-wall duplication of what's in the artifact (unless hybrid per §4).

---

## 8. Quality checklist (before you finish)

- [ ] Passed the gate — UI is genuinely better than text here
- [ ] Every value/row/label is real (from context/files), nothing fabricated
- [ ] Default skin applied via §1 starter; accent used only for CTAs and key actions
- [ ] Right component for the task (§3), not just a chart-by-default
- [ ] Header shows workspace name + title; footer present
- [ ] Responsive; empty states handled; interactive bits actually work
- [ ] Single self-contained file; only curated libs (§2)
- [ ] Written to `.tmp/glance/{NN}_{slug}/`; gallery index updated; link printed in chat
