---
name: mobile-strategy
description: Mobile strategy for People Intelligence — notification-driven quick actions, five mobile screens, what mobile explicitly does not do
metadata:
  type: project
---

# Mobile Strategy — People Intelligence

Mobile is not a smaller version of desktop. It's a different surface with a different job.

**Desktop job:** do deep work on the pipeline — build views, write feedback, make decisions with full context.

**Mobile job:** handle what needs immediate attention without being at your desk.

---

## The mobile entry point is the notification, not the app

On desktop you open the app and browse. On mobile the notification IS the navigation.

> *"Jane Smith's scorecard is outstanding — interview was 2 hours ago."*
> Tap → straight to the scorecard form. Submit. Done.

No home screen. No navigation. Tap the notification, take the action, close the app.

---

## When the team actually needs mobile

- Kelsey is between meetings — gets alert that a candidate has been stale 24 hours
- Interviewer just finished a call — wants to submit scorecard while it's fresh
- Claudia is at an industry event — meets someone, wants to add them as a referral on the spot
- Candidate responds to outreach — Claudia wants to triage before she's back at her desk
- Offer response comes in — Kelsey wants to know immediately

All of these: **notification arrives → one quick action → done.** Not browsing, not deep editing, not building filter views.

---

## Five mobile screens

### 1. Action queue — the mobile home

Not a pipeline view. What needs your attention right now?

- Stale candidates → tap to ping Claudia or move stage
- Outstanding scorecards → tap to remind the interviewer
- Offer responses received → tap to mark accepted/declined
- Response triage items → tap to classify and reply

Each item has one primary action inline. No drilling required.

### 2. Quick triage — Claudia's response queue on the go

Response text visible. Three buttons: **Interested / Not Interested / Uncertain**. One tap — status updates, draft reply sends. Swipe interaction is an option here.

### 3. Scorecard form — for interviewers right after a call

Stripped down. Structured fields only. No Dossier context loaded unless explicitly requested. Designed to complete in 3 minutes while the conversation is still fresh.

### 4. Quick add lead — for Claudia at events

Four fields maximum: name, LinkedIn URL or email, which role, referred by. Save. Everything else fills in overnight from enrichment. As fast as adding a contact to your phone.

### 5. Pipeline at a glance — read-only status check

How many candidates at each stage, per role. No editing. The 10-second check Kelsey wants between meetings.

---

## What mobile explicitly does not do

- Filter/query builder → desktop only
- Chat with data → desktop only
- Dossier editing → desktop only
- Complex decisions requiring full context → desktop only

Feature parity is the wrong goal. Mobile parity with desktop means a bad mobile experience and forces desktop design decisions that serve neither surface.

---

## Notification strategy by role

| Role | Trigger | Cadence |
|---|---|---|
| Kelsey | Offer response received | Real-time |
| Kelsey | Stale candidate (>24hrs in stage) | Daily digest, configurable |
| Kelsey | Scorecard outstanding >X hours | Real-time |
| Claudia | Outreach response received | Real-time |
| Claudia | Scheduling confirmation | Real-time |
| Claudia | Re-engage date reached | Daily digest |
| Interviewers | Submit your scorecard | X hours after interview ends |
| All | @mentions in candidate notes | Real-time |

**Internal tool advantage:** let users configure notification cadence per type. Kelsey may want offer responses real-time but stale alerts as a morning batch. External products rarely let you tune this.

---

## Design principles

1. **Notification-first navigation.** The notification is the entry point. Design every notification as a complete unit — enough context to act without opening the full app.

2. **One primary action per item.** No menus on mobile. One tap does the most likely thing. Secondary actions are one more tap deeper, not surfaced upfront.

3. **Structured input only.** Mobile forms have structured fields — dropdowns, toggles, scales. No long-form text entry on mobile. Notes and detailed feedback are desktop work.

4. **Optimistic updates.** Actions confirm immediately on screen. No spinners, no waiting. Backend reconciles in the background.

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Claudia's triage cost: anti-patterns.md §3B; Sybill May 18 call
- Kelsey's pipeline police need: anti-patterns.md §3C; Sybill June 2 call
- Scorecard gate: event-storm.md §9
- Related: [[screen-architecture]], [[data-freshness]]
