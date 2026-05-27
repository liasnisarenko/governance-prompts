# Example 2: Read-only social-media account audit

**Pattern:** Inspect-only browser workflow. Agent navigates through every section of a live account, captures state, produces a consolidated report. Never clicks anything that creates or modifies.

**Tools:** Claude in Chrome (or any browser-automation agent) on a social-media business console.

**Why this prompt is interesting:** Auditing a live business account is high-leverage (you find brand-protection gaps, expired monetization, untrained ad accounts) but high-risk (one wrong click can create a Page, delete a pixel, or accept a partnership). The read-only mode is enforced by an explicit "do not click" list, not by hope.

---

## The prompt

```
You have access to my [Platform] account via the current Chrome tab. I am
[Name], owner of [Brand] (existing) and I'm about to launch two more brands:
[Brand 2] and [Brand 3]. Today I appear to be operating from my personal
profile rather than a structured Business Portfolio, and I want to know
exactly what's set up so I can design the right multi-brand structure.

Please conduct a thorough audit by clicking through the panels listed below
in order. After each section, take a screenshot, read what's on screen, and
write a short summary. At the end, produce a single consolidated report.

## Audit checklist - do all of these in order:

1. Profile / Portfolio mode
   - At the top of the home screen, click "Business portfolio".
   - Report whether a Business Portfolio exists, its name, and what's inside.
   - If no portfolio exists, note that explicitly.
   - Click the profile selector and list every profile I can switch between.

2. Settings -> Business assets
   List:
   - Every Page attached (name, category, follower count)
   - Every Instagram account attached (handle, follower count)
   - Every WhatsApp Business account, if any
   - Every ad account (name, ID, currency, spending limit)
   - Every Pixel / Dataset (name, ID, which website it's installed on)
   - Every Catalog (commerce), if any
   - Every Domain that's been verified

3. Settings -> Users / People
   List every person with access, their role, and which assets they touch.

4. Insights -> Overview, Audience, Content
   For each connected Page and Instagram, capture:
   - Last 28 days: reach, content interactions, follower growth (net)
   - Audience: top cities, age, gender, top countries
   - Best performing post (format, topic, reach, engagement)
   - Posting cadence (posts per week, last post date)

5. Ads -> Ads Manager
   Report:
   - All ad accounts visible and which is the default
   - Lifetime ad spend per account, if shown
   - Any active campaigns (status, objective, daily budget, spend last 7d)
   - Any paused campaigns from the last 6 months and what they promoted

6. Inbox
   How many unread DMs, comments, and review notifications across all
   connected Pages/IG. DO NOT open or reply to any of them - just report
   the counts and the oldest unanswered message age.

7. Monetization
   Click Monetization in the left sidebar. Report what's set up, what's
   expired, and whether there's anything to clean up or restart.

8. All tools
   Walk through "All tools" and list which optional tools are turned on
   vs off. Pay attention to: Automated Responses, Lead Center,
   Appointments, Commerce Manager, Events Manager.

9. Events Manager / Pixel health
   For each Pixel/Dataset, report:
   - Last event received (timestamp)
   - Top events in last 7 days
   - Whether Conversions API is set up
   - Domain verification status for [your domain]

10. Brand-protection check
    Search the platform for the handles [list of handles you want to claim].
    For each, report whether the handle is taken, by whom, or whether it's
    available to claim.

## Deliverable format

After working through the checklist, write a single consolidated report
with these exact sections:

- TL;DR (5 bullets max - the most important findings)
- Current architecture (portfolio yes/no, Pages, IG, ad accounts, pixels,
  domains, users, as a clean list or table)
- Activity and engagement (the numbers from step 4)
- Ads state (from step 5)
- Backlog (unread DMs/comments/reviews from step 6)
- Tools turned on vs off (from step 8)
- Pixel health (from step 9)
- Handle availability for the new brands (from step 10)
- Top 10 issues to fix before launching [Brand 2] and [Brand 3], ranked by
  severity

## Do-not-do list

Do not make any changes. This is a read-only audit. Do not click "Allow",
"Accept", "Connect", "Add", or any button that creates, deletes, modifies,
or shares anything. If a screen requires confirmation to proceed past a
read-only state, stop and note it in the report instead.

When you're done, paste the consolidated report back to me as text so I
can copy it.
```

---

## What makes this prompt work

### "Do not click" is a list, not a vibe

The forbidden actions are named: Allow, Accept, Connect, Add. The agent doesn't have to guess what "be careful" means. It can match button text against the list.

### "Stop and note" instead of "skip"

If the agent hits a confirmation that would push past read-only, the instruction is to stop and document, not to skip silently. The report tells you exactly where the audit hit a wall.

### Numbered checklist with a fixed report format

The agent's deliverable is constrained. You get the same report shape every time, which means you can run the same audit across multiple accounts and compare results.

### Brand-protection check baked in

Most account audits stop at "what do you have." This one adds "what could you have but don't yet" - handle availability for upcoming brands. Cheap to add, high-value finding.

---

## When to use this pattern

- Any time you need to inventory a live account before designing a change.
- Vendor or contractor handoffs - know what they actually have access to.
- Pre-acquisition or pre-launch due diligence.
- Annual cleanups (expired monetization, orphaned pixels, dormant ad accounts).

## What to adapt

- The platform name and the specific panels. The pattern is the same on Meta Business Suite, Google Ads, LinkedIn Campaign Manager, TikTok Ads, Shopify admin, Cloudflare dashboard.
- The "do-not-do list" - each platform has its own dangerous buttons. Inventory them once per platform, reuse the list across audits.
