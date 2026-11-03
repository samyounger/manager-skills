---
name: jellyfish-team-insights
description: >-
  Queries and collates Jellyfish evidence for an engineering manager's configured
  teams and containing groups. Use for Jellyfish delivery metrics, group
  throughput, investment allocations, sprint evidence, DevEx, AI adoption or
  comparisons. Do not use for individual performance scoring, generic advice
  without a Jellyfish data request, or changing work-management records.
metadata:
  version: 2.0.0
---

# Jellyfish Team Insights

## Load private context

Read `.agents/em-context.md` first, using the caller's configured context location. If none is supplied, check the workspace and then the owning skill library's `.agents/em-context.md` (resolve the installed skill's real path before locating the library). Do not hardcode a user's home directory.

Read the Jellyfish configuration linked from EM context. If no link exists, check sibling `jellyfish-context.json`; otherwise use `em-context` to obtain only missing setup. Resolve relative paths against the context/configuration file, not the current directory. Follow [the configuration contract](references/configuration.md).

All team names, IDs, hierarchy, exclusions, local paths, secret-provider setup, timezone and report preferences come from private context. Never copy them into this skill, examples, validation logs or the shared index. Configuration contains references, never token values. Honour later user corrections.

## Establish scope and query

Use the requested period or derive the last completed calendar month in the configured timezone. Verify configured active IDs and hierarchy once per report; inspect child coverage for containing-group throughput. Do not substitute similarly named scopes or infer historical membership from today's hierarchy.

Read [query recipes](references/query-recipes.md). Start with separate monthly `team_metrics` calls for configured squads and requested/default groups. Use the configured runtime, working directory, CLI entry point and authentication method. The worked example loads every installation-specific value from private JSON.

Inspect offline `endpoints <name>` for unfamiliar parameters. Check child exit status before parsing `data` and `meta`; match metric `slug`, returned scope ID, timeframe and unit. Check backend limits/hints even when `complete:true`. Follow only supported pagination. Keep responses in memory and emit only the aggregates the question needs.

Never read or print credentials or mounted environment contents. On authentication failure, report the safe error and let the user authorise their configured secret provider. Do not request a token in chat or change shell/environment policy automatically.

## Collate and explain

Apply [collation rules](references/collation-and-output.md). Keep squad comparisons and containing-group throughput separate. Use the upstream group aggregate; never add it to child totals. Preserve linked-PR, integration-coverage and attribution limits. Do not average medians, combine rates without denominators, or treat observed allocation FTE as planned capacity.

Use the user's language and style preferences. Lead with the supported finding, show up to three signals, then evidence and limitations. Use configured labels for teams/groups. Record fetch timestamp, period/timezone, scope IDs, exact non-secret parameters and CLI version for reproducibility; distinguish facts, interpretation and proposed actions.

Return in chat unless saving was requested. Keep configuration and installation-specific verification under private `.agents` context. Do not cache metrics, rank people, infer AI causality, publish externally or edit Jira. For authorised vault writes, use the Obsidian CLI and destination guidance. Jira and approved planning evidence remain authoritative for commitments.
