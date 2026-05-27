# Governance-Grade AI Execution Prompts

A library of production prompts I use to run browser-automation agents and AI workflows in live environments. Each one is built with explicit safety rails: read-only modes, stop-and-confirm gates, no-auto-publish patterns, and clean human-handoff points.

The point of this repo is to demonstrate **how to write prompts you would actually trust in a real business** - where a single auto-publish, accidental delete, or unintended DM can cost money, reputation, or a client relationship.

---

## Why governance-grade

Most public prompt libraries optimize for impressive output on a clean demo. Real businesses don't run on demos. They run on:

- Live customer-facing accounts where a wrong click is irreversible.
- Multi-stakeholder approval flows where the agent should never be the one who hits publish.
- Parallel sessions and shared assets where "do not touch X" matters.
- Compliance contexts (payment, identity, content moderation) where the human has to be the one who clicks accept.

These prompts are built around four patterns that show up across all of them.

### 1. Explicit "do not do" lists

Every prompt enumerates the actions the agent must NOT take, by name. Not "be careful" - the exact buttons, fields, and flows that are off-limits. Login, payouts, terms of service, publish, delete, allow.

### 2. Stop-and-confirm gates before destructive actions

Before any action that creates, modifies, or destroys state, the agent stops and asks. Save as draft, write the change to a staging area, or pause for a human to do the irreversible step.

### 3. Read-only audit modes

For inspection workflows (CRM audits, social-media audits, account health checks), the prompt explicitly forbids any click that modifies state. "Do not click Allow, Accept, Connect, Add, or any button that creates, deletes, modifies, or shares anything."

### 4. Human-handoff points

When the agent reaches a step it can't handle (a file picker that browser automation can't drive, a 2FA challenge, a payment confirmation), it stops, names the step, and asks the human to take over. Then it resumes from a known state.

---

## What's in this repo

| File | What it shows |
|------|---------------|
| [`examples/01-event-listing-draft.md`](examples/01-event-listing-draft.md) | Browser-driven Eventbrite listing build. Drafts a complete event with 9 ticket tiers, hands off login / payouts / publish to the human. |
| [`examples/02-readonly-account-audit.md`](examples/02-readonly-account-audit.md) | Read-only social-media account audit. Inspects Pages, ad accounts, pixels, monetization, and produces a consolidated report. Forbids any state-changing click. |
| [`examples/03-multi-tool-pipeline.md`](examples/03-multi-tool-pipeline.md) | Multi-tool product pipeline (image generation → print-on-demand → marketplace). Explicit pause point for steps the agent can't drive, parallel-session safety. |

---

## How to read these

Each example follows the same structure:

1. **Context** - who I am, what business this is for, what state we're starting from.
2. **The task** - the actual work the agent should do.
3. **Safety rails** - the do-not-do list, broken out so it's impossible to miss.
4. **Handoff points** - where the agent stops and asks for human action.
5. **Deliverable format** - what the agent should return when it's done.

The patterns transfer. Strip the specific tools and the structure is reusable for any browser-automation agent task in a live environment.

---

## License

MIT. Use these as templates for your own prompts. If you ship something good with them, I'd love to see it.

---

*Maintained by [Lia Snisarenko](https://github.com/liasnisarenko). AI Workflow Operator based in Los Angeles.*
