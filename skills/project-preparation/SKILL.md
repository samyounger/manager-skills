---
name: project-preparation
description: Prepare or update a Jira epic for an engineering project from product inputs. Use when a user is starting project discovery, asks to shape an epic, needs a Portfolio Status section, or wants product references and acceptance criteria captured before delivery planning.
metadata:
  version: 1.0.0
---

# Project Preparation

## Before Starting

Read `$HOME/.agents/manager-skills/.agents/em-context.md` when it exists. Use it to ground ownership, team boundaries, and current priorities; do not invent durable team context when it is absent.

Confirm the Jira epic key and read the current epic before editing it. Read the product inputs the user provides—such as a PRD, JPD item, design artifact, or prior proposal—directly from their source. Treat the epic as a concise portfolio and delivery-shaping artifact, not a replacement for the source material.

When the request includes a Jira update, use Atlassian MCP tools first. Use Atlassian Document Format (ADF) for the epic description and Acceptance Criteria field. For RAD issues, Acceptance Criteria is `customfield_10209`; never substitute a description or checklist for it.

### Jira dates in ADF descriptions

When adding a real calendar date anywhere in a Jira description, use Atlassian's native ADF `date` inline node rather than a text date. This includes `Last reviewed`, a known `Target date`, and dates added to any other description section. Keep non-dates such as `TBD` as text.

For a date-only value, calculate the Unix epoch in milliseconds at the start of that date in UTC and store it as a string. For example, 24 September 2026 is:

```json
{
  "type": "date",
  "attrs": { "timestamp": "1790208000000" }
}
```

In a `Last reviewed` paragraph, place this node immediately after the bold label; do not embed the date in a text node. This preserves Jira's native, locale-aware date rendering.

For projects that affect several teams, a broader portfolio, platform capabilities, or AI-enabled delivery, read [references/sources.md](references/sources.md). It provides current planning lenses and primary sources for the senior engineering manager scope; do not load it for a routine single-team epic.

## Outcome

Create or refresh an epic that lets an engineering manager and delivery team quickly understand:

- what product problem the project addresses;
- the intended outcome and the owning team's contribution;
- current portfolio health, risks, dependencies, and required management action;
- the source material that supports the framing; and
- observable acceptance criteria, without prematurely prescribing an implementation.

Keep the language product-facing. Avoid low-level technical design unless the user explicitly requests it.

For a senior engineering manager, the portfolio-level contribution is to make strategic value, cross-team decisions, dependencies, delivery health, and required intervention visible. It is not to centralize every delivery decision or to add reporting for its own sake.

## Epic Description Format

Use this order. Retain an existing section when it is still accurate; do not erase user-authored material merely to make the template uniform.

### Portfolio Status

Put this section at the top of the epic description:

```text
## Portfolio Status

*Lead:* <name>
*Phase:* Discovery / Steel Thread / Planning / Delivery / Rollout
*Target date:* <date or TBD>
*Date confidence:* High / Medium / Low
*Last reviewed:* <date>
*Latest meaningful progress:*
- <the most recent decision, validated finding, or completed planning step>
*Next milestone:*
- <the next outcome that reduces uncertainty or advances delivery>
*Main risk:* <the single most material current risk>
*Dependency:* <the most material external decision, team, or artifact>
*EM attention needed:* None / Decision / Escalation / Dependency / Scope call
---
```

Populate this with current evidence, not optimistic placeholders. For a new project with no date, designs, estimates, or validated dependencies, `Discovery`, `TBD`, and `Low` confidence are appropriate. State the uncertainty plainly.

Update Portfolio Status weekly while the project is active and whenever scope, sequencing, confidence, risk, dependency, or management attention materially changes. This is a portfolio summary, not a log of every ticket update.

### Problem we're solving

Describe the customer or business problem, its consequence, and why it matters now. Tie claims to the supplied source material. Explain the owning team's relevant part of the problem without taking ownership of other teams' work.

### Solution

Describe the intended product outcome and the owning team's contribution. Name meaningful scope boundaries and dependent workstreams when they prevent misunderstanding. Do not imply that designs, delivery dates, technical architecture, or cross-team commitments have been agreed when they have not.

### References

Group source links by discipline, using the `RAD-88165` pattern:

```text
📚 Product:
  - <PRD, JPD item, research, or product decision>
🖌️ Design:
  - <design file or explicitly note that design is not yet available when material>
🏗️ Engineering:
  - <technical proposal, ADR, spike, or relevant epic>
```

Include only sources that help someone understand or shape the project. Do not create empty placeholder links. If an artifact is not yet available, say so only when it is useful context—for example, “No designs yet; discovery is in progress.”

## Acceptance Criteria

Store acceptance criteria in the dedicated Jira field, not in the description. Write criteria as observable product or workflow outcomes.

Good criteria:

- identify who can do what and in which applicable context;
- distinguish enabled and unavailable states where rollout or eligibility matters;
- state what information is saved, carried forward, or visible when it is material to the outcome;
- allow the implementation approach to remain open.
- does not go into low level technical detail.
- observability criteria of user journeys or technical flows we want to track.

Avoid criteria that prescribe internal data structures, APIs, UI components, implementation languages, or test mechanics unless those details are an explicit product constraint.

## Working Approach

1. Read the epic and supplied references, then extract the product goal, owning-team scope, constraints, known dependencies, and open questions.
2. Draft a concise Portfolio Status from actual project maturity. Do not create a target date or high confidence without evidence.
3. Draft the problem and solution in business and user language; make the scope boundary explicit.
4. Add source links under the relevant reference groups.
5. Add a small set of outcome-based acceptance criteria in the Jira Acceptance Criteria field.
6. Re-read the updated fields and confirm the description order, links, and acceptance criteria survived in ADF.

If information is missing but a safe status can be written, record the uncertainty rather than blocking. Ask the user only when a choice would materially alter scope, ownership, commitment, or an external update.
