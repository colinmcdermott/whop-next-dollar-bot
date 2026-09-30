# Skill: ei-report

When to use: weekly, or when the owner asks how the recommendations are doing.

Steps:
1. `whop economic-intelligence list --has_run --order run_started_at --direction desc --first 20 --format json`.
2. For each executed recommendation in the last 30 days, pull the seven days before and the seven days after `executed_at` with `whop stats` (read `whop stats --help` first for the metric names): revenue, paying customers, transaction count.
3. Build a table: title, executed date, result URL, revenue before, revenue after, change. One row per recommendation. Mark rows where the after window is not yet complete.
4. List incomplete runs and why they stopped, and superseded recommendations with the feedback given.
5. Mark newly executed results as seen: `whop economic-intelligence update <id> --status acknowledged --format json`.
6. If there are ready recommendations waiting, list their titles at the end.

Return: the table, then three lines: what worked, what didn't, what is waiting for a decision. If nothing has run, say so.

Requires approval: none; this reads and marks as seen.

