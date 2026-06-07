---
name: behavioral-analytics
description: Behavioral analytics strategy for People Intelligence — event taxonomy, four instrumentation categories, tool choice, and the weekly feedback loop
metadata:
  type: project
---

# Behavioral Analytics — People Intelligence

Fold in from day one. Consistent event structure from the first screen — three years of incompatible data is the alternative.

---

## What analytics means for an internal tool

With Claudia, Kelsey, and a handful of interviewers, you don't need statistical significance. You need to know:

- Claudia abandoned the Response Queue 3 times this week without completing triage — worth a conversation
- The "Schedule Interview" CTA was clicked zero times — people are scheduling in the calendar directly, bypassing the system
- The Engineering DNA filter was used 47 times this week — most valuable filter built
- "Make Hiring Decision" has a 40% completion rate — the scorecard gate might be blocking Kelsey too aggressively

This is closer to usability research than statistical analytics. Small team, fast feedback loop.

---

## Four categories of events

### 1. Feature adoption — is the feature being used at all?

- Response Queue opened / action taken / abandoned
- Filter saved / used / deleted
- Dossier chat opened / question asked
- Mobile notification tapped / action taken / ignored
- Check-in queue opened / acted on / ignored

### 2. Workflow completion rates — did they finish the task?

- Response triaged to completion vs. opened and abandoned
- Scorecard submitted vs. opened and abandoned
- Hiring decision made vs. Dossier opened without action
- Quick add lead completed vs. started and abandoned
- Offer prepared vs. hiring decision made without following through

### 3. Friction signals — where is it confusing?

- Dead clicks (clicking non-interactive elements)
- Rage clicks (same element clicked multiple times)
- Search / filter queries returning zero results
- Same page visited multiple times in one session (lost, backtracking)
- Undo actions
- Abandonment rate per form (which field causes people to quit)

### 4. AI feedback signals — is the intelligence working?

- Response classification override rate (AI suggested X, Claudia chose Y)
- Draft reply: accepted / edited / deleted
- Engineering DNA tag suggestion: accepted / rejected
- Dossier chat: thumbs up / thumbs down
- Candidate ranking: Kelsey's accept/reject rate on top-ranked suggestions

---

## The event structure — design once, use everywhere

Every event follows the same shape:

```
event_name    string    "response_triaged", "dossier_opened", "filter_saved"
actor_id      uuid      Person UUID of who did it
actor_role    string    "recruiter", "recruiting_lead", "interviewer"
object_type   string    "response", "dossier", "candidate", "filter"
object_id     uuid      UUID of what they acted on
properties    json      context specific to the event
session_id    uuid      groups actions in one session
timestamp     datetime
```

The `properties` field carries the context that makes events useful — not just "scorecard_submitted" but:

```json
{
  "fields_completed": 4,
  "fields_total": 5,
  "time_since_interview_minutes": 187,
  "submitted_from": "mobile"
}
```

---

## Tool choice

| Option | Tradeoff |
|---|---|
| **PostHog self-hosted** | Open source, session replay included, data stays internal. Best fit. |
| **Custom event log** | Log events to a DB table, query with SQL. Full control, no vendor. Works until you need funnels and session replay. |
| **Mixpanel / Amplitude** | Gold standard but overkill for a 5-person user base. Sends data to third party. |

**Recommendation: PostHog self-hosted.** Session replay — actually watching Claudia's session — tells you more than any funnel dashboard. Data stays internal, which matters since you're recording how employees work with candidate data.

---

## Build sequencing

**Phase 1 — instrument the core workflow:**
- Every CTA click with context (who, what object, what state)
- Workflow start and completion per task type
- AI override actions (what was suggested, what was chosen)
- Session start / end per role

**Phase 2 — add friction signals:**
- Zero-result searches and filter queries
- Dead clicks and rage clicks
- Page revisits within one session
- Notification tap rates and time-to-tap

**Phase 3 — AI quality signals:**
- Thumbs up / down on Dossier chat answers
- Engineering DNA suggestion accept / reject rates over time
- Response classification accuracy trend (does it improve as Claudia's overrides feed back?)

---

## The feedback loop that matters

Weekly habit: look at PostHog session recordings. See where Claudia got stuck. Fix it the following sprint. With a team this small, that loop beats any A/B test.

**What to watch weekly:**
- Workflow abandonment rates (which tasks don't get completed)
- AI override rate trends (is the classifier getting better or worse)
- Zero-result filter queries (what filter dimensions are missing)
- Notification tap rates (which alerts are being ignored)

**What triggers a conversation with users:**
- Any workflow with <60% completion rate
- AI override rate >40% on any classification type
- Same friction signal appearing 3+ times in one week

---

## Privacy note

Session replay records how employees work with candidate personal data. Be transparent with the team that sessions are recorded for product improvement. Mask sensitive fields (offer amounts, personal contact info) in the replay tool so they don't appear in recordings.

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Response Queue workflow: [[screen-architecture]]
- AI classification: anti-patterns.md §3B; John's V0 classifier
- Engineering DNA tagging: identity-architecture-decisions.md §Q3
- Related: [[system-gets-smarter]], [[chat-with-data]], [[mobile-strategy]]
