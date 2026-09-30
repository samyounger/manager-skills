# Delivery Reporting Sources & Lenses

Read the Engineering Manager role scope and Team operation for every report when they are available. Read the remaining lenses only for a cross-team, unusually complex, AI-enabled, or delivery-stability-sensitive epic. This reference improves the judgement behind the report; it is not content to reproduce in the report.

## Engineering Manager role scope

Use this source before deciding that an item requires Engineering Manager attention. It frames the role as owning the delivery system—portfolio coherence, capacity, standards, escalation, and meaningful exceptions—rather than personally coordinating every project or overriding accountable role owners. Put an item in Needs EM Attention only when it needs this system-level intervention; leave routine delivery execution, product decisions, technical design, quality evidence, release work, and incident response with their named owners.

- [Engineering Manager role scope](/Users/syounger/Documents/obsidian/work_usertesting/team-management/roles/engineering-manager.md)

## Team operation and role inputs

Use the Team operation role map to identify the accountable role that should supply a missing decision, artefact, evidence, or hand-off. Keep this separate from the Engineering Manager's intervention: a Project Lead retains the technical delivery path and day-to-day coordination; Product owns outcome, priority, scope, and acceptance; Design owns experience intent; QE owns risk-based quality strategy; Architecture and Security provide consultation for their triggers; and On-call owns immediate operational response. When the role map does not resolve the input owner, read its linked role-specific scope before writing `Input needed from`. Do not list roles that are merely informed or consulted.

- [Team operation](/Users/syounger/Documents/obsidian/work_usertesting/team-management/roles/team-operation.md)

## Report outcomes and forecast variance

The UK Government’s Project Delivery Functional Standard says portfolio performance reporting should cover achievement and forecast performance, including outcomes, milestones, and risks, with commentary that explains material variance. Apply this proportionately: use `Latest signal`, `Target risk`, and `Risk` to explain whether the epic is advancing its intended outcome or forecast, rather than summarising ticket movement. Do not invent benefits, costs, or dates where Jira does not record them.

- [Government Functional Standard GovS 002: Project Delivery, 2025](https://www.gov.uk/government/publications/project-delivery-functional-standard)

## Surface decision friction and dependencies

PMI’s 2026 Pulse finds that complex delivery commonly erodes value through decision friction, misalignment, and rework; high-performing organisations focus on outcomes, alignment, and learning rather than more activity tracking. For a complex epic, elevate only the decision, dependency, or ownership gap that materially changes the delivery path. Make the required EM action specific and leave routine coordination out of the report.

- [PMI Pulse of the Profession 2026: Driving Success in Complex Projects](https://www.pmi.org/learning/thought-leadership/driving-success-in-complex-projects)

## Balance throughput with delivery stability

Google Cloud’s 2025 DORA research reports that AI can improve delivery throughput and product performance while increasing stability risk when testing, version control, feedback loops, and other control systems are weak. For AI-enabled or high-change-volume delivery, treat the absence of a safety net, observability, test coverage, rollback plan, or feedback loop as a material risk only when current Jira evidence supports it. Do not equate more activity, output, or AI use with progress.

- [DORA 2025 State of AI-Assisted Software Development](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report)
