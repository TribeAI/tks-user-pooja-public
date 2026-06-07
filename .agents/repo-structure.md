# Source Structure

This is a **source** (also called a **brain**) within the `workspace-pooja` Obsidian vault. It is Pooja Brown's personal brain — a standalone git repo that lives as a subdirectory of the workspace.

---

## Workspace Context

```
workspace-pooja/                        # The Obsidian vault
  .obsidian/                            # Vault config (workspace-level)
  @user-pooja-public/                   # This brain (github: TribeAI/tks-user-pooja-public)
  @user-nick-public/                    # Nick's public brain (github: TribeAI/tks-user-nick-public)
  tks-team-100x/                        # 100X team brain — project deliverables (github: TribeAI/tks-team-100x)
  @engagement-cushman/                  # Cushman & Wakefield engagement brain
```

---

## Brain Layout

```
@user-pooja-public/
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
