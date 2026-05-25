---
description: Search my DevRecall index (commits, PRs, Jira, Confluence, decisions) for past activity matching a query.
---

Search my DevRecall index for: **$ARGUMENTS**

Use these MCP tools in order:

1. `mcp__devrecall__current_time` — anchor relative dates first.
2. `mcp__devrecall__semantic_search_activities` with `query="$ARGUMENTS"` — try fuzzy matches first.
3. `mcp__devrecall__search_activities` with `query="$ARGUMENTS"` — fall back to keyword search for exact terms.
4. For the top 3–5 hits, call `mcp__devrecall__get_activity` to read the full body.
5. For any activity that carries a ticket key, call `mcp__devrecall__get_related_activities` to surface the full PR/commit/Jira chain.

Then summarize the findings as a short list. For each result include: source (git/jira/github/confluence/slack), date, title, and one sentence of context — quoting the body where useful. Don't list every match; focus on the most relevant.
