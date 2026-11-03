# Collation rules and output

## Evidence ledger

Keep a compact in-memory ledger: fetch timestamp, CLI version, endpoint/path, exact non-secret parameters, configured scope label/ID/hierarchy, period/bucket boundaries, units, populations, pagination, backend limits/hints, and missing/suppressed fields. Preserve safe failure codes for unavailable queries. Do not save actual query traces or account-specific validation logs in the shared skill.

Use the configured origin and actual source paths for evidence links. Link work items only to their verified actual source URLs. Do not invent a dashboard URL or hardcode an employer's Jira hostname.

## Aggregate compatible evidence

- Keep configured squads and containing groups separate. A group can include additional children; use the upstream group aggregate rather than treating the managed-squad sum as the full group. Never add parent and child counts or double-count shared deliverables.
- Align dates, units, populations and bucket semantics. Mark partial weeks/sprints. Current hierarchy and roster mappings do not establish historical membership.
- Sum counts only across disjoint, deduplicated populations and non-overlapping periods. Never average/sum medians or percentiles; request a period/group aggregate or underlying observations instead.
- Combine rates only with compatible denominators. Weekly contributor counts do not sum to monthly unique contributors. Distinguish percentage-point changes from relative changes; a zero baseline makes relative change undefined.
- Missing, null and suppressed values are unavailable, not zero. Preserve unknown/null units and avoid interpreting them as an established unit. Validate integration coverage before interpreting zero incidents or deployments.
- Allocation FTE is observed/inferred effort, not headcount, planned capacity or a commitment. Story points are local estimates, not cross-team productivity scores.
- Preserve linked-PR and other source exclusions. Company averages and shared-repository counts cannot establish team values without defensible attribution.
- AI/delivery correlations do not establish causality. Survey sample size and suppression limit generalisation. Do not rank people or infer individual performance from activity counts.

## Response format

Apply the caller's language/style preferences from private context:

1. Lead with the supported finding and any material coverage limitation.
2. Use one small comparison table with **Signal**, one column per configured squad, and **Interpretation/limit**. Show up to three useful comparable signals.
3. When requested or configured, add a separate group-throughput section for each configured group label, with actual child coverage. Explicitly describe its containing scope; never add it to child totals.
4. State one or two proposed next checks, separating inference from recorded facts.
5. Add compact provenance: exact dates/timezone, fetch timestamp, scope IDs, CLI version, endpoint names/parameters and material limitations. Include the query ledger only when detailed reproducibility is useful.

For inaccessible sources, continue useful independent queries, label the gap and do not invent substitutes. Inspect the live schema for a contract mismatch; do not silently change the CLI.

## Persistence and publication

Private installation configuration, team mappings, exclusions and dated verification records belong under `.agents`, linked from EM context. Never commit them in shared skill files or examples. This skill contains no metrics cache.

Keep live responses in memory and reuse them only within the authorised task. Do not persist raw people/metric/survey data or credentials in either the skill or CLI repository. Return in chat unless a saved report was requested. Follow destination guidance for an authorised file; vault writes require the Obsidian CLI and local conventions. Do not publish, modify work-management records or schedule collection from this skill alone.

Use the installed `developer-productivity`, `sprint-planner` and `delivery-manager` skills when the task calls for interpretation, planning or portfolio reporting. Jellyfish supports those workflows; it does not replace their authoritative sources.
