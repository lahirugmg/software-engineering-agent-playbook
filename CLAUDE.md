# CLAUDE.md — Software Engineering Agent Playbook

This file guides Claude Code when working on this repository itself.

## What This Project Is

A library of agents, skills, and guidelines for AI software engineering assistants. Three layers:

| Layer | Location | Purpose |
|---|---|---|
| Skills | `core/skills/<agent>/` | Task execution invoked on demand |
| Guidelines | `core/knowledge/` | Normative rules agents consult during execution |
| Adapters | `adapters/` | Maps core to each tool's native format |

Tool access (what an agent can invoke) is declared in each adapter's YAML frontmatter and configured per project via `.mcp.json`.

## Architecture Rules

When making changes, enforce these invariants:

1. **No content in adapters.** `adapters/` files import from `core/` only. Never add definitions there.
2. **Skills go in `core/skills/<agent>/`**, never inside `core/agents/<agent>/`.
3. **Atomic skills have one responsibility.** If a skill file orchestrates other skills, it is composite and must say so explicitly.
4. **`core/knowledge/` has two files only**: `development-best-practices.md` and `security-guidelines.md`. Do not add more without a strong reason — thin pointer files belong in the adapter's Knowledge Access section, not here.
5. **Per-agent files must stay in sync.** If you add a skill, add it to `SKILLS.md`.
6. **Every agent must have an Agent Workflow behavioral rule.** It must be the first rule in `## Behavioral Rules`, must define pre-work, execution, and post-work phases for that role, and must include a gate condition for each phase transition.
7. **SKILLS.md files are organised by phase.** Skills are listed under Pre-work, Execution, or Post-work headings — not just as a flat list. Composite skills are listed separately under Composite Workflows.

## Structure

```
core/
  agents/<agent>/
    agent.md        — role + behavioral rules + pointer to SKILLS.md
    SKILLS.md       — index of skills this agent can invoke
  skills/<agent>/
    <skill>.md      — atomic or composite skill definition
  knowledge/
    development-best-practices.md
    security-guidelines.md

adapters/
  claude/
    <agent>.md      — YAML frontmatter + @-imports from core/
    SPEC.md         — architecture decisions for the Claude Code adapter
    MCP.md          — catalogue of MCP server options
    examples/
      mcp.json      — annotated example .mcp.json for project setup
```

## Skill File Format

```markdown
# Skill: <name>

**Type:** atomic | composite

## Purpose
<one paragraph>

## When to Invoke
- <bullet>

## Workflow

1. **Step name.** Description.
   - Sub-bullet
   **Gate:** condition before proceeding.

## Output
- **Artifact name** — description
```

## Agent File Format (core/agents/)

```markdown
# <Agent Name> Agent

## Role
<paragraph>

## Skills
Invoked on demand. See @core/agents/<agent>/SKILLS.md.

## Behavioral Rules

### Agent Workflow

**<one-line gate summary for this role>**

Every task follows three phases — in order, without skipping:

**Pre-work.** <what the agent does before producing any primary artifact>. Gate: <condition that must be true before execution begins>.

**Execution.** <what the agent produces>. Gate: <condition that must be true before post-work begins>.

**Post-work.** <how the agent verifies and signs off>. Gate: <condition that must be true before handoff>.

These phases are sequential. <Specific violation example for this role> violates this workflow.

### Rule Name
<prose rules>
```

The `### Agent Workflow` rule must always be the first rule in `## Behavioral Rules`. It is required in every agent — not optional. The gate statements are enforced constraints, not suggestions.

## Adapter File Format (adapters/claude/)

```yaml
---
name: <agent-name>
description: <one sentence — what this agent does and when Claude Code spawns it>
model: claude-opus-4-7
tools: Read, Write, Edit, Bash, WebSearch
color: <color>
---
```

Body: `@core/agents/<agent>/agent.md` import, plus Skill Loading and Knowledge Access sections.

## Agents and Their Colors

| Agent | Color |
|---|---|
| business-analyst | yellow |
| technical-architect | purple |
| software-engineer | blue |
| qa-engineer | green |
| security-engineer | red |
| devops-engineer | orange |
| sre | cyan |
| technical-writer | white |

## Knowledge Paths in This Repo

| Source | Path |
|---|---|
| development-best-practices | `core/knowledge/development-best-practices.md` |
| security-guidelines | `core/knowledge/security-guidelines.md` |
| codebase | this repository — read files directly |
| requirements | README.md + open GitHub issues |
| architecture | README.md, `adapters/claude/SPEC.md` |

## Working on This Repository

This repository includes the `agent-skills/` plugin (Addy Osmani's lifecycle-based skill pack). Use its slash commands when working on the playbook itself:

| When doing this… | Use this |
|---|---|
| Adding a new agent or skill | `/spec` — write the spec before touching files |
| Reorganising phase structure | `/plan` — break it into discrete tasks |
| Reviewing new skill files before committing | `/review` — code-review-and-quality |
| Making a non-obvious structural decision | `doubt-driven-development` — adversarial review before the decision stands |
| Scoping a new role or skill category | `interview-me` / `idea-refine` |

When enriching or adding skills to `core/`, use the agent-skills skill files as reference material — they are the upstream source for `incremental-implementation`, `code-simplification`, `source-driven-development`, `doubt-driven-development`, `interview-me`, `idea-refine`, `performance-optimization`, and `browser-testing`.

## Common Tasks

**Add a new skill to an existing agent:**
1. Create `core/skills/<agent>/<skill>.md` following the skill format above.
2. Add a row to `core/agents/<agent>/SKILLS.md` under the correct phase heading (Pre-work, Execution, or Post-work).
3. If it's a composite skill, declare which sub-skills it invokes and add a `**Phases:**` line identifying which phase each sub-skill belongs to.

**Add a new agent:**
1. Create `core/agents/<agent>/agent.md` and `SKILLS.md`.
   - `agent.md` must have `### Agent Workflow` as the first behavioral rule, with gate conditions for each phase.
   - `SKILLS.md` must organise skills under Pre-work, Execution, Post-work, and Composite Workflows headings.
2. Create `core/skills/<agent>/` directory and at minimum one skill per phase (pre-work, execution, post-work) and one composite workflow skill.
3. Create `adapters/claude/<agent>.md` with YAML frontmatter and @-imports.
4. Add the agent to the Agents table in `README.md` with its composite workflow and phase breakdown.

**Update a behavioral rule:**
1. Edit `core/agents/<agent>/agent.md`.

**Add an MCP-backed tool option:**
1. Add an entry to `adapters/claude/MCP.md` under the appropriate section.
2. Add the server to `adapters/claude/examples/mcp.json` with metadata keys.

## What Not to Do

- Do not add files to `core/knowledge/` unless they contain actual reference content agents read during execution — not pointers to where things might be.
- Do not put skill content inside `agent.md` — agent.md has pointers, skills have content.
- Do not create adapter files with content that diverges from their `core/` imports.
- Do not edit `SKILLS.md` without checking the corresponding skill files exist.
- Do not add a behavioral rule to `agent.md` before the `### Agent Workflow` rule — it must always be first.
- Do not create a new agent without skills covering all three phases and a composite workflow skill.
