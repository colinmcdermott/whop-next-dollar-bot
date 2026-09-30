# Next dollar: a Grok Bot that runs Whop's Economic Intelligence

Whop's [Economic Intelligence](https://whop.com/blog/economic-intelligence) looks at
a business and returns the single best next action: a title with the expected
benefit, the evidence behind it, a step-by-step brief, the API operations it will
take, and any decisions left to the owner. Run it and Whop checks the ledger
afterwards to see whether the action made money, so the next recommendation is
better than the last.

This Grok Bot template is the operator's preferred agent for that loop. It uses
the [Whop CLI](https://docs.whop.com/developer/cli) on the Grok Bot computer:

```
whop economic-intelligence list      → what to do next, with evidence
whop economic-intelligence update    → running / executed / incomplete / superseded, plus a rating
whop <anything else>                 → the actual work: ads, products, pages, promo codes, messages
```

**Install the template:** https://x.ai/bot/REPLACE_WITH_TEMPLATE_ID

## What it does

- **Shows you what's next.** Pulls the ready recommendations for your business and presents each one: the action, the expected benefit, the evidence, the decisions you need to make (with Whop's three suggested answers), and the exact operations it will run.
- **Runs the one you approve.** Marks it running, answers the inputs with your choices, carries out the brief with the Whop CLI, marks it executed with the id of what it created, and asks you to rate it.
- **Closes the loop honestly.** A run that didn't complete is reported as incomplete, never as executed. A recommendation you reject is superseded with your reason, which shapes the replacement.
- **Reports weekly.** What ran, what it produced, and what sales, customers and transactions did in the seven days after.

Nothing runs without your approval. Anything that spends money asks every time.

## Seven-minute setup

1. Turn on Economic Intelligence in your Whop dashboard. No Whop yet? https://whop.com/start/?a=colin
2. Add the template to Grok Bot and say **run whop-setup**.
3. The bot installs the Whop CLI on its computer and opens a login link. Sign in on the Agent Computer screen. Never paste a key in chat.
4. Say **what's next**. Approve one. Rate it when it's done.

## Files

- `grok-bot/CREATE-PROMPT.md` paste this into a new Bot to build it, then share it as a template
- `grok-bot/BOT.md` the profile and standing rules on their own
- `grok-bot/skills/` the three skills: whop-setup, ei-next, ei-report

No code. The Whop CLI is the code.

## Notes

- Economic Intelligence is rolling out; an account without it gets "You don't have access to Economic Intelligence yet" from the CLI. Enable it in the dashboard first.
- The CLI's login is shared by every Bot on the Grok Bot account, because they share one computer. Use a Whop account you're happy for all of them to act on.
- Works with the [Jev router](https://github.com/colinmcdermott/grok-jev-router) template: if it's installed, this bot asks Jev whether an action is risky before asking you, and skips the question for trivial ones.

MIT.
