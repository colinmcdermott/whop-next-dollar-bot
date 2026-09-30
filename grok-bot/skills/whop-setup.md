# Skill: whop-setup

When to use: once, right after this Bot is added, or when Whop tools stop responding.

Required: a Whop account with a business, and Economic Intelligence turned on in its dashboard (Home shows "recommended actions"). No Whop yet: https://whop.com/start/?a=colin

Steps:
1. Check for the Whop connector: type @ and look for Whop. If it is not there, tell the owner: "Open Marketplace, add the Whop connector, complete the sign-in, then say continue." Wait.
2. Attach @Whop and call `economic-intelligence_list` with `first: 5`. If the connector exposes `search_tools`, search "economic intelligence" first and inspect `economic-intelligence_list` with `get_tool_details`, then call it with `call_read_tool`.
   - If it returns recommendations, show their titles and statuses.
   - If it returns an empty list, listing has queued generation. Say so and check again in ten minutes.
   - If it says access is not enabled, tell the owner to turn on Economic Intelligence in the Whop dashboard and stop.
   - If the owner manages several businesses, ask which one and pass its `account_id` (prefixed biz_) on every list, create and stats call from then on.
3. Only if there is no Whop connector in the Marketplace: install the CLI on the Agent Computer with `curl -fsSL https://whop.com/install.sh | sh`, then run `whop login --method oauth --format jsonl > /tmp/whop-login.jsonl 2>&1 &`, open the `authorizationUrl` from the first line of that file in the Agent Computer browser exactly as written, and tell the owner to take control, sign in, and hand back. Then `whop auth account <biz_id>` and `whop economic-intelligence list --first 5 --format json`. Every CLI command takes `--format json`.
4. Recommend these Auto-review rules (Settings → General → Bot → Auto-review): Ask first before any Whop tool whose name starts with deposits, transfers, payouts, or cards, or contains refund. Ask first before ad-campaigns or ad-groups create or update. Ask first before apps_deploy.

Validate: `economic-intelligence_list` returns without an access error for the right business.

Return: how the bot is connected (connector or CLI), the selected business name and id, how many recommendations are ready, and the Auto-review rules still to add. Never a login URL or token.

Requires approval: nothing here changes the business.
