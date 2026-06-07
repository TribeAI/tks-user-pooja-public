# AGENTS.md

Guidance for AI agents working in this source.

---

## About This Brain

This is Pooja Brown's personal brain — CTO at Tribe AI. It holds working knowledge, mental models, frameworks, and decisions developed through the 100X product work. It is distinct from the team brain (`tks-team-100x`), which holds project-specific deliverables and sessions.

**What lives here:** Durable thinking — frameworks, competitive landscape, methodology notes, mental models, decisions that travel beyond a single project.

**What lives in the team brain:** Project-specific sessions, OOUX outputs, identity architecture decisions, event storm artifacts.

---

## Key Concept: Per-Project Conventions

Each project under `projects/` may have its own `CLAUDE.md` with local conventions. When working inside a project, read its `CLAUDE.md` first.

---

## Specialized Guides

| Guide | When to Read |
|-------|--------------|
| [.agents/repo-structure.md](.agents/repo-structure.md) | Always — understand the source layout |
| [.agents/project-bootstrap.md](.agents/project-bootstrap.md) | When creating a new project |

For Obsidian CLI, formatting, git commit conventions, and executable sessions — reference the shared guides in `tks-team-100x/.agents/` until Pooja-specific versions are needed.

---

## Areas

This brain uses an areas layer for durable knowledge that isn't project-scoped:

```
areas/
  product/           OOUX, event storming, design methodology
  architecture/      System design, identity service, data models
  recruiting-app/    People Intelligence — decisions, competitive landscape, UX
  leadership/        CTO mental models, team, stakeholder work
```

Areas are not time-bounded. They accumulate and evolve. Projects produce artifacts that graduate into areas when they become durable reference material.
