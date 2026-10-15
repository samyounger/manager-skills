---
name: bug-refinement
description: Investigate and refine engineering bugs into concise, evidence-backed Jira tickets with root cause, severity, reproduction steps, BDD acceptance criteria, API response contracts and observability requirements. Use for “refine this bug”, “investigate this error”, or “make this bug ticket implementation-ready”. Does not authorise implementing or deploying the fix.
metadata:
  version: 1.2.7
---

# Bug Refinement

Turn a reported failure into an engineer-readable ticket that explains the issue, its impact, the evidence, and the intended API and operational behaviour. Use en-GB spelling and dates.

## Before starting

Read `$HOME/.agents/manager-skills/.agents/em-context.md` when present; otherwise read `.agents/em-context.md` or `.agents/skills/em-context/SKILL.md` if available. Use established team ownership and priorities without inventing missing context.

Read the current ticket and applicable repository `AGENTS.md` files. Preserve user edits, references and separate custom fields. An instruction to investigate and update a named ticket authorises those updates; a request for a draft alone does not. An authorised ticket refinement includes setting P-Severity and Priority after analysis, unless the user explicitly asks to preserve them. A draft-only request includes recommendations without Jira writes. Do not implement a fix, alter live customer data, change assignment/status/type, or send messages without authorisation for that action. Assess customer-impact severity independently of delivery priority.

Use the available Jira workflow/tooling. In RAD, these rich-text field IDs are useful hints, not substitutes for field discovery:

- Description: `description`.
- Steps to Reproduce: `customfield_10330`.
- Acceptance Criteria: `customfield_10209`.

For RAD P-Severity and Priority, read [references/jira-triage-fields.md](references/jira-triage-fields.md). It contains the verified field keys, option IDs, write shape and selection guidance; these are dedicated fields, not entries in Jira `labels`. Request these fields alongside the rich-text fields in the same issue/edit-metadata read.

Read the exact issue's edit metadata and confirm each required field's name and `set` operation before writing. A visible field is not necessarily editable. If required fields are unavailable, prepare the complete refinement, then explain the specific limitation and ask how to proceed. Do not silently put reproduction steps in the description or change the issue type to bypass the limitation. Prefer one coupled update after resolving it.

## Investigation

1. Read the actual monitoring events and relevant source. Identify the affected service, frontend entry point, request/consumer path, database constraint and exception handler. Follow caller → endpoint → use case → persistence → response/event path across repositories as needed. Use the monitoring integration's own investigation guidance when available.
2. Establish the outcome: inspect the enclosing HTTP response or job result, recovery queries, transaction commit/rollback and downstream effects. An error span or exception name alone does not prove the operation failed. Check persistence and emitted events where evidence permits.
3. Test the suspected explanation against alternatives. Ordinary sequential saves may trigger an insert-first helper; do not label every duplicate-key event a race. Separate internal recovery, safe client retry, deterministic failure, partial writes, data corruption and downstream inconsistency. A retry is safe only when side effects and idempotency support it.
4. Compare environments using the same event definition and time window. Distinguish occurrence counts, retained/sampled spans, distinct affected entities and request failure rates. Do not use samples as complete counts or divide differently sampled populations into a customer failure rate. Treat feature exposure, deployments and test activity as hypotheses until verified.
5. Establish onset carefully: distinguish first confirmed occurrence, earliest retained result, code introduction, deployment and suspected onset. Check grouping: one monitoring issue can combine unrelated tables or callers. State search window, retention/sampling limits and confidence. Never promote the user's suspected date to a confirmed start date.
6. Recommend the smallest sound root-cause fix. Explain trade-offs where a workaround leaves the underlying design intact. Preserve transaction, missing-row, concurrency, permission, draft-only, omitted-field and event semantics relevant to the path. Propose focused regression coverage; distinguish source-derived reproduction from an executed reproduction.

Report severity from customer impact, recoverability, scope and data integrity. A successful internally recovered operation normally requires no client retry; operational noise and database cost can still justify a fix. Explicitly state what remains unverified.

## Set severity and priority after analysis

After completing the investigation, root-cause assessment and proposed solution, choose the target ticket's P-Severity and Priority using the evidence and [RAD field reference](references/jira-triage-fields.md). Record a short rationale for each in the description. Set both dedicated fields with the completed refinement in one coupled update; recommendations in prose alone do not complete an authorised refinement. Respect explicit user-selected values or instructions to preserve them, and do not infer priority mechanically from severity or monitoring event volume. State uncertainty when evidence is incomplete.

## Verify the proposal against the implementation

Before writing the proposed solution or acceptance criteria, trace the actual code path and supporting tests. Distinguish **existing behaviour to preserve**, **the change needed to fix the bug**, and **optional improvements outside this fix**. Do not turn template prompts or team-wide aspirations into new requirements.

