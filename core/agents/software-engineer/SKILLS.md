# Software Engineer — Available Skills

Skills are invoked on demand. Each skill file is self-contained and can be composed into larger workflows. Composite software-engineer workflows typically follow preparation, execution, and verification.

## Atomic Skills

| Skill | File | When to use |
|---|---|---|
| code-review | @core/skills/software-engineer/code-review.md | Review human-authored code before merge |
| agent-output-review | @core/skills/software-engineer/agent-output-review.md | Verify AI-generated code for drift, logic gaps, missing edge cases |
| debugging | @core/skills/software-engineer/debugging.md | Diagnose and fix a specific reported bug |
| refactoring | @core/skills/software-engineer/refactoring.md | Improve structure without changing behaviour |
| spec-writing | @core/skills/software-engineer/spec-writing.md | Write a testable spec before implementation begins |
| tech-debt | @core/skills/software-engineer/tech-debt.md | Identify, assess, and plan remediation of technical debt |
| dependency-update | @core/skills/software-engineer/dependency-update.md | Update dependencies safely with regression verification |
| migration | @core/skills/software-engineer/migration.md | Plan and execute schema, data, or API migrations |

## Composite Skills

Composite skills invoke other skills in sequence. Sub-skills are listed in execution order.

| Skill | File | Sub-skills |
|---|---|---|
| ship-feature | @core/skills/software-engineer/ship-feature.md | spec-writing → implementation → code-review |
| intrapreneur-workflow | @core/skills/software-engineer/intrapreneur-workflow.md | discovery → spec-writing → ship-feature → agent-output-review → stakeholder communication |
