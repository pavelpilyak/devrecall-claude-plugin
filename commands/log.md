---
description: Capture a manual note into DevRecall — a decision, observation, or conversation summary — without leaving the editor.
---

Log this into DevRecall as a manual note:

> $ARGUMENTS

Call `mcp__devrecall__log_event` with `text` set to the message above. After it returns, confirm with the new `activity_id` and the one-line title that was extracted.

If the message hints at named people or tags (e.g. "decided with @anna, tag: arch"), extract them and pass through the `people` and `tags` arguments respectively.
