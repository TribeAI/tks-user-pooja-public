---
name: internal-tool-advantage
description: Why building People Intelligence as an internal tool unlocks slice-and-dice flexibility that no external ATS can match — and the design pattern that enables it
metadata:
  type: project
---

# Internal Tool Advantage — Slice and Dice

The biggest product design advantage of building People Intelligence internally: you know exactly who your users are, what questions they're trying to answer, and you own the full data model. External products (Ashby, Gem) optimize for the lowest common denominator across thousands of companies. You optimize for Kelsey and Claudia specifically.

---

## Where the advantage shows up

### 1. The People Database — queries no external ATS can answer

External ATSs can't answer these because they don't have the identity layer or Tribe-specific data. People Intelligence can, because the identity service holds it all together:

- "Show me everyone with validated Python + ML Engineering DNA who was offered and declined in the last 12 months"
- "Who in our lead database has been placed at Acme Corp before?"
- "Who has React skills, was outreached 8 months ago, never responded, and is past their re-engage date?"
- "Everyone Claudia sourced in Q1 who made it to interview but didn't get an offer"

These queries span Person, Dossier, Placement, OpenRole simultaneously. Only possible with the identity service as the anchor.

### 2. Views are saved filters, not fixed screens

The named screens (Pipeline view, Alert view, Response Queue, Check-in Queue) are **default saved views on a flexible query layer** — not rigid layouts.

Each user saves views that match how they think:
- Kelsey: "stale candidates across all roles," "all offers pending this week," "referrals in pipeline"
- Claudia: "referrals only, this week," "unresponded outreach >3 days," "candidates I sourced"
- Nick: "candidates at offer stage, all active roles" (weekly check)

No one asks engineering to change a filter. Think Linear's issue list or Airtable — same underlying data, multiple lenses per user.

### 3. Engineering DNA as a first-class filter dimension

Because the taxonomy is structured and validated (not free text), it's filterable and groupable in ways no external tool supports:

- Group the pipeline by Engineering DNA cluster — all frontend engineers together, all ML engineers together — across every open role at once
- Filter the People Database to "validated React + TypeScript, never placed"
- Alert when a candidate with rare skills (e.g., CUDA, embedded systems) enters the pipeline

This view doesn't exist anywhere in the market. It's only possible because of the collaborative tagging + validation model decided in Session 003.

---

## The enabling principle

**Everything that gets structured becomes a filter dimension.**

| Structured → filterable | Unstructured → not filterable |
|---|---|
| Engineering DNA tags (validated) | Free-text interview notes |
| Lifecycle state | Recruiter comments |
| Source (referral / sourced / inbound) | Email body text |
| Re-engage date | Candidate bios |
| Days in current stage | |
| Panel scorecard status | |

This is why the Session 003 decisions mattered beyond the identity service: structured lifecycle states, validated tags, and typed source fields were designing the filter layer before the UI was even discussed.

---

## The design pattern

Every list screen has a **filter/group bar** at the top. Named screens are just default saved views:

```
[Filter: Role = All] [Group by: Stage] [Sort: Days in stage ↓] [+ Save view]
                                                               ↑
                                              "Stale candidates" — Kelsey's saved view
```

The underlying query engine is the same for all screens. The saved view is just a persisted filter state with a name.

---

## What this means for build sequencing

Build the filter layer early. It's not a Phase 2 nice-to-have — it's the core of what makes this better than Ashby. A rigid pipeline view with no filtering gives Kelsey no more power than what she has today.

Priority order for filter dimensions (most valuable first):
1. Lifecycle state + stage
2. Engineering DNA tags (validated)
3. Source
4. Days in current stage / last activity
5. Placement history (cross-context)
6. Re-engage date

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Data model foundation: [[identity-service-pattern]], Session 003 decisions
- Engineering DNA structure: identity-architecture-decisions.md §Q3
- Competitive gap: [[competitive-landscape]] — no external ATS has the cross-context query capability
