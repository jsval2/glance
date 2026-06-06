# Glance

Let your AI assistant answer with a small web page instead of a wall of text.

Glance is a pair of [Claude Code](https://docs.claude.com/claude-code) skills. When an answer has structure to it (data, comparisons, lots of moving parts), Glance builds a clean, self-contained HTML view and opens it right next to the chat, instead of printing paragraphs you have to reread.

## The two skills

**`/glance-view`** is the everyday one. Ask for something and it builds a fresh page for that answer: a sortable table for data, a chart for a trend, options laid side by side, sliders you can move. It picks the format from the question, and it only builds a page when a page is actually better. Simple questions still get a one-line reply. Output lands in `.tmp/glance/` and is linked in the chat.

**`/glance-board`** is for when several agents are running at once. It sets up one standing dashboard that each agent writes into, so you can flip between their specs, status, and output in a single place instead of scrolling through scattered messages. Agents write small JSON files; the board reads them and refreshes on its own.

## Install

**Not technical?** Open [INSTALL.md](INSTALL.md), copy the prompt, and paste it into your AI agent (Claude Code, Codex, Cursor). It fetches this repo and sets everything up for you.

To do it by hand, clone the repo and copy both skill folders into your Claude Code skills directory:

```bash
git clone https://github.com/jsval2/glance
cd glance
mkdir -p ~/.claude/skills
cp -r glance-view glance-board ~/.claude/skills/
```

Then type `/glance-view` or `/glance-board` in Claude Code. Works best on a frontier model, where generated HTML is reliable.

## What's inside

```
glance/
├── glance-view/
│   ├── SKILL.md            how /glance-view behaves
│   └── reference/
│       ├── design.md       the look: colors, type, spacing (swap to re-skin)
│       └── components.md   the build playbook: when to render, which component fits, output rules
└── glance-board/
    ├── SKILL.md            how /glance-board behaves, plus the JSON agents write
    └── viewer.html         the dashboard itself (built once, reads the JSON)
```

`design.md` and `components.md` are shared. `/glance-board` reads the same look from `/glance-view`'s reference folder.

## Customizing the look

Everything is skinned from one place. To change the theme:

1. Edit the tokens in `glance-view/reference/design.md` (colors, fonts, radius, spacing).
2. Mirror those values in the `:root` block at the top of `components.md`. That block is the starter every page is built from.
3. For the dashboard, update the matching `:root` block at the top of `glance-board/viewer.html`.

Keep the token names the same and the whole system picks up the new look. You can drop in any design system you like; the more complete `design.md` is, the more consistent the output.

## How it decides to build a page

Glance does not turn every answer into a page. The rule is simple: if it is a fact or something you would say in a sentence or two, it just answers. It builds a view when the output is worth sorting, comparing, exploring, or deciding on. It never invents data to fill space; if the real data is not there, the element is left out.

## License

MIT. See [LICENSE](LICENSE).
