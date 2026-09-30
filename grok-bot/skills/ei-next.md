# Skill: ei-next

When to use: when the owner asks what to do next, asks to run a recommendation, or the daily check finds something ready.

Steps:
1. Call `economic-intelligence_list` with `status: ready`. If the owner stated a goal, add it as `input` in their words. If the list is empty, say generation is queued and stop; do not invent actions.
2. For each ready recommendation, show: the title (it contains the action and the expected benefit), the reasoning as Whop wrote it, each item in `inputs` with its label and its three options (strongest first), and the `expected_tool_calls` as a numbered list of their plain-English descriptions. Keep Whop's order.
3. Ask the owner which one to run, and for each input whether they take the first option or want a different answer. Wait.
4. If `/workspace/jev/router.py` exists, write a one-page state (task = the title, proposed_action = the tool calls, bot = Next dollar) and run `python3 /workspace/jev/router.py --state /tmp/state.json`. Show its action and reason. Continue only if the owner still approves.
5. Mark it started: `economic-intelligence_update` with the recommendation id and `status: running`.
6. Carry out the `prompt` step by step with the Whop connector, using the owner's answers to the inputs. The tool names in the brief are the connector's tool names. Two differ: `generateImage` is `media_generate` with `type: image` and `wait: true`, which returns a file id; `loadSkill` means inspect the tools for that group before using them. Inspect each tool's schema before its first call. Before any call that spends money, stop and ask again with the exact amount. If a step needs something only the owner can do (a payment method, a domain, a login), ask them to do it and continue after. If a tool in the brief does not exist in the connector (for example `dm-channels_create` or `messages_create`; direct messages are Whop AI only), do not improvise another channel: tell the owner that step needs Whop AI or the dashboard, offer everything else, and if the recommendation cannot be completed without it, mark it `incomplete` with that reason.
7. On success: `economic-intelligence_update` with `status: executed` and `result_id` set to the id of the thing created or changed (a plan, product, ad, campaign, app, checkout link or promo code). Use `result_page` if the result is a list rather than one resource. On failure or a stopped run: `status: incomplete`, and say what stopped it.
8. Ask the owner to rate it. Send their answer with `sentiment: positive` or `negative`, and `user_feedback` if they said why.
9. If the owner rejected the recommendation at step 3: `economic-intelligence_update` with `status: superseded`, `user_feedback` in their words, and `input` describing what they want instead. Then list again for the replacement.

Validate: the recommendation's status in `economic-intelligence_list` matches what happened.

Return: one line per recommendation: title, what you did, the `result_url` from the updated recommendation, and the rating sent.

Requires approval: running any recommendation; every money-spending step; anything the owner's Auto-review rules name.
