# Management and investigations

Choose a pattern based on the reader's task. Do not force all headings into every response.

## Stakeholder or delivery update

Lead with whether the intended outcome or date is achievable, when the evidence supports that judgement. Then cover:

- **What changed:** A result or meaningful movement since the previous update.
- **What matters:** Impact, dependency, risk, or decision needed.
- **Next action:** Action and established owner or date.

Separate activity from outcome. Say whether a date is a commitment, forecast, or proposal. Avoid unexplained status colours and long lists of completed tasks. Keep project-specific terminology understandable to the audience.

## Investigation or incident

Use **Finding → evidence → next check**.

- Put current impact and any urgent action first.
- Mark a suspected cause as a hypothesis. Explain what observation supports it and what would distinguish competing explanations.
- Keep confirmed observations separate from interpretation. An absence of evidence does not establish that a failure cannot occur.
- Summarise the evidence; place long logs, queries and stack traces later or in linked files. Preserve the exact error and relevant time window.
- For a completed fix, state what changed, what verification established, and any material remaining limit. Do not imply tests prove behaviour they did not exercise.

Example:

**Finding:** Report downloads are failing intermittently. The cause is not yet confirmed.

**Evidence:** The supplied trace contains a gateway timeout. This shows the request timed out; it does not establish why processing was slow.

**Next check:** Compare report processing time with the gateway timeout for the same request.

The example is illustrative, not evidence about a live incident.

## People and management decisions

Lead with the recommended action and its purpose. Give the observations supporting it and the main uncertainty. Separate observed behaviour from assumptions about intent or motivation.

For sensitive conversations, offer a short usable script followed by only the context needed to use it well. Do not turn a private analysis into an employee-facing judgement or present speculative motives as facts.

For meeting notes, separate decisions, actions and open questions. Preserve dissent that affects the decision. Do not convert an idea into an agreed action or invent an owner.