- Follow specialised exception handlers, registered filters and their precedence through to the response serializer; a generic catch-all may not handle this request. Verify the actual wire body and consumer handling before proposing a different envelope or code. Public response codes and log taxonomy codes can differ.
- Check transaction boundaries, callbacks and event publication. Existing ORM transactions may already provide database rollback on an exception. This does not imply distributed rollback, compensation or reversal of external side effects; `after_save` is not `after_commit`. State the precise guarantee supported by the code.
- Check dirty tracking, return values and event conditions before recommending a different persistence helper. An upsert can remove SQL errors while inadvertently bypassing change-based events.
- Inspect existing frontend error display, retained form state, retries and telemetry. Preserve these unless changing them is necessary for the bug or explicitly requested. Do not add a reconciliation flow, RUM retrofit or response migration solely to complete the template.

If source access is unavailable, mark dependent requirements as unverified. Never describe a new mechanism as existing or make an unsupported architecture assumption an acceptance criterion.

## Team standards and API-first refinement

Read [references/api-and-observability.md](references/api-and-observability.md) when the bug involves a backend, API, job or error contract. It routes to the current team guidelines and researched HTTP sources. Read current repository standards rather than treating this skill's summary as a permanent snapshot.

For HTTP bugs, record the source-verified success/failure contract: status, safe JSON body, stable machine code, relevant headers, retry semantics, transaction outcome and frontend behaviour. Propose contract changes only when needed or requested, with consumer/OpenAPI compatibility explicit. Do not invent an HTTP response for a backend-only consumer or UI-only bug; define its actual failure/retry contract instead.

Plan observability with the fix: explain the relevant failure event and place the concrete JSON logging payload and its explanatory notes inside a native expand titled “Proposed logging payload”, collapsed by default. Keep only the short event summary and standards link visible on initial glance; do not show the payload or an inline inventory of fields in the main narrative. Link the repository observability standard for general schema, privacy and operational rules instead of repeating its checklist in the ticket. Explain any tier label in plain language: Tier 3 means bug-fix observability, not issue severity. Retain the applicable standard’s requirements without expanding the scope, and do not log successful recovery as failure.

## Ticket structure and formatting

Use [references/ticket-template.md](references/ticket-template.md) for the description, reproduction and acceptance criteria template and ADF examples. Keep the primary narrative concise; put detailed trace excerpts, comparisons and timeline evidence in a native expandable Evidence block. Do not bury the severity or conclusion inside the expand.

Write Proposed solution as a clear, affirmative description of the chosen implementation and the existing behaviour it preserves. Put genuinely considered and rejected alternatives in a subsection titled “Other options considered and dismissed”, using a concise numbered list with one reason for dismissal per option. Include this subsection only when there are relevant alternatives. Keep “Do not” sections and prohibitive lists out of the authored solution; express necessary constraints as positive preservation statements or explain the dismissed alternative and its trade-off.

- Link every service/MFE name in prose to its actual GitHub repository and service directory. Resolve aliases from source/deployment metadata; do not attribute a log to another service. Inside a code block, preserve literal code rather than attempting embedded links.
- Format code identifiers, filenames/paths, payload fields, SQL, error codes, headers, HTTP status codes and endpoint signatures as inline code; use fenced code blocks for multiline examples. In Jira use ADF `code` marks and `codeBlock` nodes, not literal backticks.
- Whenever mentioning an endpoint, include links to the frontend call area and server handler. For a BFF hop, show the originating frontend and intermediate HTTP caller as well as the destination handler. A compact bullet per endpoint can own the full mapping; refer to that mapping elsewhere. For a truly backend-only endpoint, state the actual caller and that no frontend exists.
- Link the original reported error/incident in “The issue” and make that same source the first References bullet. Preserve the user-supplied URL; sampled traces, filtered searches and source-code links do not replace it. Keep it even if its monitoring group contains unrelated errors, with the grouping caveat in Evidence. If no original source URL was supplied or found, do not invent one.
- References are critical-source bullets, normally four to six for a routine bug, never an inline row of links or an exhaustive investigation bibliography. Combine closely related updaters or transaction/callback sources in one bullet; remove duplicate frontend, schema, test and framework links unless essential to assessing the fix. Keep full endpoint mappings. Put detailed trace/timeline evidence in the expand rather than repeating it in References; the required original error link is the exception. Use verified source permalinks with line anchors where practical; mark mutable branch links when material. Link named Jira tickets, PRs and commits to their website pages.
- Use native ADF headings, `bulletList`/`listItem`, `orderedList`, `expand`, links, inline code and code blocks. Do not paste Markdown or HTML into ADF text. Preserve existing node IDs when retained and never invent opaque IDs.
- Reproduction belongs in the editable Steps to Reproduce field as an ordered list. Acceptance criteria belong in their dedicated field as nested bullets: conditions start “When …:” and outcomes start “It should …”. Nest dependent conditions under their parent. Aim for three or four distinct conditions with one or two outcomes each; add more only for a material independent requirement. State each outcome once. Keep implementation explanation in Proposed solution. Acceptance outcomes and verification requirements belong only in the dedicated Acceptance Criteria field (`customfield_10209` in RAD, confirmed against metadata), never under “Verification:”, “Done when”, a checklist or a second acceptance section in the description. Do not add regression-test inventories to either field; retain focused, testable outcomes without a generic “When the fix is verified” group.

