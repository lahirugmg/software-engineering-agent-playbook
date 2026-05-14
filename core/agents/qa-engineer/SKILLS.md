# QA Engineer — Available Skills

Skills are invoked on demand. Each skill file is self-contained and can be composed into larger workflows. Every workflow follows three phases: **Pre-work** (plan what to test before testing begins), **Execution** (run the tests), **Post-work** (document defects and produce a sign-off verdict).

## Pre-work Skills

| Skill | File | When to use |
|---|---|---|
| test-planning | @core/skills/qa-engineer/test-planning.md | Define the scope, approach, and coverage targets for a testing effort |

## Execution Skills

| Skill | File | When to use |
|---|---|---|
| test-writing | @core/skills/qa-engineer/test-writing.md | Write test cases or automated tests for a specific feature or code path |
| browser-testing | @core/skills/qa-engineer/browser-testing.md | Test and debug web UI using live browser runtime data |
| performance-testing | @core/skills/qa-engineer/performance-testing.md | Design and execute performance tests against a defined baseline and threshold |
| exploratory-testing | @core/skills/qa-engineer/exploratory-testing.md | Run structured exploratory testing to find issues outside the specified scope |

## Post-work Skills

| Skill | File | When to use |
|---|---|---|
| bug-report | @core/skills/qa-engineer/bug-report.md | Produce a well-formed bug report with reproduction steps and expected behaviour |

## Composite Workflows

Composite skills chain pre-work, execution, and post-work into a single invocable workflow.

| Skill | File | Phase chain |
|---|---|---|
| feature-validation | @core/skills/qa-engineer/feature-validation.md | test-planning (pre-work) → test-writing → exploratory-testing (execution) → bug-report (post-work) |
