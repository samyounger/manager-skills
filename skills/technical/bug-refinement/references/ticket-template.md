# Ticket template

Use this content model for an engineering bug. Replace illustrative placeholders with verified facts and remove irrelevant prompts. It is a menu of useful sections, not a requirement to invent work for every section. The Markdown below is an authoring template; compile it into native Jira ADF rather than pasting it as a plain-text description.

## Description

This field explains the problem, evidence and proposed approach. It contains no acceptance criteria, future verification checklist or “Verification:” paragraph, including inside expands. Put future pass/fail outcomes only in the Acceptance Criteria custom field.

### The issue

One short paragraph: trigger, expected behaviour, observed behaviour and customer consequence. Include a clickable link to the original reported error/incident, preserving the supplied URL; do not substitute a sample trace or a filtered search. Link service names to repository/service directories and format technical strings as code.

#### Severity and priority

**P-Severity:** selected Jira option and impact rationale; affected users/workflows; internal recovery versus client retry; data/event integrity; evidence limits. Distinguish a handled exception from a failed operation.

**Priority:** selected Jira option and urgency rationale, considering current exposure, frequency, workarounds and time sensitivity. Set both dedicated fields using [jira-triage-fields.md](jira-triage-fields.md); prose assessments do not substitute for field values. Keep severity and priority independent.

#### Timeline

**First confirmed:** date, environment and linked occurrence, or “Not established”.
**Suspected onset:** date and source, if different. **Code/deployment context:** only when established. State why retention, sampling or issue grouping limits the conclusion. Render human dates in en-GB; include timezone for exact occurrence times.

### Root cause

Explain the causal chain in a short paragraph: frontend/caller, server operation, defective behaviour, recovery/failure and outcome. State confidence. Reference the endpoint mapping bullets instead of repeatedly restating unlinked routes. A hypothesis remains labelled until verified.

**Native expand: “Evidence — traces, environment comparison and timeline”**

Include only evidence useful to an engineer:

- Linked trace/log occurrence with timestamp, environment, handling state and enclosing request/job outcome.
- A short code/SQL excerpt demonstrating the mechanism; remove customer content and secrets.
- Transaction commit/rollback, saved values or event observations that support the impact claim.
- Environment comparison with exact window, filter, retained versus complete counts and denominator limitations.
- First-seen search limits and deployment/code evidence; do not substitute a grouped error's date for the specific bug's onset.
- Reproduction status: executed, source-derived, or still a hypothesis.

**End expand.** Put the conclusion and recommended severity above it, not only here.

### Proposed solution

State the chosen root-cause change directly, with the source-verified behaviour and implementation boundaries it preserves. Use affirmative implementation steps, including the precise transaction and external-publication guarantees supported by the source. Keep testable acceptance/verification outcomes in the dedicated custom field.

#### Other options considered and dismissed

When relevant, use a concise numbered list of genuinely considered alternatives, each with its reason for dismissal:

1. **Alternative approach** — dismissed because of the specific evidence, risk or trade-off.
2. **Another approach** — dismissed because it leaves the root cause unresolved or adds unnecessary scope.

Omit this subsection when no relevant alternatives were considered. Keep “Do not” sections and prohibitive lists out of Proposed solution; express necessary constraints through positive preservation statements or the relevant dismissal reason.

#### Server response and failure handling

For an HTTP bug, describe the actual contract and only the changes needed. A small table helps when success and failure need comparison:

| Condition | HTTP outcome | Response and caller behaviour |
| --- | --- | --- |
| Operation succeeds, including internal recovery | Existing documented success status | Existing success representation; no error envelope or unnecessary retry. |
| Actual failure condition | Source-verified existing status, or justified proposed change | Existing sanitised body and caller handling, or a clearly labelled necessary change. |

Inspect the complete error-handler/filter chain, not only the generic filter. If an existing client-safe contract is adequate, retain it; use its actual JSON shape for any example. Identify necessary OpenAPI/consumer changes only when changing the contract. The logging schema and public response body are separate contracts. For non-HTTP bugs substitute the real job/event/UI contract.

#### Observability

Keep only a short event summary and the repository standards link visible. Put the example JSON log payload and all payload-specific explanation inside a native ADF expand titled “Proposed logging payload”, collapsed by default. Do not list fields inline or repeat general “avoid PII”, secret-handling, cardinality and propagation checklists: those belong in the repository standard. Apply those rules while constructing the example.

If naming a tier, explain it: “Bug-fix observability (Tier 3; not issue severity).” A behaviour/contract change may fall under Tier 2; use the current repository definition. Do not leave an unexplained tier number.

**Native expand: “Proposed logging payload”**

Illustrative shape only; adapt field names and injected metadata to the verified logger/runtime, and omit unavailable context rather than inventing values:

