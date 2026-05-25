---
description: Brief me on what I've been working on recently so you start with real context.
---

Pull a brief of my recent activity from DevRecall. If `$ARGUMENTS` is a number, treat it as the number of days back; otherwise default to 7.

Steps:

1. `mcp__devrecall__current_time` — anchor "now".
2. `mcp__devrecall__list_activities` with `start` set to `now - N days` and `limit=50`.
3. If anything looks decision-shaped or you want a wider view, call `mcp__devrecall__recent_decisions` for the same window.

Then summarize in 4–6 bullets, grouped by source. Highlight:

- What I shipped (PRs merged, tickets closed, docs authored).
- Any decisions captured (notes, ADRs, RFCs).
- One line of "open threads" — recent activity without a follow-up.

Keep it tight. This is context for the rest of the session, not a full standup.
