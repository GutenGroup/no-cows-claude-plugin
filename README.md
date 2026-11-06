# no-cows for Claude

no-cows' Claude plugin. It installs two things:

- the **no-cows connector** (`https://no-cows.com/mcp`), and
- the **no-cows skill**, which teaches Claude the board's jobs: what is next, reading and moving cards, goals and the Story, Jev's lessons, filing items, and working cards end to end.

Each person signs in to no-cows once, the first time Claude uses the connector, and picks the workspace.

## Install for your whole company (Claude Team or Enterprise)

1. Build the zip: `python3 scripts/generate.py --source https://no-cows.com/mcp-kit --zip dist/no-cows-claude-plugin.zip`.
2. In Claude, open **Organization settings > Plugins & skills > Add > Upload a plugin** and choose the zip.
3. Set **Default access** to **Installed by default**.

## Install for yourself (Claude Code)

```bash
claude plugin marketplace add https://git.no-cows.com/guten-group/no-cows-claude-plugin.git
```

```bash
claude plugin install no-cows@no-cows
```

Or, for Claude Code, Cursor, Codex and Gemini CLI at once: `curl -fsSL https://no-cows.com/mcp-kit/install.py | python3 -`.

## How it stays current

`plugins/no-cows/` is generated, never hand-edited. `scripts/generate.py` builds it from the no-cows MCP kit (the tool catalog the server generates from its own tool table, and the companion skill). CI (`check-plugin`) fails the moment the live server's tools and this plugin disagree, and it also runs daily.
