# Published parent and team goal-note template

Read the latest configured sprint goals before drafting. Their live structure is the primary style reference; this is a reusable shape, not a frozen copy of one organisation's content. Publish the result as Markdown notes that can be pasted into the configured goal-sharing tool.

## Titles, frontmatter and links

- Take human-readable goal titles, the sprint-folder pattern and note basenames from `.agents/em-context.md`. Store all notes for a sprint in its own subdirectory under the configured publication root, using lowercase hyphen-separated filenames (kebab-case). Do not reuse display titles as filenames.
- Keep `title` and H1 human-readable. Use sprint-qualified vault-relative wikilinks for repeated basenames, including the team note's `parent` field. After a requested move, check inbound links and preserve useful aliases.
- Follow the destination `AGENTS.md` for indentation. In a tab-indented workspace, use one literal tab per nested body-list level and check saved source; keep YAML and code indentation unchanged.
- Create a parent note and one note per team. Link them with Obsidian wikilinks when published in the same vault.
- Give every note concise frontmatter: `title`, `aliases` where useful, `sprint`, `period`, `type`, `status: ready-to-publish` and relevant tags. Team notes also name `team` and `parent`.
- Derive start and end dates from the configured sprint-boundary rule; verify the prior sprint's actual completion date.

## Parent note

Write this after reading both current team-goal notes. Summarise their actual objectives, section grouping and prioritised project order; preserve targets, uncertainty and material dependencies. Use short sentences and familiar wording. Do not invent scope or elevate conditional work into a commitment.

The body contains one short paragraph per non-empty source section, with a bold inline label and a single Markdown hard line break between sections. There are no blank lines between the section paragraphs. The backslashes at line ends below produce the required visible line breaks; two trailing spaces followed by a single newline are also valid.

```markdown
# <Product> sprint <number>

**Ready to release:** <Support/release outcome for [project](<Jira epic URL>), in team-plan order; retain unconfirmed release timing>.\
**In progress:** <[Project](<Jira epic URL>) outcomes in each team's prioritised order, with targets and dependencies>.\
**Planning / Upcoming:** <[Project](<Jira epic URL>) readiness or decision; keep conditional work conditional>.\
**<Active programme>:** <[Epic](<Jira epic URL>) milestones in source order; distinguish team contributions without duplicating shared scope>.\
**Maintenance:** <[Operational project](<Jira epic URL>), [bugs](<holding epic URL>) and [maintenance](<holding epic URL>); brief reserve statement if useful>.
```

Include **Discovery:** between Planning / Upcoming and programme work only when the team goals contain active discovery. Omit any empty parent section rather than adding N/A or placeholder prose. Every project mentioned must link to its Jira epic. If team-specific priority orders differ, use brief team-specific clauses rather than silently creating a new combined order.

Do not repeat the sprint date range in the body; the title/frontmatter supplies that context. Retain specific project deadlines where they affect the outcome. Keep detailed capacity figures, history, formulas and review checklists in the team/evidence notes. Preserve external goal URLs and sprint-qualified team/evidence wikilinks in frontmatter (for example, `goal`, `team_goals` and `planning_evidence`). Do not add extra body sections for those links.

## Team note

Keep project work concise and outcome-based. Use linked Jira epics or initiatives followed by a nested objective and exactly one engineering lead taken from that epic's Portfolio Status section. The objective states the meaningful sprint end state: a measured portion/milestone, release readiness or verification. Do not invent a percentage or confidence label.

