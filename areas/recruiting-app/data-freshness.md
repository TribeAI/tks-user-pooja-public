---
name: data-freshness
description: Data freshness model for People Intelligence — four data speeds, UX patterns for surfacing staleness, and when stale data actually causes harm
metadata:
  type: project
---

# Data Freshness — People Intelligence

Data in the system moves at four different speeds. The UX problem: users can't tell what's current and what's stale — and stale data at a decision point causes real harm.

---

## The four data speeds

| Speed | What updates | Examples |
|---|---|---|
| Real-time | User-triggered actions | Outreach sent, stage changed, scorecard submitted |
| Near-real-time | External signals | Lemlist open/click tracking, calendar confirmations |
| Overnight batch | Enrichment + identity resolution | LinkedIn sync, Candor import, Engineering DNA suggestions, deduplication, re-engage dates |
| On-demand | AI-generated output | Dossier chat synthesis, placement recommendation |

---

## The UX principle

**Don't show freshness indicators everywhere — show them where staleness causes a bad decision.**

Timestamps on every field create noise and anxiety. The goal is to surface freshness only at the moments it matters.

---

## Five UX patterns

### 1. Separate real-time from enriched data visually

The activity feed (outreach sent, stage changes, scorecards) is always current — updates the moment something happens. The enriched profile (LinkedIn data, Engineering DNA, contact info) is batch-updated overnight. These live in visually distinct sections. Users know at a glance what they're looking at.

### 2. One "last enriched" timestamp per enriched section

Not per field — per section. "Profile last synced: last night, 2:14 AM" on the enriched block. Clean, unobtrusive, honest.

### 3. Staleness warnings at decision points only

Not in the passive profile view. At the moment of action:
- Preparing to send an offer → "Email last verified 45 days ago — confirm before sending"
- Initiating outreach → "LinkedIn profile last synced 3 weeks ago — data may be outdated"

This is where staleness causes harm. Flag it there, not everywhere.

### 4. Proactive surfacing when data is probably wrong

If a candidate's LinkedIn was last synced 30 days ago but they were recently active (signal from enrichment layer), surface: "Profile may have changed — refresh?" A prompt, not an alert.

### 5. Optimistic updates for user actions

When Claudia sends outreach or Kelsey moves a stage, the UI updates immediately. Don't wait for the event to propagate through the identity service. Reconcile in the background. The user's action is never blocked by backend latency.

---

## Where staleness actually causes harm

| Data | Risk if stale | Severity |
|---|---|---|
| Engineering DNA tags | Wrong placement recommendation | High |
| Contact email | Outreach bounces or goes to wrong address | Medium |
| LinkedIn profile | Missing job change, wrong re-engagement timing | Medium |
| Lifecycle state | Should never be stale — real-time event-driven | N/A |
| Activity feed | Should never be stale — real-time | N/A |

---

## Build sequencing

**Phase 1:** Separate real-time vs. enriched sections visually. Single "last enriched" timestamp on the enriched block.

**Phase 2:** Staleness warnings at specific decision points (offer send, outreach initiation).

**Phase 3:** Confidence scores on enriched fields — not just *when* data was updated but *how confident* the identity service is in it. Especially relevant for Engineering DNA tags inferred from sparse data.

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Overnight batch architecture: [[identity-service-pattern]], Q2 matching rules decision
- Engineering DNA freshness risk: identity-architecture-decisions.md §Q3
- Related: [[chat-with-data]] — on-demand synthesis as a fourth speed
