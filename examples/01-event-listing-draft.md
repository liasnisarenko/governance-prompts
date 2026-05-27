# Example 1: Browser-driven event listing draft

**Pattern:** Long-form data entry across many fields, with the agent stopping before any irreversible step (login, payments, publish).

**Tools:** Claude in Chrome (or any browser-automation agent) on an event-platform listing builder.

**Why this prompt is interesting:** A live event listing has 50+ fields across 6 screens, plus 9 ticket tiers with pricing and visibility rules. The agent can save hours of form-filling - but a single accidental Publish click puts the wrong event in front of paying customers. This prompt does all the data entry as a draft and explicitly hands the publish step back to the human.

---

## The prompt

```
I want you to help me create an event listing in Chrome. I'll handle anything
that touches login, payments, or final publish. You do all the form-filling.

## Event details - paste these exact values into each field

Event Title (max 75 chars):
[Event Title Here]

Subtitle / Presented by (max 75 chars):
[Subtitle Here]

Summary (max 140 chars):
[Summary copy]

Type of event: Single Event
Page language: English (US)

## Date and time

Start: [Date] at [Time]
End:   [Date] at [Time]
Time zone: [Timezone]
Display end time: ON

## Location

Type: Venue
Venue name: [Venue]
Address:    [Address]
(Use the address autocomplete - pick the matching venue entry if it appears.)

## Description

Paste this block into the Description editor. Use the editor's H2 style for
every line that starts with `## `.

[Long-form description copy here]

## Tickets - create these tiers in this order

For each, set Type = Paid, Visibility = Visible (unless noted hidden), Sales
channel = Everywhere.

1. [Tier name] - $[price] - qty [n] - sales: [start] -> [end]
   description: "[copy]"

2. [Tier name] - $[price] - qty [n] - sales: [start] -> [end]
   description: "[copy]"

[... repeat for each tier ...]

## Refund policy field

Select "Custom" and paste:
"[Refund policy copy]"

## Privacy / settings

- Event visibility: Public
- Listed: Yes
- Searchable: Yes

## What I handle myself, do NOT do these

- Logging in
- Uploading the hero image
- Connecting payouts / bank info
- Accepting the platform's terms of service
- Hitting "Publish" - when you finish all the form fields, save as DRAFT and
  tell me to review before I publish

If you hit a field I haven't given you a value for, ask me. Do not guess.
```

---

## What makes this prompt work

### Explicit "do not do" list at the end

Login, image upload, payouts, ToS acceptance, and publish are all named individually. The agent never has to interpret "be careful" - it has a list to check against.

### Save-as-draft, not publish

The final action is always "save as draft and tell me to review." The publish click stays with the human, who is the one who's actually accountable for what goes live.

### "Ask me, do not guess"

The single most important line. The agent will fill 90% of fields confidently and then encounter one ambiguous case (a missing value, an unclear option, a field the prompt didn't anticipate). The default behavior for browser agents is to guess. This sentence flips it to "pause and ask."

### Exact values, exact order

Tickets are listed in the exact order they need to be created, with all parameters specified per row. No interpretation. No "use your best judgment." The agent's job is data entry, not decision-making.

---

## When to use this pattern

- Long-form data entry on a customer-facing platform (events, listings, product pages, ads).
- Anywhere the cost of an accidental publish is higher than the cost of one extra human click at the end.
- Anywhere the agent might hit a field the prompt didn't anticipate and would otherwise improvise.

## What to adapt

- The "do not do" list is platform-specific. Eventbrite has Publish, Connect payouts, Accept ToS. Shopify has Connect domain, Activate payments, Charge customer. Customize for the platform.
- The "ask me, do not guess" sentence is universal. Keep it in every browser-automation prompt you write.
