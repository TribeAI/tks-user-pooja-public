---
type: area-note
area: product
created: 2026-06-06
tags: [ooux, event-storming, methodology, ux-design]
---

# OOUX → Event Storming → Representation: The Design Sequence

Mental model for how to go from problem space to designed screens without guessing. Resolved during the People Intelligence lighthouse work (2026-06-06).

---

## The Core Question

> "How do you design an experience if you don't know how the data flows?"

You don't design screens until you know both **what exists** (objects) and **what happens** (events). OOUX answers the first question. Event storming answers the second. Representation (sketching) is only possible after both.

---

## The Three Lenses

```
OOUX           → What exists and what you do to it   → Object model, IA, screens
Event Storming → What happens and in what order       → Event flows, process, pivotal moments  
DDD            → How the backend is structured        → Services, aggregates, contracts
```

These are not sequential dependencies — they are parallel lenses that inform each other. But there is a practical sequence for a designer/builder:

**1. OOUX first** — identify objects, relationships, CTAs, attributes. This gives you the data model and the vocabulary. You can't design anything without knowing what things exist and what you can do to them.

**2. Event storming second** — map the events that happen in the process. This gives you the process flow the UX needs to support, the pivotal moments that need UI affordances, and the hotspots where the current process breaks down.

**3. Representation last** — sketch screens. By this point you know:
- What objects exist and what's in them (OOUX Object Map → card and detail page content)
- What events happen and in what order (event storm → navigation flow and state transitions)
- Where the process breaks down (hotspots → where the UI needs to do the most work)

No blank canvas. No guessing about data.

---

## What Each Phase Produces for UX

### From OOUX

| Output | UX Use |
|---|---|
| Object Map (attributes, prioritized) | Card content (top 5-7 attributes), detail page fields |
| CTA Matrix | Buttons and actions on cards and detail pages |
| Nested Object Matrix | Navigation paths, nested content, what links to what |
| Anti-pattern audit | What NOT to design — the broken, isolated, masked, shapeshifting patterns to avoid |

### From Event Storming

| Output | UX Use |
|---|---|
| Happy path event flow | The primary navigation sequence through the product |
| Pivotal events | Moments requiring UI affordances — alerts, CTAs, state changes |
| Hotspots | Where the UI needs to do the most work (current process breaks down here) |
| Policies | Business rules that trigger automatically — what the system does without human input |
| Read models | What data needs to be visible to support each decision point |

### From Representation

- Cards (top attributes from Object Map + high-priority CTAs)
- Detail pages (full Object Map + all CTAs + nested objects)
- List pages (metadata fields become filters/sort options)
- Landing pages (mash-up of object cards)

---

## Event Storming Formats

Three formats — pick by scope and audience:

| Format | When | Participants | Output |
|---|---|---|---|
| Big Picture | Whole domain discovery | 10-30 people | Shared understanding, hotspots, candidate bounded contexts |
| Process Modeling | Specific end-to-end process | 5-15 people | Detailed process model, policies, alternative paths |
| Design-Level | Specific feature for implementation | 3-8 people | Aggregates, command/event flows, read model requirements |

For a solo designer/builder who already has domain knowledge: **Process Modeling** with AI as domain expert proxy. Covers the process in enough depth to design and build from.

---

## What You Don't Need for UX Design

From event storming — bounded contexts, aggregates, CQRS patterns, service contracts are engineering decisions. You don't need to resolve them to design screens. Hand those to the backend after your process model is complete.

---

## Related

- People Intelligence OOUX output: `tks-team-100x/projects/2026-06-05-people-intelligence-ooux/sessions/002-output/object-model.md`
- Event storming skill: `@user-nick-public/projects/2026-06-04-event-storming-skill/`
- OOUX skill: `@user-nick-public/projects/2026-06-02-ooux-skill/`
