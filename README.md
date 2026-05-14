# Software Engineering Agent Playbook

A library of **agents**, **skills**, and **guidelines** for AI software engineering assistants — plus thin adapters that map the core definitions to Claude Code, GitHub Copilot, Cursor, and others.

The playbook covers the full software development lifecycle — from requirements and architecture through implementation, testing, security, deployment, operations, and documentation. Each SDLC stage has a dedicated agent with its own behavioral rules and skills, so no stage is a gap and no agent carries responsibility beyond its scope.

## Model

Three layers:

| Layer | Location | Loaded | Purpose |
|---|---|---|---|
| **Skills** | `core/skills/<agent>/` | On demand | Task execution invoked when needed |
| **Guidelines** | `core/knowledge/` | Always available | Normative rules agents consult during execution |
| **Adapters** | `adapters/` | — | Maps core to each tool's native format |

Behavioral rules live inline in each agent's `agent.md`. Skills define what an agent **does**. Guidelines define how an agent **behaves** during execution — coding standards, security rules. Adapters translate both into the format each platform understands, with no content of their own. Tool access (what an agent can invoke: bash, web search, file reads) is declared in each adapter's frontmatter and configured per project.

![Three-layer architecture: Adapters import from Core, which contains Skills and Guidelines](docs/diagrams/architecture.png)

## Agent Workflow

Phase logic applies inside an individual agent workflow, not as a human-versus-AI operating model. A typical agent workflow has three steps: prepare the work, execute the change, and verify the result.

For a software engineer, that usually means clarifying scope, writing a spec or test-first plan, implementing the change, and then running tests and reviewing the output before it is considered done.

![Agent workflow: Preparation leads to Execution leads to Verification](docs/diagrams/phase-model.png)

## Agents

Eight agents cover the full software development lifecycle:

| Agent | Skills |
|---|---|
| **Business Analyst** | requirements-gathering, user-story, prd-writing, process-analysis, roadmap, sprint-planning, estimation, innovation-discovery |
| **Technical Architect** | system-design, architecture-review, adr-writing, technical-spec, feasibility-analysis |
| **Software Engineer** | code-review, agent-output-review, debugging, refactoring, spec-writing, tech-debt, dependency-update, migration, intrapreneur-workflow |
| **QA Engineer** | test-planning, test-writing, bug-report, performance-testing, exploratory-testing |
| **Security Engineer** | security-audit, threat-modeling, vulnerability-assessment, dependency-vulnerability, compliance-review |
| **DevOps Engineer** | pipeline-design, deployment, infrastructure-as-code, containerization, build-optimization |
| **SRE** | observability, alerting, incident-response, post-mortem, capacity-planning, runbook, root-cause-analysis |
| **Technical Writer** | api-docs, user-guide, runbook-writing, onboarding-guide, changelog, stakeholder-trust |

Together the eight agents span the full SDLC: requirements → architecture → implementation → testing → security → deployment → operations → documentation. Each agent's behavioral rules are defined inline in its `agent.md`. Available skills are declared in the agent's `SKILLS.md` — a QA Engineer has no awareness of deployment skills, and vice versa.

![SDLC agent coverage: eight agents from Business Analyst through Technical Writer](docs/diagrams/sdlc-flow.png)

## Skill Hierarchy

Skills are either **atomic** (single responsibility) or **composite** (invoke other skills):

```
core/skills/software-engineer/
  code-review.md          ← atomic: reviews human-authored code against standards
  agent-output-review.md  ← atomic: verifies AI-generated code for drift, logic gaps, missing edge cases
  debugging.md            ← atomic: diagnoses and fixes a specific bug
  ship-feature.md         ← composite: spec-writing → code-review → testing
  intrapreneur-workflow.md ← composite: innovation-discovery → spec-writing → build → agent-output-review → stakeholder-trust
```

Composite skills reference sub-skills in sequence. Atomic skills stay self-contained and are reused by composites without duplication. This hierarchy means complex workflows are built by composition, not by writing monolithic prompts.

## Adapters

Adapters are thin by design — each one imports from `core/` using the tool's native syntax:

| Tool | Native format | Import mechanism |
|---|---|---|
| Claude Code | `.claude/agents/*.md` with YAML frontmatter | `@path/to/file` |
| GitHub Copilot | `.github/` chat participants | Tool-specific |
| Cursor | `.cursor/rules/*.md` | `@path/to/file` |

No content lives in adapters. Updating a core skill or guideline is automatically reflected in all adapters that import it.

## Structure

### Core

