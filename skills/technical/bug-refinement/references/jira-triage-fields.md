# RAD severity and priority fields

Verified on 15 October 2026 against the exact edit metadata for [RAD-90791](https://user-testing.atlassian.net/browse/RAD-90791) on `user-testing.atlassian.net` (cloud ID `9e7820fa-18c5-4e87-9db0-7611af19f569`). Both fields support `set` on that ticket. Use these cached mappings to avoid rediscovering the fields; validate their names, editability and allowed IDs against each target issue's `editmeta` before writing. Other projects or field contexts may differ.

## P-Severity — customer impact

- Display name: **P-Severity**.
- Field key: `customfield_10108` (custom single-select option).

| Option | Option ID |
| --- | --- |
| Blocker | `10128` |
| Major | `12640` |
| Minor | `12639` |
| Info | `12641` |

## Priority — delivery urgency

- Display name: **Priority**.
- Field key: `priority` (standard Jira priority field; no custom-field ID).

| Option | Priority ID |
| --- | --- |
| Immediate | `11003` |
| High | `2` |
| Medium | `3` |
| Low | `4` |
| N/A | `10000` |

## Choosing the values

The option names and IDs above come from Jira; the following is refinement guidance, not a verified organisational severity policy. Use an applicable team policy or explicit user direction first.

- Assess P-Severity from actual workflow impairment, scope, recoverability and data integrity. Blocker suits a critical workflow that cannot proceed without an acceptable workaround; Major suits substantial impairment; Minor suits limited impairment with a practical workaround; Info suits informational findings without demonstrated functional impairment. State evidence limits rather than claiming a monitoring exception proves customer failure.
- Assess Priority separately from current production exposure, recurrence, workaround quality, time sensitivity and competing delivery context. Immediate suits an active issue requiring immediate intervention; High suits prompt remediation; Medium suits normal planned work; Low suits deferrable work; N/A is appropriate only when priority genuinely does not apply. Do not use N/A as a substitute for assessment.
- Do not map Major automatically to High, infer an incident from event counts, or treat staging-only evidence as verified production impact. An internally recovered operation can have low customer-impact severity while still meriting work for operational cost.
- Honour explicit user-set values or instructions to preserve the fields. Otherwise an authorised refinement includes setting both after analysis. For a draft-only request, include proposed values and rationales without a Jira update.

## Read, write and verify

Include `customfield_10108` and `priority` in the initial issue read with `expand: "editmeta,names"`, alongside the description, reproduction and Acceptance Criteria fields. Confirm each field's `operations` includes `set` and the selected ID appears in its `allowedValues`. If either is unavailable, prepare the complete refinement and resolve the limitation before the coupled update; do not substitute Jira labels or change issue type.

Add the following native JSON option objects to the same `fields` payload as the ADF documents. This illustrates Major / High; choose each independently for the actual bug:

```json
{
  "customfield_10108": { "id": "12640" },
  "priority": { "id": "2" }
}
```

These select fields take option objects, not ADF, strings or label values. Rich-text fields still take ADF. Read both fields back after saving and verify the stored names and IDs. Keep the description's short impact and urgency rationales consistent with the saved values, and report the selections with the ticket link.
