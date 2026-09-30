# Engineering Portfolio Report Format

Use this format for the file. Keep fields on separate lines and leave a blank line between portfolio items.

```markdown
---
title: <Portfolio or Delivery Team> Engineering Portfolio Report – <D Month YYYY>
date: YYYY-MM-DD
tags:
  - delivery-management
  - portfolio-report
  - jira
aliases:
  - <Portfolio or Delivery Team> portfolio report
  - <Engineering Manager> portfolio report
status: current
---

# <Portfolio or Delivery Team> Engineering Portfolio Report – <D Month YYYY>

## Focus today

| Priority | Epic | EM action | By |
| --- | --- | --- | --- |
| 🔴 Now | <Epic summary> ([<KEY>](https://user-testing.atlassian.net/browse/<KEY>)) | <specific action the Engineering Manager can take> | <documented date or not recorded> |

## Needs EM Attention

### <Epic summary> ([<KEY>](https://user-testing.atlassian.net/browse/<KEY>))

Status: <emoji> <Jira workflow status>
Focus: <🔴 Now / 🟠 This week / 🟡 Watch>
Target: <date or not recorded>
Target risk: <🔴 Missed / 🟠 Approaching / ⚪ None / not recorded>
Confidence: <recorded value or not recorded>
Latest signal: <most material current evidence>
Signal freshness: <Today / This week / Stale / not recorded>
Risk: <the delivery or outcome risk>
EM action: <specific action the Engineering Manager can take>
Owner: <Engineering Manager>
Input needed from: <accountable role or not recorded>
By: <documented date or not recorded>

## On Track

### <Epic summary> ([<KEY>](https://user-testing.atlassian.net/browse/<KEY>))

Status: <emoji> <Jira workflow status>
Target: <date or not recorded>
Target risk: <🔴 Missed / 🟠 Approaching / ⚪ None / not recorded>
Confidence: <recorded value or not recorded>
Latest signal: <most material current evidence>
Signal freshness: <Today / This week / Stale / not recorded>

## Planning Watchlist

### <Epic summary> ([<KEY>](https://user-testing.atlassian.net/browse/<KEY>))

Status: <emoji> <Jira workflow status>
Phase: <recorded phase or planning state>
Start readiness: <ready / not ready and why>
Missing: <the one to three inputs preventing a sound start>
Signal freshness: <Today / This week / Stale / not recorded>
```

Include only one to three rows in **Focus today**, drawn from Needs EM Attention and ordered `🔴 Now`, `🟠 This week`, then `🟡 Watch`. If Needs EM Attention is empty, replace the table with `No EM action required today.`

## Status legend

Use the closest recorded Jira workflow status. Keep an unrecognised Jira status verbatim and choose the closest semantic emoji and number only when Jira has no mapped value. Sort On Track and Planning Watchlist in descending priority number; `0` is the completed state.

| Jira workflow state | Status prefix and priority |
| --- | --- |
| Cancelled / Won't do | `⚫ 7` |
| Blocked | `🔴 6` |
| Discovering / Discovery | `🔵 5` |
| Proposing / Proposal | `🟣 4` |
| Ready / To do / Planned | `⚪ 3` |
| Developing / In progress | `🟠 2` |
| Verifying / In review / Testing | `🟡 1` |
| Done / Complete / Released | `🟢 0` |

`🔴 6 Blocked` is a workflow status only. An item that needs management attention but remains in development is still, for example, `🟠 Developing`.

## Focus urgency

Use Focus only for items in Needs EM Attention. It is an action-urgency label, not a Jira workflow status.

| Focus label | Use when |
| --- | --- |
| `🔴 Now` | The Engineering Manager's decision, escalation, or unblock is currently holding up a material outcome or needs action before the next working week. |
| `🟠 This week` | The action is needed soon to protect a recorded milestone, capacity decision, or delivery sequence. |
| `🟡 Watch` | No immediate intervention is required, but an identified decision, dependency, or planning input needs active monitoring. |

Sort Needs EM Attention by this order, then by the earliest documented `By` date. Place `not recorded` deadlines after recorded deadlines of the same Focus label, and use material consequence as the final tie-breaker.

## Field guidance

- **Target:** Use an explicit Jira date or `not recorded`. Do not turn a sprint, a child-ticket date, or a hopeful sequence into an epic target.
- **Target risk:** Use `🔴 Missed` only when a recorded target has passed without completion; use `🟠 Approaching` only when a recorded target is near enough that the current evidence creates a credible delivery concern; use `⚪ None` when a recorded target has no such concern. Otherwise use `not recorded`.
- **Confidence:** Use the explicit confidence or date-confidence value recorded on the epic, including its Portfolio Status. Otherwise use `not recorded`; do not derive it from ticket activity.
- **Latest signal:** One compact, dated-in-context signal that changes the reader's understanding of progress, readiness, scope, or risk. Prefer a decision, validated outcome, completed milestone, newly observed dependency, or a meaningful absence of ownership.
- **Signal freshness:** Use `Today` for material evidence from the report date, `This week` for evidence from the preceding seven calendar days, and `Stale` when the newest material evidence is older. Use `not recorded` when it cannot be dated reliably.
- **Risk:** Name the consequence and its material cause, not a generic concern. Include this field only in Needs EM Attention.
- **EM action:** State the next action the Engineering Manager can take. `Owner` is the Engineering Manager for Needs EM Attention. `Input needed from` names an accountable role only when that role must provide a decision, artefact, evidence, or hand-off that enables the intervention; otherwise use `not recorded`. Do not make the Engineering Manager the owner of the other role's routine work. `By` must be a documented deadline or `not recorded`; do not invent one. If no action is needed, the item belongs outside Needs EM Attention.
- **Start readiness:** Be specific about the condition that makes the work not ready; avoid an unqualified `not ready`.
- **Missing:** List the few planning inputs that would change start readiness, such as confirmed capacity, an owner, a decision, a dependency agreement, a target, or a technical/design artefact.
- **Epic links:** Every epic occurrence, including Focus today rows and section headings, must use `<Epic summary> ([<KEY>](https://user-testing.atlassian.net/browse/<KEY>))`. Do not write a bare epic key.
