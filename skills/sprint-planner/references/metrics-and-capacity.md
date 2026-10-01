# Capacity and hybrid delivery metrics

## Capacity worksheet

Use one worksheet per team.

| Contributor | Role / planned delivery contribution | Scheduled workdays | PTO / public holiday | Other non-delivery time | Available days |
|---|---:|---:|---:|---:|---:|
| Name | Developer / QE / lead / shared specialist |  |  |  |  |

Then calculate:

| Measure | Formula |
|---|---|
| Baseline people-days | Sum scheduled workdays for people actually contributing delivery capacity |
| Lost people-days | PTO + public holidays + known non-delivery commitments |
| Available people-days | Baseline − lost people-days |
| Availability | Available / baseline |
| Protected contingency | A visible percentage or people-day allowance, based on recent unplanned work and risk |
| Commitable capacity | Available − protected contingency |

Account for part-time patterns, travel, company ceremonies, support rotations, onboarding, interviews, cross-team help and planned release/QE work when they materially displace delivery. Do not count calendar entries as leave until their attendee/status/date make that interpretation credible; flag ambiguous events for confirmation.

## Hybrid forecast

Story points cover only pointed work; ticket throughput captures bugs, technical debt, migrations and other unpointed work. Track both per team and use neither as a performance target.

For each of the most recent three to five completed sprints, collect:

| Sprint | Available people-days | Committed SP at start | Completed SP at close | Committed tickets at start | Completed tickets at close | Added after start | Unplanned completed | Spillover | Work-type mix | Release outcome |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---|---|

Rules:

- Count a story point only when its issue was completed by sprint close. Do not award partial points.
- Count completed tickets across the agreed issue types, but keep sub-tasks and duplicate/administrative items consistently included or excluded. State the rule.
- Preserve the original committed baseline. Report scope added after the sprint began separately rather than treating it as planned delivery.
- Use a trailing median, not a single best sprint. Exclude or annotate an abnormal sprint only with a recorded reason (team change, extended incident, unusual leave, or an exceptional release).
- Adjust for availability when it differs meaningfully. Compare points per available people-day and tickets per available people-day as a diagnostic, not an incentive.
- Segment by work type: release/feature, bugs, maintenance/tech debt, migration, discovery and support. A stable overall ticket count can conceal an unsustainable change in mix.

Suggested planning table:

| Team | Availability | Trailing median SP | Availability-adjusted SP forecast | Trailing median tickets | Availability-adjusted ticket forecast | Protected contingency | Must-deliver commitment | Pull items |
|---|---:|---:|---:|---:|---:|---:|---|---|

If points and ticket throughput point in different directions, do not average them. Explain the work-mix change and size the commitment to the constrained measure. Do not compare point totals between teams: point scales are local.

## Delivery-confidence test

For every must-deliver item, give a confidence label grounded in evidence:

- **High:** scoped, start-ready, owner and verification path confirmed, no unmanaged external dependency.
- **Medium:** a known dependency, test/release gate, or limited discovery remains; include the date and owner for resolving it.
- **Low:** material unknown, blocked work, absent owner, insufficient capacity or an unconfirmed release need; do not present it as a normal commitment.

The suitable response to low confidence is a decision or scope reduction, not hiding the risk in a broad sprint goal.

