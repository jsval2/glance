---
name: glance-board
description: A persistent local dashboard that parallel agents write structured JSON into — toggle specs, context files, and live outputs from multiple agents in one stable viewer. Use for multi-agent or parallel coding/research runs to see what every agent is doing without the mental overload of scattered output. Sets up a standing viewer at .tmp/glance-board/ and defines the JSON contract agents write to. Invoke with /glance-board.
---

# Glance-Board

A standing dashboard for when several agents are running at once. The viewer is built **once** and never regenerated; agents just write structured JSON into a state folder, and the board renders + lets you toggle between them. This is the *visibility* layer — seeing what's happening — not an orchestrator (it doesn't spawn, route, or stop agents).

Shares the active design system with `/glance-view` (`~/.claude/skills/glance-view/reference/design.md` + `components.md`).

## Setup (on invocation)

1. Ensure the board exists in the workspace:
   - `.tmp/glance-board/viewer.html` — if missing, copy it from `~/.claude/skills/glance-board/viewer.html`.
   - `.tmp/glance-board/state/` — create the dir.
   - `.tmp/glance-board/state/manifest.json` — create/refresh (see contract).
2. **Serve it over HTTP** (the viewer `fetch()`es JSON, so `file://` is blocked by CORS). Serve `.tmp/glance-board/` with an **absolute** `--directory`:
   ```bash
   python3 -m http.server 7341 --directory "/absolute/path/to/.tmp/glance-board"
   ```
   Your editor's built-in preview server works too. **Known footgun:** some preview runners use a stripped PATH where a bare `python3` fails to bind. If `curl localhost:7341` refuses, use an absolute interpreter path (find it with `which python3`) or run the command above in the background.
3. Print the link in chat: `http://localhost:7341/viewer.html`.

## The JSON contract

**`state/manifest.json`** — the index the viewer polls:
```json
{
  "workspace": "my-project",
  "title": "Run: auth refactor",
  "updated": "2026-06-05T22:00:00Z",
  "panels": [
    { "id": "orchestrator", "file": "orchestrator.json" },
    { "id": "agent-auth",   "file": "agent-auth.json" }
  ]
}
```

**`state/{id}.json`** — one file per agent / spec / output / context (each writer owns its own file, so parallel agents never collide):
```json
{
  "id": "agent-auth",
  "type": "agent",
  "title": "Auth refactor agent",
  "status": "running",
  "updated": "2026-06-05T22:01:00Z",
  "summary": "Rewriting session middleware",
  "tags": ["backend","auth"],
  "blocks": [
    { "type":"markdown", "md":"## Plan\n- swap cookie store\n- thread session through middleware" },
    { "type":"code", "lang":"ts", "title":"middleware.ts", "code":"export function withSession(){/*…*/}" },
    { "type":"diff", "title":"auth.ts", "diff":"@@\n-const x = old()\n+const x = next()" },
    { "type":"table", "columns":["File","Status"], "rows":[["auth.ts","done"],["session.ts","wip"]] },
    { "type":"kv", "items":{"Branch":"feat/auth","Tests":"12/12 green"} },
    { "type":"callout", "tone":"warn", "md":"Blocked on secret rotation" },
    { "type":"list", "items":["read existing middleware","mapped call sites"] },
    { "type":"link", "href":".tmp/glance/05_auth-map/index.html", "label":"See the render" }
  ]
}
```

- **`type`**: `agent` | `spec` | `output` | `context` | `status` (drives sidebar grouping + icon).
- **`status`**: `idle` | `running` | `done` | `blocked` | `error` (drives the status dot).
- **`blocks[].type`**: `markdown` | `code` | `diff` | `table` | `kv` | `callout` (tone: `info`/`good`/`warn`/`bad`) | `list` | `link`.
- Always set `updated` to an ISO timestamp on every write so the board shows freshness.

## How agents use it

- Each parallel agent (or spec/output stream) **writes and updates its own `state/{id}.json`** — append/replace blocks as it works. No shared-file contention.
- The **orchestrator** (you, the main agent) maintains `manifest.json`: add a panel entry when you spin up a stream, so the viewer picks it up.
- The viewer polls `manifest.json` + each panel every ~2s and re-renders. Nothing else regenerates the UI.

## Rules

- Same as Glance: **real data only**, no fabricated status/diffs/output. The board is for trust — fake state defeats it.
- The viewer is the stable shell — **never regenerate it per update**; only write JSON.
- It's temporary: `.tmp/glance-board/` is wipeable. Pin a run by moving the folder out.
