# Install Glance

Not technical? You don't need to be. Copy everything inside the box below and paste it to your AI coding agent (Claude Code, Codex, Cursor, or similar). It will fetch this repo and set Glance up for you.

```
Please set up the "Glance" skills on my computer. I am not technical, so do every step yourself and explain what you did in plain language at the end.

1. Get the files from this public repo: https://github.com/jsval2/glance
   - If you have git, clone it into a temporary folder.
   - Otherwise, download the ZIP from that page and unzip it into a temporary folder.

2. Find my Claude Code skills folder, and create it if it does not exist:
   - macOS or Linux: ~/.claude/skills/
   - Windows: %USERPROFILE%\.claude\skills\

3. Copy the two folders named "glance-view" and "glance-board" from the repo into that skills folder. If either one already exists, stop and ask me before replacing it.

4. Check that both folders are now inside the skills folder.

5. Delete the temporary clone or download so nothing is left lying around.

6. Then tell me, simply:
   - that Glance is installed,
   - that I should restart my agent so it loads the new skills,
   - how to use it: type /glance-view to turn an answer into a visual page, or /glance-board to watch several agents work together in one dashboard,
   - and that I can change the colors and fonts any time by editing the file glance-view/reference/design.md.

Do not change anything inside the skill folders. Install them exactly as they are.
```

Once it finishes, restart your agent and type `/glance-view`.