## Field ownership

- **Description:** issue, severity, timeline, source-backed root cause, proposed implementation approach, concise observability summary, collapsed evidence/log examples and critical references.
- **P-Severity and Priority:** dedicated Jira fields holding the selected impact and urgency values; use option objects, with their separate rationales in the description.
- **Steps to Reproduce:** the dedicated editable custom field; use an ordered list.
- **Acceptance Criteria:** the dedicated editable custom field; use concise nested BDD bullets. This is the sole home for future pass/fail outcomes and verification requirements, including observability outcomes.

When cleaning an existing ticket, check each description-side acceptance or verification statement against the custom field. If it is already covered, delete the duplicate. If it adds a necessary distinct outcome, merge that outcome into the custom field before removing it from the description; avoid expanding the short criteria list unnecessarily. Do not discard a unique requirement when its target field is unavailable. Prepare the coupled edit and resolve field availability first.

Do not disguise acceptance criteria as “Verification:”, “Validation”, “Testing”, “Done when”, a checklist, or prose inside an expand. Factual results from checks already performed remain valid evidence when clearly labelled as observations. The proposed solution may explain how the fix works; it must not repeat the custom field’s completion checklist.

## Save and verify

Before saving, remove acceptance/verification checklists from the description and ensure every necessary unique outcome is covered once in the Acceptance Criteria custom field. Keep the description to the issue, evidence, causal explanation and proposed approach; factual observed results and reproduction status are evidence, not new acceptance requirements. Also remove repeated outcomes, references already supplied by an endpoint mapping, unnecessary new mechanisms and claims unsupported by source. Preserve important customer/contract guarantees when shortening; omit irrelevant template sections rather than filling them mechanically.

For an authorised update, write the completed rich-text fields together with P-Severity and Priority, then read them back. Confirm both triage fields store the intended option IDs and names. Read the rich-text fields back in ADF. Verify the stored headings, expand, links, code marks, reference bullets, ordered steps and nested criteria, alongside the actual text. Some connectors flatten ADF in structured results while retaining raw ADF in their text response; inspect the raw representation before assuming formatting was lost. Ignore Jira-generated `localId` values when comparing semantic structure.

If a write fails, use its schema/error to make a bounded correction; do not cycle formats blindly or overwrite unrelated fields. If an expand is unsupported, explain the limitation before substituting a different layout.

## Independent post-save review — required

After writing the ticket and reading back its stored fields, spawn a sub-agent to review the saved ticket against this skill before reporting completion. Repeat this review after subsequent authorised ticket edits; respect the user's requested edit scope. A draft-only task has no saved ticket to review.

Give the reviewer the ticket URL, this skill's absolute path, relevant references, the user's instructions and any explicit scope or field-preservation exceptions. Prefer a fresh context (`fork_turns: "none"` where supported), supplying the original report and evidence links needed for the review rather than the author's conclusions or a pre-filled verdict. The reviewer reads the skill and its applicable references, then independently retrieves the current ticket fields in raw ADF. A local draft or the writer's successful tool response is insufficient evidence of the saved ticket.

The reviewer checks the applicable requirements across the whole ticket:

- Evidence supports the impact, timeline, root cause and proposed approach; hypotheses and unverified outcomes remain labelled, and service attribution, endpoint mappings and source links are accurate.
- Proposed solution states the chosen approach clearly; dismissed alternatives, when relevant, form a concise numbered list with reasons.
- Reproduction steps and concise, non-duplicated BDD Acceptance Criteria occupy their dedicated fields; requirements appear once, and necessary outcomes have not been lost during shortening.
- P-Severity and Priority match the evidence, separate rationales and permitted option values, subject to explicit user exceptions.
- Native headings, lists, code marks, links and collapsed Evidence/logging expands follow the skill, with no unsupported contract changes or unnecessary scope.

Keep the review read-only. Ask for a pass/fail verdict, concise actionable findings with exact field/section and supporting evidence, and any checks that could not be completed. An unavailable source is an explicit review limitation, not proof that a claim is false or verified.

Evaluate the findings, make justified corrections within the authorised scope, read the changed fields back, and ask the same reviewer to recheck the corrections against the live ticket. Resolve material findings before finishing. If a correction requires unavailable access or additional authorisation, complete unaffected work and report the specific outstanding finding; do not silently expand scope or claim a clean review. If sub-agents are unavailable, disclose that the independent review could not run rather than substituting a self-review and calling it independent.

Close with the ticket link, important finding, material uncertainty and independent review outcome; do not repeat the whole ticket.
