# Source Structure

This is a **source** (also called a **brain**) within the `workspace-pooja` Obsidian vault. It is Pooja Brown's personal brain — a standalone git repo that lives as a subdirectory of the workspace.

---

## Workspace Context

```
workspace-pooja/
  .obsidian/                 Vault config (workspace-level)
  @user-pooja/               This brain (Pooja's personal knowledge)
  @user-nick-public/         Nick's public brain
  tks-team-100x/             100X team brain (project deliverables)
  @engagement-cushman/       Cushman engagement brain
```

---

## Brain Layout

```
@user-pooja/
  README.md          Overview
  CLAUDE.md          Claude Code pointer to AGENTS.md
  AGENTS.md          Agent guidance hub
  .agents/           Specialized guides
  projects/          Time-bounded initiatives
  areas/             Durable knowledge by domain
```

### areas/ — durable knowledge

Unlike projects (time-bounded), areas accumulate over time. They don't have sessions or charters — they have notes, frameworks, and reference material that evolves.

```
areas/
  product/           OOUX, event storming, representation, design methodology
  architecture/      Identity service, data models, system design decisions
  recruiting-app/    People Intelligence — competitive landscape, UX decisions, design
  leadership/        CTO mental models, stakeholder work, team
```

### projects/ — time-bounded work

Same structure as the team brain. Use the project-bootstrap guide from `tks-team-100x/.agents/project-bootstrap.md` until a Pooja-specific version is needed.

---

## Key Principles

1. **This brain holds thinking, not deliverables.** Deliverables live in the team brain. Mental models, frameworks, and durable reference live here.
2. **Areas graduate from projects.** When a project produces something durable — a competitive landscape, a design framework — it moves into the relevant area.
3. **Shareable by default.** This brain is structured to be shared with the 100X team. Nothing confidential belongs here.
