# Skill: ei-next

When to use: when the owner asks what to do next, asks to run a recommendation, or the daily check finds something ready.

Steps:
1. `whop economic-intelligence list --status ready --format json`. If the owner stated a goal, add `--input "<their words>"`. If empty, say generation is queued and stop; do not invent actions.
2. For each ready recommendation, show: the title (this contains the action and the expected benefit), the reasoning as Whop wrote it, each item in `inputs` with its label and its three options (strongest first), and the `expected_tool_calls` as a numbered list of plain-English descriptions. Keep Whop's order.
3. Ask the owner which one to run, and for each input whether they take the first option or want a different answer. Wait.
4. If `/workspace/jev/router.py` exists, write a one-page state (task = the title, proposed_action = the tool calls, bot = Next dollar) and run `python3 /workspace/jev/router.py --state /tmp/state.json`. Show its action and reason. Continue only if the owner still approves.
5. Mark it started: `whop economic-intelligence update <reca_id> --status running --format json`.
6. Carry out the `prompt` step by step with the Whop CLI, using the owner's answers to the inputs. Map each expected tool call to a CLI command by reading `whop <group> <command> --help` first. Before any command that spends money, stop and ask again with the exact amount. If a step needs something only the owner can do (a browser login, a payment method, a domain), ask them to take over on the Agent Computer and continue after.
7. On success: `whop economic-intelligence update <reca_id> --status executed --result_id <id of the thing created or changed> --format json`. Use `--result_page` if the result is a list rather than one resource. On failure or a stopped run: `--status incomplete`, and say what stopped it.
8. Ask the owner to rate it. Send their answer: `--sentiment positive` or `--sentiment negative` with `--user_feedback` if they said why.
9. If the owner rejected the recommendation at step 3: `whop economic-intelligence update <reca_id> --status superseded --user_feedback "<their reason>" --input "<what they want instead>" --format json`. Then list again for the replacement.

Validate: the recommendation's status in `whop economic-intelligence list` matches what happened.

Return: one line per recommendation: title, what you did, the result URL from the updated recommendation, and the rating sent.

Requires approval: running any recommendation; every money-spending step; anything the owner's Auto-review rules name.
