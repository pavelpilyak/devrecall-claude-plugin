# DevRecall — Claude Code plugin

Give Claude Code memory of what you've shipped — commits, PRs, Jira tickets,
Confluence docs, Slack threads, and decisions you've captured, all
searchable and citeable from inside your coding session.

This plugin wraps the [DevRecall](https://devrecall.dev) MCP server and
adds four slash commands so the most common workflows are one keystroke
away.

## Prerequisites

Install the DevRecall CLI and desktop app:

```bash
brew install --cask pavelpilyak/devrecall/devrecall
```

The cask installs the GUI at `/Applications/DevRecall.app` and symlinks the
`devrecall` CLI into your Homebrew prefix. The plugin's MCP server entry
points at the symlinked binary, so make sure `which devrecall` resolves
before continuing.

You'll also want to authenticate at least one source (`devrecall auth jira`,
`devrecall auth github`, …) and run an initial sync. See the
[DevRecall docs](https://docs.devrecall.dev/) for source-by-source setup.

## Install

```bash
# Add this repo as a Claude Code marketplace…
/plugin marketplace add pavelpilyak/devrecall-claude-plugin

# …then install the plugin from it.
/plugin install devrecall@devrecall
```

Verify it loaded:

```bash
/mcp          # devrecall server should appear, listing 15 tools
/plugin       # devrecall plugin should appear under installed
```

If `/mcp` shows the server as failing, run `devrecall mcp` manually in a
terminal — it should sit waiting on stdin without errors.

## What you get

### Slash commands

| Command                          | What it does                                          |
|----------------------------------|-------------------------------------------------------|
| `/devrecall:recall <query>`      | Semantic + keyword search, returns cited matches      |
| `/devrecall:context [days]`      | Brief of recent activity for session context          |
| `/devrecall:log <text>`          | Capture a manual note without leaving Claude Code     |
| `/devrecall:prep <id-or-date>`   | Meeting brief: event + each attendee's recent work    |

### MCP tools

Claude Code can call these directly during any conversation (not just from a
slash command):

`current_time`, `list_activities`, `count_activities`, `search_activities`,
`semantic_search_activities`, `get_activity`, `get_related_activities`,
`who_worked_on`, `recent_decisions`, `prep_meeting`, `log_event`,
`list_summaries`, `get_summary`, `list_identities`, `resolve_person`.

### MCP resources

URI-addressed views the assistant can fetch as raw context:

- `standup://YYYY-MM-DD` — all activity from a day
- `activity://<id>` — full row by ID
- `timeline://<period>` — period grammar matches the `devrecall timeline`
  CLI (`YYYY-MM-DD`, `YYYY-MM`, `YYYY`, `last-week`, `last-month`,
  `last-quarter`)

### MCP prompts

Three discoverable prompts surfaced through `/mcp` (in addition to the
plugin slash commands above):

- `devrecall-recall <query>`
- `devrecall-context [days]`
- `devrecall-log <text>`

## Examples

```text
/devrecall:recall what auth bug did I fix in February
/devrecall:context 14
/devrecall:log decided to switch from ULIDs to KSUIDs after benchmarks
/devrecall:prep 2026-05-26 1:1 with Anna
```

## Opt-in: auto-inject context at session start

A session-start hook can run `devrecall standup` for the last 7 days and
inject it as context every time you open Claude Code. It's off by default
because it adds a few hundred tokens to every cold start.

Enable it:

```bash
mv hooks/hooks.example.json hooks/hooks.json
```

Then restart Claude Code. Disable by renaming back.

## Local development

Want to test against an unreleased `devrecall` binary?

1. Build the binary you want to test: `cd ~/Pets/devrecall && make build`.
2. Edit `plugin.json` and change the MCP `command` to the absolute path:
   `/Users/you/Pets/devrecall/bin/devrecall`.
3. Reload the plugin (`/plugin` → reload, or restart Claude Code).

Or skip the plugin entirely and add the dev binary as a project-scoped MCP
server via a `.mcp.json` in your project root — see
[the DevRecall MCP integration docs](https://docs.devrecall.dev/integrations/mcp/)
for the snippet shape.

## License

MIT.