```text
# <Team> - Sprint <number>

Parent: [[<sprint-qualified parent note path>|<Product> sprint <number>]]

## Features

### Ready to release

- <linked release/initiative>:
	- Objective: <release-ready or launch outcome>, by <date where known>.
	- Lead: <single engineering lead from the epic Portfolio Status>.

### In progress

- <linked initiative>:
	- Objective: <named milestone, completion or verification outcome>.
	- Lead: <single engineering lead from the epic Portfolio Status>.

### Planning / Upcoming
N/A

### Discovery
In no particular priority order:

- <linked opportunity or initiative>

## <Active cross-cutting programme>

Programme board: <inline link>.

- <workstream>:
	- <linked milestone>
		- Objective: <next verified outcome>.
		- Lead: <single engineering lead from the epic Portfolio Status>.

## Maintenance

Known planned bugs and maintenance consume the baseline ticket/point allowances. Reserve contingency for urgent additions and uncertainty; do not count the same work twice.

- <linked bugs holding epic>
	- Objective: <what is captured and how it will be handled>.
	- Lead: <single engineering lead from the epic Portfolio Status>.
- <linked maintenance holding epic>
	- Objective: <what is captured and how it will be handled>.
	- Lead: <single engineering lead from the epic Portfolio Status>.

## Capacity planning

- <Person>: **<whole days> days** absent — <dates and brief reason>. Repeat only for contributors with recorded absence; otherwise write "No recorded holidays or absences".
- Delivery capacity: **<whole people-days> people-days**, including **<whole QE days> QE days**, after the **<whole percentage>% reserve**.
- Recommended mix: **<stories> stories, <tasks> tasks, <bugs> bugs and <other> other tickets**. Omit Other when zero.
- Recommended total: **<total tickets> tickets and <story points> story points**.

Estimates use the previous six completed sprints and team availability. Confirm carry-over and verification effort before commitment.

### Supporting detail

<team calendar link>.

<Dataview capacity block, or a compact table when Dataview is unavailable>

<Six most recent completed sprints: dates, working days, historical contributor availability, completed stories/tasks/bugs/other, total tickets and points>

<Capacity-adjusted forecast table: indicative story/task/bug/other mix, total tickets and story points, gross estimates and final allowances after reserve; state assumptions and verification constraints>
```

### Formatting conventions

- The Summary for Atlassian is a self-contained, copy-ready block at the top of Capacity planning; aim for 150 words or fewer. Replace template instructions with actual values. List one absence bullet per affected contributor, with rounded days; omit absence detail when none is recorded. Keep Dataview, tables, formulas, internal wikilinks and extensive assumptions in Supporting detail. Recommendations must match the detailed forecast and the type counts must sum to total tickets.

- Display all capacity figures as whole numbers, rounded to the nearest integer (halves up), including prose, tables, percentages, historical medians, forecasts and rendered Dataview values. Preserve precise inputs and calculations; round only the output. Show capacity factors as whole-number percentages. Compute totals before rounding, with a brief note if rounded rows do not sum.

- Preserve the configured live section names. The `Features` lanes—**Ready to release**, **In progress**, **Planning / Upcoming** and **Discovery**—are the default.
- Use `Lead` (or `Project lead` where that is the configured convention) for exactly one engineering lead from the linked epic's Portfolio Status section. The value contains only that person's name. Product/design leads, QE owners, coordinators, implementers and dependency owners belong in the objective or dependency summary, not in this bullet. Do not infer the engineering lead from issue assignment.
- If Portfolio Status omits an engineering lead or names multiple engineering leads, flag the gap and seek clarification; omit the unresolved Lead bullet and keep the entry `needs-review` until one engineering lead is confirmed.
- An initiative can appear in multiple team notes only where each team has a distinct objective. The same epic retains its single Portfolio Status engineering lead in every note; record team-specific contributors separately. Do not duplicate the full programme commitment.
- Include a cross-cutting programme section only while it is active for that team. Preserve its board link and use nested bullets for workstream ownership.
- Include `Maintenance` whenever bugs or maintenance are planned. Verify holding links before copying them.
- Use real release dates, not “this sprint”. Name an absent lead as a risk; do not silently write `Unassigned` as a normal plan state.
- When **Planning / Upcoming** or **Discovery** has no work, write `N/A`; do not add explanatory prose or a placeholder project.
- Use a Dataview block for repeatable in-vault capacity calculations when the vault supports it. Otherwise use a compact Markdown table. In both forms, distinguish IC capacity from team-lead/partner PTO and show the protected contingency.
