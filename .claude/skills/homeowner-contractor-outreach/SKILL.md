---
name: homeowner-contractor-outreach
description: >
  End-to-end homeowner workflow for finding licensed contractors, requesting
  quotes, comparing bids, and booking appointments — all hands-free. Use this
  skill whenever a homeowner describes a home repair, maintenance problem, or
  improvement project and needs help finding and hiring a contractor. Triggers
  on phrases like "find me a plumber", "I need someone to fix my water heater",
  "get me quotes for", "who should I call for", "my [appliance/system] is
  broken", or any description of a home repair problem followed by intent to
  hire. Also triggers when a user wants to compare contractor quotes, follow up
  on estimate requests, or book a service appointment. Do NOT use for pure DIY
  questions where the user has stated they want to do the work themselves.
---

# Homeowner Contractor Outreach Skill

This skill coordinates a multi-agent workflow to take a homeowner from problem
description to booked appointment. It spawns specialized sub-agents for each
phase, keeps the homeowner in control at every decision point, and uses only
tools the homeowner has already authorized (Gmail, Google Calendar).

---

## Workflow Overview

```
[1. Intake]  →  [2. Research]  →  [3. Outreach]  →  [4. Intake & Compare]  →  [5. Book]
User describes    Find 3–5          Draft + send      Monitor replies,          Calendar
the problem       licensed          quote request     summarize quotes,         invite +
+ location        contractors       emails via        flag best options         confirmation
                  in the area       Gmail                                       email
```

The orchestrator (this skill) runs phases 1 and 5 directly. Phases 2–4 are
handled by sub-agents defined in `references/agents.md`.

---

## Phase 1 — Intake (Orchestrator)

Collect the following before spawning any sub-agents. If the user's initial
message already contains these, skip asking:

| Field | Why it matters |
|---|---|
| `problem_description` | What's broken or needed, in the user's words |
| `trade_type` | Plumber, HVAC, electrician, roofer, handyman, etc. |
| `urgency` | Emergency (today), soon (this week), flexible |
| `zip_code` | Required for contractor search radius |
| `preferred_contact_window` | When the user is available for appointments |
| `budget_range` | Optional — helps filter out mismatched contractors |

**Ask only what's missing.** Infer `trade_type` from `problem_description`
when possible (e.g. "water heater" → plumber/HVAC).

Once intake is complete, confirm the summary with the user before proceeding:

> "I'll search for licensed [trade_type] contractors near [zip], draft quote
> request emails to the top 3–5, and report back once replies come in. Sound
> good?"

---

## Phase 2 — Contractor Research (Sub-agent)

Spawn the **research sub-agent** defined in `references/agents.md#research`.

Pass in: `trade_type`, `zip_code`, `urgency`

Expect back: a list of contractors with these fields:
```json
{
  "name": "string",
  "company": "string",
  "email": "string or null",
  "phone": "string",
  "license_verified": true | false | "unknown",
  "rating": "float or null",
  "review_count": "int or null",
  "source": "Google | Yelp | Angi | BBB | other",
  "notes": "string"
}
```

**Quality filters** (instruct the research agent to apply these):
- Prefer contractors with 4.0+ rating and 10+ reviews
- Flag any contractor without a verifiable license number
- For emergencies, filter to contractors with "24/7" or "emergency" in listing
- Return 3–5 results; 5 if email addresses are available for at least 3

If no emails are found for any contractor, note this and suggest the user
call directly — provide the phone numbers and a call script. See
`references/call-script-template.md`.

---

## Phase 3 — Email Outreach (Sub-agent)

Spawn the **outreach sub-agent** defined in `references/agents.md#outreach`.

Pass in: contractor list from Phase 2, `problem_description`, `zip_code`,
`preferred_contact_window`, `budget_range` (if provided).

The outreach agent will:
1. Draft a quote request email for each contractor with a valid email address
2. Personalize lightly (use the contractor's name/company)
3. Include the problem description, location, urgency, and availability window
4. Add a clear subject line: `Quote Request — [Trade Type] at [City, State]`
5. **Show the user the first draft before sending any emails**
6. On user approval, send all emails via Gmail MCP
7. Label all sent emails `contractor-quotes` in Gmail for easy retrieval

**Never send emails without explicit user approval.** Present a preview and
wait for "send" or "looks good" before proceeding.

Email template is in `references/email-template.md`. Follow it but make
the tone conversational, not corporate.

---

## Phase 4 — Reply Monitoring & Comparison (Sub-agent)

This phase is **asynchronous**. Two modes:

### Mode A — User returns with replies
User says "I got some responses" or "contractors replied" → spawn the
**intake sub-agent** defined in `references/agents.md#intake`.

Pass in: raw reply text or ask the user to forward/paste the replies.

The intake agent produces a comparison table:
```
| Contractor | Quote | Availability | Notes | Recommended? |
|------------|-------|--------------|-------|--------------|
```

Flag the recommended option based on: price reasonableness, earliest
availability matching user's window, rating, and license status.

### Mode B — Proactive check (if Gmail MCP available)
If the Gmail MCP is connected, the user can ask "any replies yet?" and the
intake agent searches the `contractor-quotes` label for unread threads.

---

## Phase 5 — Booking (Orchestrator)

Once the user selects a contractor:

1. Draft a confirmation reply email to the chosen contractor (via Gmail)
2. Create a Google Calendar event with:
   - Title: `[Trade] Appointment — [Company Name]`
   - Description: problem summary + contractor contact info
   - Reminder: 24 hours before + 1 hour before
3. Optionally send a polite decline to contractors not chosen (ask user first)

Confirm the calendar event was created and show the user the event details.

---

## Decision Points (Human-in-the-Loop)

This skill **always pauses for user approval** at these moments:

- [ ] After intake summary — before starting research
- [ ] After showing contractor list — before drafting emails
- [ ] After showing email draft — before sending
- [ ] After showing comparison table — before booking
- [ ] Before sending decline emails to unchosen contractors

Never skip these checkpoints even if the user says "just do it" — confirm
with a one-line summary of what's about to happen.

---

## Edge Cases

**No emails found for any contractor**
→ Pivot to call-assist mode. Provide phone numbers, trade-specific call
script from `references/call-script-template.md`, and offer to draft a
follow-up email if they get a contractor name.

**Emergency / same-day urgency**
→ Skip email entirely. Provide top 3 phone numbers with the call script.
Offer to book calendar slot once the user confirms a contractor by phone.

**User is in a rural area with < 3 contractor results**
→ Expand search radius to 25 miles, then 50 miles. Notify user of the
expanded radius.

**Contractor replies with a phone number instead of a quote**
→ Note this in the comparison table. Offer to add a call reminder to
the user's calendar.

**User wants to negotiate**
→ Draft a counter-offer email. See `references/negotiation-template.md`.

---

## Sub-agent Reference

See `references/agents.md` for full prompts for each sub-agent:
- `#research` — Contractor discovery agent
- `#outreach` — Email drafting + sending agent
- `#intake` — Reply parsing + comparison agent

See `references/` for templates:
- `email-template.md` — Quote request email template
- `call-script-template.md` — Phone call script by trade type
- `negotiation-template.md` — Counter-offer email template
