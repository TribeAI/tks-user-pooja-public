---
name: system-gets-smarter
description: How People Intelligence meets the promise of getting smarter over time — three learning timelines, feedback loop design, cold start solution, and the UX of visible learning
metadata:
  type: project
---

# The System Gets Smarter — People Intelligence

The hardest promise to keep. Requires designing for it from day one — can't be bolted on later. Three distinct timelines, each requiring different design decisions.

---

## Three learning timelines

### 1. Pattern recognition — available at launch

The identity service resolves years of Tribe's historical data: every lead in Candor, every candidate in Ashby, every placement in SCC. That history is the first training dataset. Day one capabilities:

- "Candidates with Python + ML DNA in this domain have a 78% offer acceptance rate at Tribe"
- "Cold outreach to this profile type gets 12% response rate; referrals to this profile get 40%"
- "This account type tends to extend placements — contractors here have a 60% renewal rate"

Not future learning — pattern recognition on existing history. Available at launch if historical data is ingested properly during identity resolution.

### 2. Feedback learning — accumulates over weeks and months

Every user judgment is a labeled training example. Captured from natural actions, not surveys:

| User action | Signal captured |
|---|---|
| Claudia overrides AI response classification | Labeled training example for the classifier |
| Kelsey rejects a candidate with certain DNA tags | What doesn't fit this role/account |
| Kelsey advances a candidate | What does fit |
| Outreach sequence gets high response rate | What messaging works for which profile type |
| Interviewer scorecard patterns | What interview signals predict offer acceptance |

**Core principle: capture signal from actions, not from asking users to rate things.** Nobody fills out feedback forms. Everybody takes actions.

### 3. Outcome learning — accumulates over months and years

The most valuable and the slowest. Closes the loop from recruiting through placement and beyond:

- Did the placement work out?
- Did the client renew?
- Did the contractor stay or churn?
- Did candidates with certain DNA profiles succeed at this account type?

This is what makes People Intelligence genuinely different from an ATS over time. An ATS records what happened. People Intelligence learns what predicts success.

Eventually enables: *"Based on 47 placements at accounts like this, engineers with this profile tend to succeed — here's who in your pipeline matches that."*

---

## The cold start problem — solved by history

The system starts with zero labeled outcomes from its own predictions. But it doesn't start with zero data. It starts with Tribe's full history.

The identity service resolution pass IS the first training pass. Historical hires that worked, placements that renewed, response rates from past outreach sequences — all of that is signal before the new system makes its first recommendation.

**Why this matters for build sequencing:** getting the historical data ingestion right during identity resolution isn't just a data migration — it's bootstrapping the intelligence layer.

---

## The UX of visible learning

The promise fails if users can't see it happening. Trust requires legibility.

### Explain the reasoning

When the system ranks a candidate highly, show why — not a black box score:

> "Strong match — 4 similar profiles placed at this account type in the last 18 months all succeeded. Python + ML validated."

The reasoning makes the recommendation trustworthy. Without it, Kelsey has no basis to agree or override — she's flying blind.

### Surface when the model updates

When a recommendation changes based on new signal, tell Kelsey:

> "Ranking updated based on recent placement outcomes at this account type."

She knows the system incorporated something new. That's what builds trust in the learning promise over time.

---

## Priority ranking for Tribe

| Learning type | Priority | Why |
|---|---|---|
| Response classifier improvement | High | Immediate daily value for Claudia; fast feedback loop from overrides |
| Offer acceptance prediction | High | Helps Kelsey prioritize which candidates to push hard for |
| Engineering DNA inference quality | Medium | Improves as more people are tagged and validated by Kelsey/Kim |
| Placement success prediction | High long-term | Most valuable capability; needs 12+ months of outcome data to be reliable |

---

## What this requires from the data model

For the system to learn, outcomes must be captured as structured data — not buried in notes:

- Offer accepted / declined (with reason) → already in event storm
- Placement renewed / ended (with reason) → placement app scope, but signal must flow back
- Claudia's override actions → captured as events, not just silent corrections
- Scorecard patterns → structured form, not free text notes

Free text doesn't feed models. Structured outcomes do. Every place in the UX where a user makes a judgment, there should be a structured field capturing it — even if it's just a 1-5 score or a reason dropdown.

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Historical data foundation: [[identity-service-pattern]] — identity resolution as first training pass
- Response classifier: anti-patterns.md §3B (broken CTA — response triage); John's V0 classifier
- Engineering DNA validation model: identity-architecture-decisions.md §Q3
- Outcome data: event-storm.md — offer declined reason field, placement flow (Ian's scope)
- Related: [[chat-with-data]], [[internal-tool-advantage]]
