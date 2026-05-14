# Skill: stakeholder-trust

**Type:** atomic

## Purpose

Communicate outcomes, system changes, and agent-produced work to non-technical stakeholders in a way that builds confidence, maintains transparency, and enables informed decision-making.

## When to Invoke

- A significant feature, change, or incident response has been completed and stakeholders need to be informed.
- Agent-generated output is being presented to stakeholders who need to trust it.
- A technical decision or tradeoff needs to be explained to non-technical decision-makers.
- A risk or finding needs to be communicated without causing unnecessary alarm or false reassurance.

## Workflow

1. **Identify the audience and their goal.** Non-technical stakeholders are not a single group:
   - Executives want: impact on business goals, risk exposure, decisions required
   - Product managers want: what users will experience, what changed vs. what was planned, what comes next
   - Customers want: what changed for them, whether they need to do anything
   Write for one audience per communication. Mixed-audience communications satisfy no one.

2. **Lead with impact, not activity.** The first thing the reader learns should be what changed for them — not what the team did:
   - Good: "Users can now export reports in CSV format, reducing the manual step they needed before."
   - Bad: "We shipped the CSV export feature in v2.4.1 after three sprints of development."
   Activity is evidence of impact. Impact is what the reader cares about.

3. **Be specific about what changed.** Vague communications erode trust:
   - State which systems, users, or processes are affected
   - Quantify where possible: "reduced error rate from 4.2% to 0.3%", "affects all users who log in via SSO"
   - If something was expected but not delivered, name it and explain why

4. **Address risk honestly.** Do not minimise risk to avoid difficult conversations. If something is uncertain, say so:
   - "We have mitigated the immediate issue, but are still investigating the root cause."
   - "This change affects a small number of users; we will monitor for 48 hours before declaring it fully stable."
   Stakeholders who are surprised by a risk they were not told about lose trust faster than those who were informed of it upfront.

5. **State what happens next.** Every stakeholder communication should end with clarity about next steps:
   - What actions are required from the stakeholder?
   - What will the team do next?
   - When will the stakeholder hear from the team again?
   Absence of a next step creates anxiety and follow-up questions.

6. **Match the format to the channel.** A Slack message, an email, a slide deck, and a dashboard widget are different formats:
   - Slack: brief, timely, with a link to the full story
   - Email: structured, with a subject line that stands alone
   - Executive presentation: three to five key points, visualised impact, decision request clearly stated
   Do not send a long email when a Slack message with a link would be more effective.

7. **Review with a non-technical reader.** Before sending, have someone from the target audience read the draft. If they have questions after reading it, the draft is not done.

## Output

- **Stakeholder communication** — audience-appropriate, impact-led, specific, with clear next steps
- **Supporting material** — metrics, before/after comparisons, or timeline as needed by the audience
- **Review record** — confirmation that a member of the target audience reviewed the draft before sending
- **Distribution record** — when it was sent and to whom
