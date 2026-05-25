---
description: Brief me for a calendar meeting — event details + each attendee's recent activity.
---

Prep me for an upcoming meeting. The user's input is in `$ARGUMENTS` — it may be:

- A calendar activity ID (numeric).
- A date (YYYY-MM-DD) with optional title fragment, e.g. `2026-05-26 1:1 Anna`.
- A title fragment alone — in that case, assume today.

Steps:

1. If the input is numeric, call `mcp__devrecall__prep_meeting` with `activity_id`.
2. Otherwise, parse out the date (default to today via `mcp__devrecall__current_time`) and any title fragment. Call `mcp__devrecall__prep_meeting` with `date` and `title_contains`.
3. The tool returns the meeting details plus, for each attendee, their recent activity (last 30 days by default).

Then summarize:

- Meeting title, time, and link (if any).
- For each attendee, 2–4 bullets of their most recent shipped work — group by source where it reads cleanly.
- A short "things to ask / decide" prompt list inferred from the activity (open PRs, in-progress tickets, recent decisions).
