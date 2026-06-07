---
name: recruiting-app-screen-architecture
description: Screen hierarchy, role-based navigation tracks, and Claudia's response queue design for People Intelligence recruiting app
metadata:
  type: project
---

# People Intelligence — Screen Architecture

Derived from OOUX object model + event storm. Every screen maps to an object or a role-based working view.

---

## The hierarchy

```
Kelsey's track          Claudia's track
─────────────           ───────────────
Alert view              Response Queue      ← each role's primary entry point
  ↓                       ↓
Pipeline view    ←→    Pipeline view        ← converge here
  ↓                       ↓
Dossier          ←→    Dossier              ← converge here
```

**Key principle:** Same app, role-based entry points. Kelsey enters from top (alerts, pipeline health). Claudia enters from the side (responses, then works the individual). They converge at Pipeline and Dossier.

---

## Screen inventory

| Screen | Primary user | What it answers |
|---|---|---|
| Alert view | Kelsey | Who needs my attention right now? |
| Response Queue | Claudia | What came in overnight? What do I do with each? |
| Pipeline view | Both | Where is everyone on this role? |
| Dossier | Claudia (deep) / Kelsey (decision) | What do I do with this specific person? |
| Briefing | Interviewers | What do I need to know before this interview? |
| People Database | Both | Find anyone across all contexts |
| Check-in Queue | Kelsey | Who do I re-engage today? |

---

## Dossier depth

The Dossier is Claudia's primary working view, not Kelsey's. Kelsey dips in for decisions; Claudia lives there when working a candidate.

**Design principle:** The Pipeline card handles 80% of Kelsey's actions. The Dossier handles the 20% that need full context. Dossier should be deep but fast to navigate — not simplified for the occasional visitor.

---

## Claudia's Response Queue — action queue, not review queue

The Response Queue is Claudia's highest-priority screen. She opens it first every morning. Currently this workflow is broken across Lemlist + RA and costs ~1 hour/day.

### What the queue does

Every inbound response from a candidate appears here. Each response surfaces:
- The response text
- AI pre-classification with confidence ("AI thinks: Interested — 82%")
- Candidate context inline (who they are, which role, Engineering DNA, prior contact history)
- A draft reply based on the classification

### Claudia's job in the queue

**Confirm or override** the AI classification — not cold-judge it herself.
**Review the draft reply** — edit if needed, then send. Not write from scratch.
**For Interested:** pick a screening slot from Kelsey's availability inline — schedule without switching to calendar.

### Before → after

| Today | With People Intelligence |
|---|---|
| Read response in Lemlist | Response + AI classification + context inline |
| Judge intent manually | Confirm or override AI suggestion |
| Copy reply template | Review AI draft, edit if needed |
| Send from Lemlist | Send from within People Intelligence |
| Update status in RA | Status updates automatically on send |
| Switch to calendar to schedule | Pick a slot inline if Interested |

Result: ~1 hour/day → ~15 minutes.

### Queue ordering — not chronological

Priority order:
1. Responses from referrals (higher stakes)
2. Responses sitting >24 hours (urgency)
3. Uncertain classifications (need human judgment)
4. Everything else

### Phase 1 vs Phase 2

**Phase 1 (MVP):** Context surfacing inline, manual classification with 3 CTAs (Interested / Not Interested / Uncertain), manual reply, auto status update.

**Phase 2:** AI pre-classification with confidence score, AI draft replies, embedded scheduling for Interested responses.

---

## Alert view — Kelsey's pipeline police

Kelsey's home screen. Cross-role visibility — not filtered to one OpenRole.

Surfaces:
- Candidates stuck in a stage past SLA (>24hrs without movement)
- Scorecards outstanding (who hasn't submitted)
- Offers without response
- Re-engage dates reached (check-in queue)

Kelsey acts from this view — she doesn't need to open Dossiers to manage these.

---

## Sources

- Evidence: Sybill calls May 18, May 19, June 2 (Claudia on triage cost; Kelsey on pipeline police)
- Anti-patterns: Broken CTA 3B (response triage), Broken CTA 3C (pipeline police)
- Event storm: Session 004, recruiting flow
- Competitive reference: [[competitive-landscape]] — Gem response queue pattern; Ashby pipeline card pattern
