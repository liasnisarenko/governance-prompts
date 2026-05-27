# Example 3: Multi-tool pipeline with parallel-session safety

**Pattern:** A workflow that spans three platforms (image generation, print-on-demand, marketplace). The agent drives most of it, pauses at the one step it can't, and respects state held by a parallel session.

**Tools:** Cowork session + Claude in Chrome, running across image-gen, print-on-demand, and marketplace platforms.

**Why this prompt is interesting:** Real production workflows cross tool boundaries. Browser-automation agents can't drive every step (native file pickers, drag-drop zones, OS-level dialogs are common dead ends). The fix isn't to abandon the workflow - it's to design a clean pause-and-resume pattern. This prompt also shows how to protect state held by a parallel session you don't want disturbed.

---

## The prompt

```
I'm continuing the [Project] pipeline. Tonight's run:

Stack:
- [Image tool]: generate variants from a source.jpg - driven via Chrome MCP
- [Print-on-demand]: create new product, upload output, apply mockup template
  - driven via Chrome MCP
- [Marketplace]: publish listing on [Shop Name] - driven via Chrome MCP

Status:
- [Project]-001 - 3 listings LIVE on [Marketplace]. 15 Pinterest pins live
  across 5 boards.
- [Project]-002 - publishing in progress (run currently mid-flight in a
  parallel session). DO NOT modify [Project]-002 records or state.
- Next: [Project]-003 (end-to-end), then [Project]-004 if time, working
  toward all 5 [Project] designs.

Known blocker:
Chrome MCP cannot drive [Image tool]'s native file picker. When the upload
step is reached, pause and ask me to drag source.jpg onto the upload zone
myself. I'll do it in ~5 seconds and you continue.

Context:
- /path/to/source-images/ (source images, brand assets)
- /path/to/MASTER_TRACKER.md (the [Project] section has the full state)

Workflow:
1) Open [Image tool] in Chrome. Locate the [Project]-003 source.jpg in the
   source folder (ask me if you can't find it).
2) Pause for my drag-drop handoff.
3) Generate variants per the [Project] style (warm/earthy abstract landscape)
   - keep visual consistency with [Project]-001 and [Project]-002.
4) Save variant images to /path/to/[Project]-003/.
5) Move output to [Print-on-demand], create product, apply the standard
   mockup template, set price tier consistent with [Project]-001.
6) Publish to [Marketplace] on [Shop Name] shop. Set renewal to Automatic.
7) Stop and report. Move to [Project]-004 if I say go.

Success criteria:
- [Project]-003 live on [Marketplace], [Shop Name], renewal = Automatic.
- Variant images saved to /path/to/[Project]-003/.
- [Print-on-demand] product linked correctly to the [Marketplace] listing.

Notes:
- Do not pin [Project]-002 or [Project]-003 to Pinterest yet - algorithm
  needs 24-72h to index the prior batch first.
- DO NOT touch [Project]-002 records - that run is still in flight in a
  parallel session.
```

---

## What makes this prompt work

### State snapshot up front

Before the workflow, the prompt declares what's already done, what's in flight, and what's next. The agent doesn't have to discover this from scratch. The "DO NOT modify [Project]-002 records" instruction is explicit and capitalized.

### Named known blocker, named handoff

The native file-picker dead-end is called out at the top, not discovered mid-run. The agent knows the workflow has a planned pause and the human knows when to be ready.

### Numbered workflow + success criteria + notes

Three different things, three different sections:

- The workflow is the steps.
- The success criteria is how you know the run succeeded.
- The notes are out-of-band constraints that don't fit the linear flow (algorithm cooldowns, parallel-session safety).

This separation matters. Agents that conflate "what to do" with "what to avoid" tend to drop the avoidance instructions when they get focused on execution.

### Cross-session safety

"DO NOT touch [Project]-002 records - that run is still in flight in a parallel session" is the kind of instruction that's easy to forget and expensive to omit. If you're running multiple sessions on shared state, name the shared state and protect it explicitly.

---

## When to use this pattern

- Any workflow that spans 2+ platforms.
- Workflows that include a step browser automation cannot drive (file pickers, captchas, 2FA, payment confirmation, OS dialogs).
- Workflows running in parallel with other sessions touching the same accounts.
- Recurring batch workflows where each run is similar but not identical (different source files, different IDs).

## What to adapt

- Replace `[Project]`, `[Image tool]`, `[Print-on-demand]`, `[Marketplace]`, `[Shop Name]` with your actual tools.
- The "Known blocker" section is per-tool-combination. Each platform has its own dead-ends. Inventory them once per stack, reuse the list across runs.
- The "Notes" section is where you put platform-specific quirks (algorithm cooldowns, rate limits, scheduling rules).
