# Skill: ei-report

When to use: weekly, or when the owner asks how the recommendations are doing.

Steps:
1. Call `economic-intelligence_list` with `has_run: true`, `order: run_started_at`, `direction: desc`, `first: 20`.
2. For each executed recommendation in the last 30 days, call `stats_get` for the seven days before and the seven days after `executed_at`, with `interval: week`, for these metrics: `net_revenue`, `successful_payments`, `paid_active_members`, `users_growth`. If a metric call is rejected, say the numbers are unavailable for that metric rather than estimating.
3. Build a table: title, executed date, result URL, revenue before, revenue after, change. One row per recommendation. Mark rows where the after window is not yet complete.
4. List incomplete runs and why they stopped, and superseded recommendations with the feedback given.
5. Mark newly executed results as seen: `economic-intelligence_update` with `status: acknowledged`.
6. If there are ready recommendations waiting, list their titles at the end.

Return: the table, then three lines: what worked, what didn't, what is waiting for a decision. If nothing has run, say so.

Requires approval: none; this reads and marks as seen.

