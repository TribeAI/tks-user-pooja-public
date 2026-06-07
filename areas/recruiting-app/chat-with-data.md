---
name: chat-with-data
description: Chat-with-data capability for People Intelligence — three modes, how each resolves, and why the identity layer makes this uniquely powerful internally
metadata:
  type: project
---

# Chat With Your Data — People Intelligence

Distinct from the filter/group bar. The filter bar serves users who know what they want and can express it in structured terms. Chat serves the question you can't pre-build a filter for, and sophisticated users want this as a first-class capability.

Kim already does this manually with a Claude skill that pulls together Ashby + Sybill + RA data on demand. This makes it native.

---

## Three modes

### 1. Pipeline-level chat

Ask questions across the whole dataset. Resolves into a database query, returns structured results. Replaces building a one-off filter from scratch.

Example questions:
- "How many candidates are currently in interview stage across all roles?"
- "Who are our strongest frontend engineers who declined offers in the last year?"
- "What's our average time-to-hire for referrals vs. cold outreach?"
- "Which roles have the most stale candidates right now?"

**Where it lives:** Global search / chat bar, accessible from any screen.

---

### 2. Dossier-level chat

Ask questions about a specific person. Resolves into document retrieval + summarization across the activity feed, scorecards, enrichment data, and placement history.

Example questions:
- "What did interviewers say about her system design skills?"
- "Has anyone at Tribe worked with this person before?"
- "Summarize everything we know about this candidate before I get on a call"
- "What roles has she been considered for and what was the outcome?"

**Where it lives:** Chat input on the Dossier detail view. This is Kim's manual Claude skill — built natively into the Dossier.

**Why this is different from the activity feed:** The feed is chronological raw data. Dossier chat synthesizes across it — answers a specific question rather than making the user read everything.

---

### 3. Recommendation queries

The highest-leverage capability. Resolves into matching and ranking across Person + Engineering DNA + Placement history + OpenRole requirements.

Example questions:
- "Who in our database would be a strong fit for this new opening?"
- "Which of our declined candidates should we re-engage given this role just opened?"
- "Who have we placed at similar accounts that we could introduce to this client?"
- "Who in our lead database has Python + ML skills and has never been outreached?"

**Where it lives:** OpenRole detail view ("Find candidates for this role"), and People Database global bar.

---

## Why internal beats external here

External ATS AI is bounded by external ATS data:
- Ashby AI sees Ashby candidates only
- Gem AI sees Gem pipeline only
- Neither has placement history, Engineering DNA, or cross-context signal

People Intelligence AI sees everything the identity service holds: sourcing history, outreach history, interview signal, Engineering DNA (validated), placement history, account context — all in one query. The identity layer is what makes the AI answers trustworthy.

---

## Filter bar vs. chat — when to use each

| Filter bar | Chat |
|---|---|
| Repeatable views (save and reuse) | One-off questions |
| Structured dimensions you know | Questions you can't pre-express as filters |
| Power users building dashboards | Anyone with a question |
| "Show me all referrals in interview stage" | "Who should I re-engage for this new role?" |

They're complementary. Sophisticated users use both. The filter bar is for what you know you want to see regularly. Chat is for what you're trying to figure out.

---

## Build sequencing

- **Phase 1:** Dossier-level chat (summarize this candidate) — highest daily value, lowest query complexity
- **Phase 2:** Pipeline-level chat (aggregate questions across dataset)
- **Phase 3:** Recommendation queries (matching + ranking across Person + OpenRole)

Phase 1 directly replaces Kim's manual Claude skill. Start there.

---

## Sources

- Design session: Representation phase, Session 005 (2026-06-06)
- Kim's manual Claude skill: anti-patterns.md §3A (broken CTA — Placement Recommendation)
- Identity layer foundation: [[identity-service-pattern]]
- Internal tool advantage: [[internal-tool-advantage]]
