# Skill: deployment

**Type:** atomic

## Purpose

Plan and execute a deployment to a target environment with a defined rollback plan, post-deploy verification, and a clear declaration of success or failure. A deployment without a rollback plan is a gamble.

## When to Invoke

- A change needs to be deployed to staging or production.
- A deployment needs to be planned in advance for a high-risk or high-traffic release.
- A previously failed deployment needs to be assessed and retried or rolled back.

## Workflow

1. **Confirm prerequisites.** Before deploying:
   - [ ] The artifact to be deployed has passed all pipeline stages in a lower environment
   - [ ] The same artifact (same image digest or binary hash) is being promoted — not rebuilt
   - [ ] The deployment window has been communicated to relevant stakeholders
   - [ ] A rollback plan is documented and the rollback has been tested or is known to work
   - [ ] The change log is understood: what is changing, what configuration is changing, what migrations will run

2. **Define the deployment strategy.** Choose the appropriate strategy for the risk level:
   - **Blue/green** — full switch from old version to new; rollback = switch back
   - **Canary** — route a small percentage of traffic to new version; expand or roll back based on metrics
   - **Rolling** — replace instances one at a time; slower but no double capacity required
   - **Feature flag** — code ships but feature is off; activate separately from deployment

3. **Define the rollback trigger.** Specify exactly what will trigger a rollback:
   - Error rate exceeds X% for Y minutes
   - p95 latency exceeds X ms for Y minutes
   - Health check fails on > N instances
   Rollback triggers must be measurable and automatic where possible.

4. **Execute the deployment.** Follow the defined strategy:
   - Apply the change in the target environment
   - Monitor the rollback triggers in real time during the deployment
   - Do not leave the deployment unmonitored during the initial soak period

5. **Run smoke tests.** Immediately after deployment, execute a minimal set of automated checks against the production environment:
   - Key user flows (login, primary action, data read)
   - Health check endpoints
   - A sample of monitoring metrics
   Smoke tests are not regression tests — they are the minimum to confirm the service is alive.

6. **Define the soak period.** The deployment is not complete until metrics are stable for a defined period after the last change. Typical soak: 15–30 minutes for routine changes, 1–4 hours for high-risk changes.

7. **Declare the outcome.** Explicitly state: COMPLETE (all green through soak), ROLLED BACK (trigger hit, rollback executed), or INVESTIGATING (anomaly detected, not yet rolled back).

## Output

- **Pre-deploy checklist** — confirmed prerequisites
- **Deployment log** — what was deployed, when, by whom, with artifact reference
- **Smoke test results** — pass/fail for each check
- **Post-deploy metrics summary** — key metrics during the soak period
- **Outcome declaration** — COMPLETE / ROLLED BACK / INVESTIGATING with rationale

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The rollback plan is obvious, I don't need to write it down" | Under incident pressure, "obvious" plans fail. The rollback steps must be written before deploy, not recalled during one. |
| "We can monitor it after the soak period" | Post-deploy monitoring must begin during the soak period. Anomalies caught at 5 minutes are cheaper than anomalies caught at 5 hours. |
| "Smoke tests are enough verification" | Smoke tests confirm the service is alive. They don't confirm correct behavior. The soak period and metrics monitoring fill the gap. |
| "We've deployed this before, no need for a checklist" | Familiarity breeds skipped steps. The checklist exists because the same team makes the same mistakes on familiar deployments. |
| "I'll run the smoke tests manually, it's faster" | Manual smoke tests are not repeatable and not auditable. Automate them — then run them manually as a supplement if needed. |

## Red Flags

- Deploying without a documented rollback plan
- Rollback plan that has never been tested in any environment
- Declaring the deployment COMPLETE before the soak period ends
- No defined rollback trigger — the team is guessing when to roll back
- Smoke tests skipped because "the pipeline already tested this"
- Leaving the deployment unmonitored during the soak period
- Deploying a rebuilt artifact instead of promoting the pipeline-verified artifact

## Verification

Before declaring COMPLETE:

- [ ] Pre-deploy checklist confirmed — same artifact promoted (not rebuilt)
- [ ] Rollback plan documented and rollback trigger is specific and measurable
- [ ] Deployment strategy chosen and matches the risk level of the change
- [ ] Smoke tests ran against the production environment after deploy
- [ ] Soak period ran to completion with metrics checked at start, midpoint, and end
- [ ] Deployment log records what, when, and by whom with artifact reference
- [ ] Outcome declaration is explicit: COMPLETE / ROLLED BACK / INVESTIGATING
