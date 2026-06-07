---
type: area-note
area: architecture
created: 2026-06-06
tags: [identity-service, golden-record, architecture, data-model]
---

# The Golden Record Pattern — Identity Service Architecture

Architectural pattern for building a unified identity layer when a person (or any entity) exists across multiple systems with no shared key. Developed during the People Intelligence lighthouse work (2026-06-06).

---

## The Problem

When a person exists in N systems (recruiting tool, ATS, HR system, staffing tool), each system has its own ID for the same human. There is no shared key. The same person is four different records in four different databases.

The cost:
- Operators can't see a unified view
- Intelligence gathered in one system disappears when the person moves to another
- Basic questions ("how many engineers do we have?") can't be answered from one place
- Automated logic (matching, recommendations) operates on incomplete data

---

## The Pattern

### Two layers

```
Product (UI layer)
  ↕ reads/writes via API
Identity Service (infrastructure layer)
  ↕ syncs events from
Source Systems (Candor, Ashby, TribeHub, Rippling, etc.)
```

**The identity service has no UI.** It is an API + event ingestion layer + overnight batch process. Nobody logs into it. It is invisible to operators.

**The product is what operators see.** All screens read from and write to the identity service. The identity service is the foundation; the product is built on top of it.

**Source systems are data contributors, not authorities.** Each source system owns a slice of attributes on the unified record. No source system owns the whole record.

---

## The Canonical Key

**Co-primary match signals + internal UUID anchor.**

- **LinkedIn URL** — primary match signal for anyone who came through the recruiting pipeline. Stable, unique, human-verifiable. Already the spine of sourcing tools.
- **Email** — fallback match signal for internal employees who were never in the recruiting pipeline. Rippling has work email.
- **UUID** — internal anchor assigned at Person record creation. Decouples the identity service from any external system. If LinkedIn URL scheme changes or email changes, UUID doesn't move.

---

## Matching Rules

Run overnight as a batch process. Two tiers:

| Condition | Action |
|---|---|
| LinkedIn URL match (normalized) | Auto-merge — high confidence |
| Email exact match, no LinkedIn URL conflict | Auto-merge — medium confidence |
| Name + partial signal match | Flag for human review queue |
| Name only | Never auto-merge |

**Human-in-the-loop:**
- Reviews the overnight batch (spot-check, not approve every merge)
- Works the flagged review queue
- Provides feedback that improves future matching rules

**Conflict resolution:** When co-primary signals conflict (LinkedIn URL X + email Y vs. LinkedIn URL X + email Z), source priority determines which field wins. Define priority order per field type at design time.

---

## Field Ownership

Each source system writes to a specific slice. No system owns the whole record. Define ownership per attribute:

| Attribute type | Owner |
|---|---|
| Employment data (location, FTE%, legal name) | HR system (e.g., Rippling) |
| Recruiting data (interview stage, feedback) | ATS (e.g., Ashby) |
| Sourcing data (LinkedIn background, embedding) | Sourcing tool (e.g., Candor) |
| Intelligence / notes | Product (human-authored in the UI) |
| Collaborative tags (skills, specialties) | Validated collaborative — anyone suggests, designated roles confirm |
| Lifecycle state | Identity service (event-driven, see below) |
| System ID mappings | Identity service |

---

## Lifecycle State

**The identity service owns lifecycle state. Source systems contribute events.**

No single downstream system owns the lifecycle field. Instead:
- Source system fires an event ("candidate hired", "placement ended")
- Identity service receives the event and updates lifecycle state
- Decoupled from any one system's API limitations

**State model:**
- Lifecycle state is a **list**, not a scalar — a person can be in multiple states simultaneously (e.g., Placed on one account + Interviewing for FTE conversion)
- Each state is **context-scoped** (attached to an Account or OpenRole)
- Only one state per context — can't be Interviewing and Offered for the same OpenRole simultaneously
- Sequence is **non-linear** — enforce nothing, just record where someone is

---

## Collaborative Tags (e.g., Engineering DNA)

When a tag attribute needs to feed automated logic (matching, recommendations), use a validation layer:

- Anyone can suggest a tag at any lifecycle stage
- Tags from non-authoritative roles are marked "unconfirmed"
- Designated roles (e.g., recruiting lead, staffing lead) validate tags
- Only **validated tags** feed automated logic
- **Unconfirmed tags** are visible in the UI but flagged

Base taxonomy: use an existing standard (LinkedIn Skills, O*NET, ESCO) as the foundation. Allow custom tags for domain-specific concepts not in the standard taxonomy.

---

## When to Use This Pattern

Apply when:
- The same real-world entity exists in N systems with no shared key
- Operators need a unified view across systems
- Automated logic (matching, recommendations, scoring) needs clean, canonical data
- The entity has a lifecycle that spans multiple systems

Do not apply when:
- All data lives in one system already
- The entity doesn't travel across system boundaries
- Real-time sync is required (this pattern is batch-first)

---

## Related

- People Intelligence identity decisions: `tks-team-100x/projects/2026-06-05-people-intelligence-ooux/sessions/003-output/identity-architecture-decisions.md`
- People Intelligence object model: `tks-team-100x/projects/2026-06-05-people-intelligence-ooux/sessions/002-output/object-model.md`