```json
{
  "timestamp": "<UTC ISO-8601 timestamp>",
  "level": "error",
  "message": "<safe description of the actual failure>",
  "service": "<deployed service name>",
  "env": "<environment>",
  "version": "<deployed version>",
  "event_name": "<feature.operation>",
  "event_category": "error",
  "product_domain": "<owning domain>",
  "layer": "<owning layer>",
  "trace_id": "<active trace ID>",
  "request_id": "<current request ID>",
  "study_id": "<available domain ID>",
  "error": {
    "code": "<stable log taxonomy code>",
    "message": "<safe error message>",
    "type": "<actual error type>",
    "kind": "<same error type>",
    "severity": "error"
  }
}
```

This is a proposed example, not an observed event. Use the runtime’s actual correlation representation: for example, the existing Ruby logger uses nested `dd.trace_id` / `dd.span_id`, while another runtime may expose `trace_id` directly. Show the JSON shape, not code that hardcodes dynamic IDs. Keep the public response contract separate from the log.

**End expand.** Outside the expand, state only when the event fires and link to the standards. Put the success/failure verification outcome in the Acceptance Criteria field, not here.

### References

Keep only references critical to assessing or implementing the fix, normally four to six bullets:

- **First bullet: original reported error/incident link**, matching the link in “The issue”. This remains essential even when the monitoring issue groups other errors.
- Root-cause implementation; combine equivalent affected updaters in one bullet.
- Each affected endpoint mapping: inline-code method/route; frontend call area → HTTP client → server handler. Include intermediate callers and state when no frontend exists.
- Transaction/event boundary or specialised error contract, when material to the proposal.
- The governing logging/API standard, when material.

Service names still link to their repository/directory in prose. Do not duplicate source links already present in a complete endpoint mapping. Keep sample traces, onset commits and supporting investigation sources in the Evidence expand; retain the original reported error link in both required locations. Add a test/schema/framework reference only if the reviewer needs it to understand the cause or safe fix; this is not a repository reading list.

## Steps to Reproduce — dedicated field

Use an ADF ordered list. Specify:

1. Preconditions: environment, feature exposure, permissions, lifecycle state and relevant existing/missing records.
2. Exact user/API action and request shape, with caller/handler source links for endpoints. Reuse a complete endpoint mapping when already present in the description.
3. Timing: ordinary sequential action, or the precise concurrency/failure injection required. Do not assume a race.
4. Observed versus expected HTTP/UI result and persisted/event outcome; how to locate the evidence.

Say if source-derived and unexecuted. Do not mutate live customer data just to reproduce.

## Acceptance Criteria — dedicated field

This is the only field for acceptance and verification requirements. When moving existing description-side requirements here, merge necessary unique outcomes with the current criteria and remove duplicates; do not simply append another list.

Use nested ADF bullets. Aim for three or four distinct conditions and one or two short outcomes per condition. These are illustrative; include only conditions relevant to the bug:

- When the reported trigger occurs:
  - It should produce the intended result and retain the agreed API response.
  - It should preserve the affected change/event semantics.
- When the relevant missing-record or concurrency edge case occurs:
  - It should create or reuse one valid record without surfacing a handled conflict as a failure.
- When the existing eligibility or omitted-input condition applies:
  - It should preserve the established restriction or unchanged state.
- When the operation genuinely fails:
  - It should preserve the source-verified transaction and API/UI error behaviour.
  - It should retain the required correlated failure logging without PII.

Nest a dependent condition only when it depends on its parent; do not nest “missing row” beneath “existing row”. Merge overlapping cases and state each outcome once. Keep implementation mechanisms, architecture caveats and detailed log-field schemas in the description; keep acceptance and verification outcomes exclusively in the Acceptance Criteria custom field. Do not add “Verification:”, acceptance checklists or completion tests to the description, even inside an expand. Observed evidence may report checks already performed, clearly separated from future requirements. Avoid test inventories. Do not add a “When the fix is verified” group that restates all other criteria. Retain enough precision to test the outcomes, and add a condition only for a material independent requirement.

## ADF construction notes

Jira descriptions/comments/custom fields use ADF documents, not Markdown strings. Field metadata still determines the supported value type and editability.

Native evidence fragment inside a document's `content` array:

```json
{
  "type": "expand",
  "attrs": { "title": "Evidence — traces and outcomes" },
  "content": [
    {
      "type": "paragraph",
      "content": [
        { "type": "text", "text": "The enclosing request returned " },
        { "type": "text", "text": "200 OK", "marks": [{ "type": "code" }] },
        { "type": "text", "text": "." }
      ]
    }
  ]
}
```

Use `heading` with `attrs.level` for sections; `codeBlock` with `attrs.language` for JSON/SQL; `link` marks with `attrs.href` for clickable text. Wrap each list item in `listItem` with a paragraph first. Put dependent `bulletList` nodes after that paragraph inside the parent item. Steps use `orderedList`, optionally with `attrs.order: 1`.

Wrap content in `{ "type": "doc", "version": 1, "content": [...] }`. Send native JSON field values, not stringified documents, unless the specific tool explicitly requires a serialised comment body. Read back raw ADF to confirm rendering structure.
