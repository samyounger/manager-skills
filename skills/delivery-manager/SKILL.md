---
name: delivery-manager
description: Generate a concise, evidence-based engineering portfolio report from Jira. Use for daily portfolio reporting, delivery-health reviews, and identifying only the epic-level items that need Engineering Manager attention.
metadata:
  version: 1.0.0
---

# Delivery Manager

Create a skimmable engineering portfolio report that helps an Engineering Manager decide where to intervene. The report is an epic-level management brief, not a Jira activity digest.

## Grounding and evidence

Establish the portfolio or Delivery Team scope, the Engineering Manager, the Jira scope, and the output file from the caller's context. Read `.agents/em-context.md` when it exists, so the report reflects The Delivery Team's remit and current priorities. Use live Jira data as the source of truth. Read the epic before judging its health, including its Portfolio Status when present. Inspect child tickets only to establish a material delivery risk or the latest meaningful signal; do not surface them in the report unless they are essential to explaining that risk.

Before classifying Needs EM Attention, read [the Engineering Manager role-scope source](references/sources.md#engineering-manager-role-scope) and [the team-operation source](references/sources.md#team-operation-and-role-inputs) when they are available. Use them to distinguish a system-level exception the Engineering Manager should address from routine project, technical, product, quality, or release work owned by another role. Where an attention item requires another role's evidence, decision, or hand-off, name that role in `Input needed from`.

When a prior daily report exists, compare it with current evidence. Report a genuine change, decision, risk, or delivery signal rather than restating routine ticket churn. Do not infer target dates, confidence, ownership, or progress from activity alone. If confidence is unavailable in Jira, write `Confidence: not recorded`.

For a cross-team, unusually complex, AI-enabled, or delivery-stability-sensitive epic, read [the delivery reporting sources](references/sources.md). Apply only the relevant lens: outcome and forecast variance, decision rights and dependencies, or delivery stability. Do not add source citations or a generic framework checklist to the report.

## What to include

Report only relevant, active epics within the supplied portfolio scope. Exclude completed, cancelled, duplicate, or purely operational work unless it has a live risk or requires an Engineering Manager decision. Do not include a completed or cancelled epic merely because it has a high workflow-priority number.

Classify an epic as **Needs EM Attention** only when the Engineering Manager can meaningfully decide, unblock, escalate, secure ownership/capacity, or make a scope or sequencing call. Put healthy, advancing work in **On Track**. Use **Planning Watchlist** for credible upcoming work that is not start-ready or lacks a material planning input.

Begin the report with a **Focus today** queue containing the one to three most important items from Needs EM Attention. State the specific EM action and the evidence-based deadline or `not recorded`; do not introduce an epic that is absent from Needs EM Attention. When no action is required, say so plainly in this queue.

Keep the report readable in under five minutes: prefer no more than four attention items, six on-track items, and three watchlist items. Omit an empty detail section. Prioritise the most consequential evidence when the portfolio is larger.

Use the actual Jira summary in every epic heading, and never mention a Jira key without its summary alongside it. Every epic reference must hyperlink the Jira key to `https://user-testing.atlassian.net/browse/<KEY>`, using the form `<Epic summary> ([<KEY>](https://user-testing.atlassian.net/browse/<KEY>))`. Prefer product- and team-readable descriptions over Jira terminology. Use en-GB spelling.

## Status and output

Prefix every `Status` value with the workflow emoji in [the report format reference](references/report-format.md#status-legend). Preserve the workflow state recorded in Jira; do not use an emoji to imply health or confidence. Use the separate `Focus` field to convey the urgency of an EM action. Include `Target risk` and `Signal freshness` using the evidence rules in that reference.

Sort **Needs EM Attention** by Focus urgency, then documented action deadline, then consequence. Sort **On Track** and **Planning Watchlist** in descending status priority from the legend: Cancelled (`7`), Blocked (`6`), Discovering (`5`), Proposing (`4`), Ready (`3`), Developing (`2`), Verifying (`1`), then Done (`0`). Use the closest matching priority for an unrecognised Jira workflow state and sort items with equal priority by consequence: the most material customer, delivery, or management impact first.

Write the finished Markdown report to the caller-specified output file. Use the caller's local date for any date-based filename. Update only that day's generated report; do not alter historical reports. If no output file is provided, ask for one rather than choosing a location. Follow [the report format reference](references/report-format.md) exactly for front matter, headings, fields, link form, and absence wording.

Do not update Jira as part of generating the report. If the user asks for Jira edits as a separate action, use Atlassian MCP and ADF for Jira descriptions, comments, and Acceptance Criteria.
