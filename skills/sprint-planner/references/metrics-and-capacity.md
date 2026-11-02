# Capacity and hybrid delivery metrics

## Display rounding

Show every capacity figure without decimal places, rounded to the nearest whole number; halves round up. This applies to prose, table cells and rendered Dataview values: individual absences, hours/days, people-days, historical medians, percentages, gross forecasts and final ticket/point allowances. Show a capacity factor as a rounded percentage, not a decimal multiplier.

Retain exact source values and full precision internally. Apply the reserve to the unrounded forecast, then round the result; do not calculate using a displayed rounded median, percentage or subtotal. Calculate totals from exact inputs. Rounded individual rows may differ from rounded totals; explain this briefly instead of changing source data. Reconcile integer ticket categories to their rounded total using the largest-remainder method below.

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

## Obsidian Dataview presentation

When the local sprint-goal notes use DataviewJS, copy the matching team's named `individualContributors` and `teamLeads` roster from `.agents/em-context.md` into the block. Preserve roles, list individual contributors separately from team leads and partners, and set `days` from the current sprint's absence data. Count only individual-contributor absences in engineering capacity, while retaining lead and partner PTO for awareness. Keep the calculation inputs at the top of the block so the plan owner can refine them directly.

```javascript
const individualContributors = [
  // Copy the matching team's named roster from .agents/em-context.md.
  { person: "<name>", role: "Engineer", days: 0 },
];

const teamLeads = [
  // Copy the matching team's named roster from .agents/em-context.md.
  { person: "<name>", role: "EM", days: 0 },
];

const displayWhole = value => Math.round(value);
const engineers = individualContributors.length;
const workingDaysPerEngineer = <working days>;
const baselineDays = engineers * workingDaysPerEngineer;
const icPtoDays = individualContributors.reduce((sum, entry) => sum + entry.days, 0);
const leadPtoDays = teamLeads.reduce((sum, entry) => sum + entry.days, 0);
const availableDays = baselineDays - icPtoDays;
const availability = availableDays / baselineDays;

dv.header(2, "Team PTO");
dv.header(3, "Individual contributors — included in sprint capacity");
dv.table(["Person", "Role", "Days"], [
  ...individualContributors.map(entry => [entry.person, entry.role, displayWhole(entry.days)]),
  ["**Total**", "", `**${displayWhole(icPtoDays)}**`],
]);
dv.header(3, "Team leads and partners — excluded from sprint capacity");
dv.table(["Person", "Role", "Days"], [
  ...teamLeads.map(entry => [entry.person, entry.role, displayWhole(entry.days)]),
  ["**Total**", "", `**${displayWhole(leadPtoDays)}**`],
]);
dv.header(2, "Capacity plan");
dv.table(["Capacity measure", "Plan"], [
  ["Contributors", `${engineers} engineers`],
  ["Engineering baseline", `${engineers} engineers × ${displayWhole(workingDaysPerEngineer)} days = ${displayWhole(baselineDays)} engineering people-days`],
  ["IC PTO affecting capacity", `${displayWhole(icPtoDays)} days`],
  ["Engineering capacity", `${displayWhole(availableDays)} engineering people-days (${Math.round(availability * 100)}%)`],
  ["Protected contingency", "<percentage>% non-feature delivery"],
  ["Team lead and partner PTO", `${displayWhole(leadPtoDays)} days — tracked for awareness only`],
]);
```

Use this only in a fenced `dataviewjs` block. Calculate the parent-note total by adding the teams' available engineering people-days and PTO days; do not include team-lead or partner days in either total.

## Six-sprint forecast

Use the **six most recent completed sprints per team**, ordered by actual completion time. Exclude active/future sprints and paginate all issue results. Keep all six, including unusual sprints, and explain material changes in work mix or team size. If fewer exist, label the sample limitation.

| Sprint / actual dates | Working days | Historical contributors | Available people-days | Stories | Tasks | Bugs | Other | Total tickets | Completed SP |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |

Also retain committed-at-start measures, additions, unplanned completed work, spillover reasons and release outcomes when available. Never infer the original commitment from current sprint membership.

### Counting and historical capacity

