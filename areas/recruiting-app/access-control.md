---
name: access-control
description: Roles, permissions, and access control model for People Intelligence — five roles, permission matrix, three key design decisions, Tribe employee as candidate problem
metadata:
  type: project
---

# Access Control — People Intelligence

Permissions must be in the data model from day one. Retrofitting after launch causes breaches or awkward internal incidents.

---

## Five roles

| Role | Who | In one sentence |
|---|---|---|
| Recruiter | Claudia | Works candidates day-to-day — sources, outreaches, manages pipeline |
| Recruiting Lead | Kelsey | Owns the pipeline — makes hiring decisions, sets offers, sees everything |
| Interviewer | Any Tribe employee on a panel | Scoped to their specific interview — nothing else |
| Leadership | Nick, Pooja | Full read access, analytics, no pipeline editing |
| Admin | Pooja, Andrew | Configure roles, integrations, system settings |

---

## Permission matrix

| Permission | Recruiter | Recruiting Lead | Interviewer | Leadership |
|---|---|---|---|---|
| View pipeline — all candidates | ✅ | ✅ | ❌ | ✅ |
| Add / edit leads | ✅ | ✅ | ❌ | ❌ |
| View full Dossier | ✅ | ✅ | Briefing only | ✅ |
| View offer amount | ❌ | ✅ | ❌ | ✅ |
| Make hiring decision | ❌ | ✅ | ❌ | ❌ |
| Submit scorecard | ❌ | ❌ | ✅ | ❌ |
| View other scorecards | After all submitted | ✅ | Own only, after submitted | ✅ |
| Suggest Engineering DNA tags | ✅ | ✅ | ❌ | ❌ |
| Validate Engineering DNA tags | ❌ | ✅ | ❌ | ❌ |
| View outreach history | ✅ | ✅ | ❌ | ✅ |
| View prior rejection history | ✅ | ✅ | ❌ | ✅ |
| View analytics | Own candidates | All roles | ❌ | All |
| Configure notification thresholds | ❌ | ✅ | ❌ | ✅ |

---

## Three key design decisions

### 1. Offer amounts — Recruiting Lead and above only

Claudia sources and manages the pipeline but doesn't set or see comp. Kelsey owns the offer conversation. Clean separation — Claudia shouldn't accidentally quote a number in conversation with a candidate.

### 2. Feedback blinding for interviewers

Interviewers cannot see any other scorecard until they've submitted their own — not even after submission. They see only their own scorecard, ever.

- Kelsey and Claudia see all scorecards once the gate unlocks (all panel members submitted)
- This is evaluation integrity, not just good practice
- System enforces this — not an honor system

### 3. Interviewer access is scoped, not role-based

An interviewer does not have a standing "Interviewer" permission in the system. They get access to:
- One specific interview
- One specific briefing
- One specific scorecard form

Access is granted when Kelsey schedules them. Access expires after scorecard submission.

This matters especially because Tribe employees who are also contractors or potential FTE candidates could otherwise browse the pipeline and find themselves in it.

---

## The Tribe employee as candidate problem

When a placed contractor is being considered for FTE conversion, they appear in the system as both a Person (employee/contractor) and a candidate. They may also be scheduled as an interviewer for other roles.

**The scoped interviewer access model handles this cleanly:**
- They can only see what they were explicitly granted for a specific interview
- They cannot search the pipeline
- They cannot find their own candidate record
- Access = "Interview X with Candidate Y" — scoped, time-limited, expires on submission

No special-casing needed. The scoping model covers it.

---

## What this requires from the data model

Every sensitive field needs a visibility attribute checked at the API layer:

| Field | Visibility |
|---|---|
| Offer amount | Recruiting Lead, Leadership, Admin |
| Full outreach history | Recruiter, Recruiting Lead, Leadership |
| Prior rejection history | Recruiter, Recruiting Lead, Leadership |
| Other interviewers' scorecards | Recruiting Lead, Leadership (always); Recruiter (after gate unlocks) |
| Engineering DNA — unvalidated tags | Recruiter, Recruiting Lead |
| Engineering DNA — validated tags | All authenticated users |
| Candidate personal contact info | Recruiter, Recruiting Lead |

The API returns only what the caller's role permits. The UI never receives data it shouldn't show — not just hidden, but never sent.

---

## Build sequencing note

Permissions are not a Phase 2 feature. They must be designed into:
1. The identity service API (role-aware data returns)
2. The data model (visibility attributes on sensitive fields)
3. The interviewer invitation flow (scoped access grant + expiry)
4. The scorecard gate (feedback blinding enforcement)

Adding permissions after the fact requires auditing every API endpoint and every UI component. Build it in from the start.

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Scorecard gate: event-storm.md §9 (all panel members must submit before decision unlocks)
- Engineering DNA validation ownership: identity-architecture-decisions.md §Q3
- Offer amount ownership: identity-architecture-decisions.md §Q3 field ownership table
- Related: [[screen-architecture]], [[system-gets-smarter]]
