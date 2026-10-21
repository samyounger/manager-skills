# Technical decisions

Use for proposals, RFCs, architecture decisions and technical options. Adapt these sections to the requested template and complexity; do not create empty sections.

## A proposal the reader can scan

Start with a short decision brief:

- **Recommendation:** What to change and the problem it solves.
- **Why:** The most important evidence or constraint.
- **Decision needed:** What approval or choice is required, by whom and when, if known. Include the main trade-off or unresolved risk.

Then provide the full reasoning in short sections:

1. **Problem and success:** Describe the current behaviour, its impact, and how success will be recognised. Use measured baselines only when supplied or verified.
2. **Proposed change:** Explain the behaviour before and after. State boundaries, dependencies, and consequential interface or data changes.
3. **Options:** Compare only credible alternatives on the criteria that affect the decision. Include retaining the current approach when viable. Explain why the recommendation wins.
4. **Delivery and risk:** Cover migration, compatibility, rollout, verification, and rollback when relevant. State unknowns and the evidence needed to resolve them.
5. **Next action:** Name the decision, owner, and date only if established. Mark proposed ownership or dates as proposed.

Keep implementation detail and supporting evidence after the decision brief. Do not hide a blocking risk in an appendix. Link to precise sources so readers can inspect the detail without reading it all immediately.

## Explain the technical part

- Introduce the purpose before the mechanism: what changes for users or operators, then how it works.
- Use one concrete example or a small before/after comparison when abstractions are hard to follow.
- Keep terminology consistent. Do not rename a component halfway through the document.
- Use a small diagram only if it clarifies a relationship or sequence. Explain its conclusion in text. Avoid using a diagram and a large table to repeat the same information.
- Distinguish a required constraint from a preference. State whether performance or capacity figures are measurements, estimates, or targets.

## Example: a proposal opening

**Recommendation:** Move report generation to a background job so users can continue working while a report is prepared.

**Why:** The current request waits for the whole report. The supplied logs show that some requests exceed the gateway timeout.

**Main trade-off:** Users will receive a job status instead of an immediate download. We must define retry behaviour to prevent duplicate work.

**Decision needed:** Agree the asynchronous user journey before estimating delivery. The queue design and completion notification remain open.

This example is illustrative. Do not reuse its claims, evidence or design as facts about the user's system.
