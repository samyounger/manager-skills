# API contracts and observability

Read this when a refinement touches a backend execution point, HTTP boundary or error contract. Guidance reviewed on 15 October 2026. External HTTP sources were researched online; private team guidance was read from the local study checkout. Re-read the team's current files for each substantial refinement because contracts and standards evolve.

## Team sources and precedence

The [study coding-guidelines directory](https://github.com/UserTestingEnterprise/study/tree/main/docs/coding-guidelines) is the team source of truth. Apply repository instructions and current service contracts; distinguish a documented requirement from this skill's recommendation. HTTP protocol semantics remain authoritative. Surface a conflict instead of silently changing an established wire contract or treating an example as mandatory.

Known local source roots (verify availability and remotes):

- [Orders](https://github.com/usertesting/orders): `/Users/syounger/development/01-orders`.
- [Study](https://github.com/UserTestingEnterprise/study): `/Users/syounger/development/02-services/02-test-builder/study`.

Read the relevant documents, not the entire directory by default:

- [REST API standard](https://github.com/UserTestingEnterprise/study/blob/main/docs/coding-guidelines/ut-rest-api-standard.md): status semantics, JSON envelopes, naming, idempotency, versioning and OpenAPI.
- [Observability and logging](https://github.com/UserTestingEnterprise/study/blob/main/docs/coding-guidelines/observability-logging.md): tier, canonical schema, taxonomy, trace context, privacy and monitor quality. Read this when planning every runtime fix.
- [Backend design](https://github.com/UserTestingEnterprise/study/blob/main/docs/coding-guidelines/backend-code-design.md): controller/use-case/domain/adapter responsibilities and typed errors.
- [Persistence patterns](https://github.com/UserTestingEnterprise/study/blob/main/docs/coding-guidelines/persistence-patterns.md): database behaviour and transaction boundaries.
- [Kafka and async processing](https://github.com/UserTestingEnterprise/study/blob/main/docs/coding-guidelines/kafka-and-async-processing.md): consumers/jobs, retries, idempotency and event effects.
- [Backend change checklist](https://github.com/UserTestingEnterprise/study/blob/main/docs/coding-guidelines/backend-change-checklist.md): verification, consumer changes, deployment/infrastructure concerns and data safety when applicable.

If private GitHub access fails, use the verified local checkout and link the canonical source. State any material freshness/access limitation; never imply the live branch was read when it was not.

## Choose the operation outcome first

Inspect the actual success/failure boundary before proposing a code. Do not map a database exception directly to an HTTP status. Recovery that completed the requested operation can retain its documented success contract. In particular, an internally recovered uniqueness violation is not automatically a client conflict.

Protocol reference: [RFC 9110, HTTP semantics](https://www.rfc-editor.org/rfc/rfc9110.html#name-status-codes).

| Status | Decision guide |
| --- | --- |
| `200` | Completed success with a representation. |
| `201` | Resource created; identify it, commonly with `Location`. |
| `202` | Accepted but unfinished; define how completion is tracked. |
| `204` | Success without response content. |
| `400` | Invalid request; team default for input/validation errors. |
| `401` | Authentication missing/invalid; applicable authentication challenge required. |
| `403` | Request refused despite understood credentials/authority context. |
| `404` | Resource unavailable or intentionally undisclosed. |
| `409` | Operation blocked by current resource state. |
| `412` | A request-header precondition failed, e.g. `If-Match`. |
| `422` | Valid syntax, unprocessable instructions; preserve the team's `400` validation contract unless deliberately changing it. |
| `500` | Unexpected server failure prevents completion. |
| `502` | Gateway/proxy receives an invalid upstream response. |
| `503` | Temporary unavailability; consider `Retry-After`. |
| `504` | Gateway/proxy times out awaiting upstream. |

These are decision aids, not permission to change established endpoint behaviour. `Retry-After` is delay-seconds or an HTTP-date; do not fabricate a recovery estimate. See [RFC 9110 §10.2.3](https://www.rfc-editor.org/rfc/rfc9110.html#section-10.2.3).

For rate limiting, [RFC 6585 §4](https://www.rfc-editor.org/rfc/rfc6585.html#section-4) defines `429 Too Many Requests`; the body should explain the condition and may carry `Retry-After`.

In the ticket, specify caller action and whether the result is known to have committed. A timeout can leave an uncertain outcome. Recommend bounded retries only with suitable idempotency/deduplication or reconciliation; HTTP status alone does not make a write safe to repeat. Avoid blanket retries for deterministic validation failures or all `5xx` responses. Assess which layer owns recovery to avoid multiplied retries across hops.

## Response body: team contract first

The team REST standard recommends a JSON object containing `error.code`, `error.message` and optional `error.target`; it does not prove that every existing endpoint uses that envelope. JSON properties use camelCase. Trace the actual client, specialised exceptions, filter registration/precedence and serializer before describing the response. A generic fallback may never handle this failure. Retain an adequate existing contract for a focused bug fix; a standards migration needs its own necessity and compatibility rationale.

Illustrative design option for an API needing a new error contract, only when the update actually fails. Do not copy this into a bug ticket in place of a different, verified existing response:

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json
```

```json
{
  "error": {
    "code": "STUDY.SETTINGS.UPDATE_FAILED",
    "message": "Unable to save study settings.",
    "target": "studySettings"
  }
}
```

This code is illustrative, not an existing universal code. Public error codes and log taxonomy codes can legitimately differ; document their mapping rather than forcing a wire-contract change. Preserve existing codes when appropriate. Callers branch on the agreed machine code, not message wording. Explain actionable correction for expected failures; sanitise unexpected failures. Keep stack traces, SQL/index names, credentials and internal dependency details out of client bodies. The complete structured log is not the public error payload.

### Transaction and consumer scope

Verify whether the existing ORM transaction already rolls back database changes on a raised exception. Distinguish that local guarantee from external side effects: callbacks can publish before commit, and post-commit failures cannot undo the completed transaction. Preserve the real boundary rather than requiring a new rollback/compensation pattern or promising all events are atomic.

Check how the frontend currently displays an error, retains edits, consumes the body and retries. A sound general recommendation such as reconciling uncertain results is not automatically part of this bug. Require a new retry/reconciliation or RUM flow only if it is needed to resolve the reported issue or explicitly requested; otherwise state existing behaviour to preserve.

### RFC 9457 as an explicit option

[RFC 9457](https://www.rfc-editor.org/rfc/rfc9457.html) defines `application/problem+json`, with `type`, `title`, `status`, `detail`, `instance` and extension members. If `status` is included, it must match the HTTP status. A problem type identifies a stable category; `detail` explains the occurrence. Problem details must not expose implementation internals. The RFC replaces RFC 7807.

Do not silently replace the team's envelope with Problem Details for an isolated bug. Evaluate it for a new API or an explicitly requested standards change, with consumer, content-negotiation, versioning and schema decisions recorded. Do not label the existing `{ "error": ... }` envelope RFC 9457-compliant.

Document successful and known failure responses, content types, headers, schemas and examples in the service's supported OpenAPI version; see the [OpenAPI Responses Object](https://spec.openapis.org/oas/latest.html#responses-object). Update contract tests and consumers when behaviour changes; this does not require upgrading the project's OpenAPI version.

## Observability: use the repository standard and a payload example

Read the applicable repository observability document for the current schema, runtime injection, privacy rules and tier gates. The ticket should reference that document rather than reproduce its general checklist. Use the JSON example in [ticket-template.md](ticket-template.md), populated for the actual failure point, inside a native “Proposed logging payload” expand, collapsed by default. Keep the initial view to a short event summary and standards link.

- Explain classification only when useful: Tier 3 means a bug fix, Tier 2 a change to existing behaviour, and Tier 1 a new endpoint/feature. These are observability work categories, not incident severity. Classify the necessary fix, not an optional contract/client expansion.
- Inspect the actual logger and metadata injection before building the example. Use the correct deployed service, domain, layer and error taxonomy. Runtime-provided values are labelled placeholders; the example is not presented as a captured event. Do not assume that the raw emitted payload and Datadog’s parsed field names are identical.
- Specify the genuine failure that emits the event. Successful internal recovery must not be represented as application failure, and real unhandled errors must remain visible. Preserve existing public response codes; a log-only taxonomy code does not require a wire-contract change.
- Describe the bug-specific failure/monitor signal, and put its testable verification outcome only in the Acceptance Criteria custom field. Do not add a “Verification:” paragraph to the description. Keep general privacy/security, cardinality and trace-propagation requirements in the linked standard; follow them in the example without copying them into the ticket. Meet the relevant gates in the touched path without requiring a service-wide instrumentation rewrite.
