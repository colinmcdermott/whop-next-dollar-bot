# Skill: whop-setup

When to use: once, right after this Bot is added, or when `whop auth status` fails.

Required: a Whop account with a business, and Economic Intelligence turned on in its dashboard (Home shows "recommended actions"). No Whop yet: https://whop.com/start/?a=colin

Steps:
1. Install the CLI on the Agent Computer: `curl -fsSL https://whop.com/install.sh | sh`. Then `whop --version`. If curl is blocked, `npm install -g @whop/cli`.
2. Log in without ever handling a secret: run `whop login --method oauth --format jsonl > /tmp/whop-login.jsonl 2>&1 &` in the background, then read the first line of /tmp/whop-login.jsonl and open its `authorizationUrl` in the Agent Computer browser exactly as written, never retyped or shortened. Tell the owner: "Open Agent Computer, take control, sign in to Whop in the browser window, then hand control back." Wait for the file to show a `done` event.
3. Pick the business: `whop auth account --list`, then `whop auth account <biz_id>` for the one the owner names. If there is only one, select it and say which.
4. Confirm: `whop auth status --format json` shows the account. Then `whop economic-intelligence list --first 5 --format json`.
   - If it returns recommendations, show their titles and statuses.
   - If it returns an empty list, listing has queued generation. Say so and check again in ten minutes.
   - If it says access is not enabled, tell the owner to turn on Economic Intelligence in the Whop dashboard and stop.
5. Recommend these Auto-review rules (Settings → General → Bot → Auto-review): Ask first before any command containing `deposits`, `transfers`, `payouts`, `cards`, or `refund`. Ask first before any `ad-campaigns` or `ad-groups` create or update that sets a budget. Ask first before `apps deploy`.

Validate: `whop auth status` shows the right business and `whop economic-intelligence list` returns without an access error.

Return: CLI version, the selected business name and id, how many recommendations are ready, and the Auto-review rules still to add. Never the login URL or any token.

Requires approval: nothing here changes the business.