```
core/
  agents/
    business-analyst/
      agent.md            ← role, behavioral rules, pointer to SKILLS.md
      SKILLS.md           ← index of skills this agent can invoke
    technical-architect/
      agent.md
      SKILLS.md
    software-engineer/
      agent.md
      SKILLS.md
    qa-engineer/
      agent.md
      SKILLS.md
    security-engineer/
      agent.md
      SKILLS.md
    devops-engineer/
      agent.md
      SKILLS.md
    sre/
      agent.md
      SKILLS.md
    technical-writer/
      agent.md
      SKILLS.md

  knowledge/
    development-best-practices.md  ← design principles, coding standards, and team conventions
    security-guidelines.md         ← developer-facing rules for writing secure code

  skills/
    business-analyst/
      requirements-gathering.md
      user-story.md
      prd-writing.md
      process-analysis.md
      roadmap.md
      sprint-planning.md
      estimation.md
      innovation-discovery.md   ← explore unspoken needs before a ticket exists
      requirements-to-prd.md    ← composite: requirements-gathering → user-story → prd-writing
    technical-architect/
      system-design.md
      architecture-review.md
      adr-writing.md
      technical-spec.md
      feasibility-analysis.md
      design-and-document.md    ← composite: system-design → adr-writing → technical-spec
    software-engineer/
      code-review.md
      agent-output-review.md    ← atomic: verifies AI-generated code for drift, logic gaps, missing edge cases
      debugging.md
      refactoring.md
      spec-writing.md
      tech-debt.md
      dependency-update.md
      migration.md
      ship-feature.md           ← composite: spec-writing → code-review → testing
      intrapreneur-workflow.md  ← composite: discovery → spec → build → review → trust
    qa-engineer/
      test-planning.md
      test-writing.md
      bug-report.md
      performance-testing.md
      exploratory-testing.md
      feature-validation.md     ← composite: test-planning → test-writing → exploratory-testing → bug-report
    security-engineer/
      security-audit.md
      threat-modeling.md
      vulnerability-assessment.md
      dependency-vulnerability.md
      compliance-review.md
      security-review.md        ← composite: threat-modeling → security-audit → vulnerability-assessment
    devops-engineer/
      pipeline-design.md
      deployment.md
      infrastructure-as-code.md
      containerization.md
      build-optimization.md
      ship-service.md           ← composite: pipeline-design → containerization → infrastructure-as-code → deployment
    sre/
      observability.md
      alerting.md
      incident-response.md
      post-mortem.md
      capacity-planning.md
      runbook.md
      root-cause-analysis.md
      incident-to-action.md     ← composite: incident-response → root-cause-analysis → post-mortem
    technical-writer/
      api-docs.md
      user-guide.md
      runbook-writing.md
      onboarding-guide.md
      changelog.md
      stakeholder-trust.md      ← atomic: communicate outcomes and build trust with stakeholders
      document-release.md       ← composite: api-docs → changelog → stakeholder-trust

adapters/
  claude/
    *.md                  ← one file per agent: YAML frontmatter + @-imports from core/
    SPEC.md               ← architecture decisions for the Claude Code adapter
    MCP.md                ← catalogue of MCP server options for optional tool integrations
    examples/
      mcp.json            ← annotated example .mcp.json for project setup
  copilot/
  cursor/
```

## Quick Start

**Claude Code**

See [`adapters/claude/GETTING-STARTED.md`](adapters/claude/GETTING-STARTED.md) for the full setup guide, including:
- Two deployment models (fork vs. submodule)
- Writing your project's `CLAUDE.md`
- Multi-role configuration for developers who wear more than one hat
- Role selection guide (which agent and skill to invoke for each task)
- Customizing the knowledge files for your team

Short version:
1. Fork or clone this repo — your project code lives alongside `core/`.
2. Copy `adapters/claude/` into `.claude/agents/`.
3. Create a `CLAUDE.md` pointing to your architecture docs, runbooks, and issue tracker.
4. Optionally copy `adapters/claude/examples/mcp.json` to `.mcp.json` for GitHub, Slack, or database integrations.

**GitHub Copilot** — copy `adapters/copilot/` files into `.github/` (adapter files coming soon)

**Cursor** — copy `adapters/cursor/` files into `.cursor/rules/` (adapter files coming soon)

## Principles

- **Minimal core**: skills and two guideline files — nothing else
- **Non-redundant**: each skill and guideline has one authoritative source
- **On-demand skills**: skills are invoked when needed, never always-loaded
- **Composable**: atomic skills combine into composite skills without duplication
- **Agent-scoped behavior**: each agent defines only the behavioral rules relevant to its role
- **Full SDLC coverage**: every stage from requirements to documentation has a dedicated agent and skills

## Contributing

1. All content goes in `core/` — never in `adapters/`
2. Skills go in `core/skills/<agent>/` — never nested inside `core/agents/`
3. Skills must be either atomic (single task) or explicitly composite (declare which sub-skills they invoke)
4. New agents must define behavioral rules inline in `agent.md` and list available skills in `SKILLS.md`
5. Adapters require no manual updates — they reflect `core/` automatically via imports

## License

Apache 2.0 — see LICENSE file