- Prefer a completed-at-close report. Otherwise reconstruct from sprint membership and resolution instants between actual start and close; label the result as a proxy affected by later reopening, membership edits and re-estimation.
- Exclude subtasks and epic/initiative wrappers. Deduplicate issue keys within each sprint. Count Stories/User Stories, Tasks and Bugs separately; report Support Issues, Vulnerabilities and any other included top-level types as Other. State the mapping so total tickets equals the sum of categories.
- Sum points only on completed included tickets. Zero or absent points are not zero effort. Never award partial points or convert ticket counts to points.
- Reconstruct each sprint's contributor roster, working days and calendar losses. Apply part-time patterns, holidays and known non-delivery time; deduplicate overlapping absences. Exclude leads/partners unless an allocation is known. Do not copy today's headcount backwards or reuse another sprint's leave.
- Where records conflict or are missing, ask for the historical roster/pattern, document the chosen evidence and label estimates or sensitivity scenarios. Keep medical details out of the published plan.

### Calculate the planning allowances

For each team independently:

```text
capacity factor = new available people-days / median(historical available people-days over six sprints)
gross ticket estimate = median(completed total tickets over six sprints) × capacity factor
gross point estimate = median(completed story points over six sprints) × capacity factor
planned ticket allowance = round(gross ticket estimate × (1 − reserve fraction))
planned point allowance = round(gross point estimate × (1 − reserve fraction))
```

For an even sample, median is the average of the two central values. Retain full precision for the calculation; display medians, gross estimates, reserve percentage/people-days and final allowances rounded to the nearest whole number. Show final allowances and any further dependency-driven reduction. Treat these as estimates, not targets. A known QE bottleneck, material team change or larger work items can justify a lower commitment.

For each ticket category, calculate `category share = completed category tickets across six sprints / completed total tickets across six sprints`. Multiply shares by the planned ticket allowance. Floor each category and distribute the remaining whole tickets to the largest fractional remainders, using Stories, Tasks, Bugs, Other as the stable tie-break order. This reconciles Stories + Tasks + Bugs + Other to the total; independent category medians generally do not. The indicative mix may be changed for known demand while staying within total/point/capacity limits; explain a material change. Flag an exceptional bug-heavy sprint's effect on the mix.

Apply the reserve once. Planned bugs and maintenance count in the baseline allowance; urgent work added later uses the protected reserve. Do not double-count the same known repair as both planned scope and contingency. Keep pull work outside the baseline. A point allowance and a ticket allowance describe different constraints; do not add, average or compare point scales across teams.

### Required team-note presentation

Each team's Capacity planning section starts with **Summary for Atlassian**, following [the goal template](goal-template.md): a short copy-ready bullet summary of sprint dates, absent contributors and rounded days (or no recorded absence), delivery capacity after reserve, recommended stories/tasks/bugs/Other, total tickets and story points. Show only the necessary whole-number recommendations and one sentence about carry-over/verification. Ensure the category counts sum to the total. Keep the block self-contained and normally within 150 words.

Place **Supporting detail** beneath it. This contains the team's six-row history table, calendar/Dataview capacity inputs, assumptions and a compact forecast table:

| Metric | Six-sprint baseline | Gross availability-adjusted estimate | After reserve / planning allowance |
| --- | ---: | ---: | ---: |
| Stories | Six-sprint completed count/share | Indicative share of gross tickets | Reconciled whole-ticket estimate |
| Tasks | Six-sprint completed count/share | Indicative share of gross tickets | Reconciled whole-ticket estimate |
| Bugs | Six-sprint completed count/share | Indicative share of gross tickets | Reconciled whole-ticket estimate |
| Other included types | Six-sprint completed count/share | Indicative share of gross tickets | Reconciled whole-ticket estimate |
| Total tickets | Median completed total | Gross ticket estimate | Whole-ticket allowance |
| Story points | Median completed points | Gross point estimate | Whole-point allowance |

State historical median people-days, new available people-days, the capacity factor as a whole-number percentage, reserve and the counting method. Display all figures rounded to whole numbers; compute from the unrounded inputs. Separate implementation and verification days. Explain work-mix changes and unresolved assumptions. Retain original commitments, additions and completed-at-close measures for the next forecast.

## Delivery-confidence test

For every must-deliver item, give a confidence label grounded in evidence:

- **High:** scoped, start-ready, owner and verification path confirmed, no unmanaged external dependency.
- **Medium:** a known dependency, test/release gate, or limited discovery remains; include the date and owner for resolving it.
- **Low:** material unknown, blocked work, absent owner, insufficient capacity or an unconfirmed release need; do not present it as a normal commitment.

The suitable response to low confidence is a decision or scope reduction, not hiding the risk in a broad sprint goal.
