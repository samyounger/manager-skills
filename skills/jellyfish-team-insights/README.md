# Jellyfish Team Insights

Generic instructions for querying and collating Jellyfish evidence for configured teams and containing groups. Version **2.0.0**.

The skill discovers private setup through `.agents/em-context.md` and its linked Jellyfish configuration. Team names, IDs, hierarchy, installation paths, secret-provider references and user preferences stay in private `.agents` files. Credentials are supplied externally and never stored in the skill or configuration.

Examples:

- “Compare my teams' delivery metrics last month.”
- “Include our containing group's throughput without double-counting its squads.”
- “Summarise our investment allocations or AI adoption, with coverage limitations.”

[Instructions](SKILL.md) · [Configuration contract](references/configuration.md) · [Query recipes](references/query-recipes.md) · [Collation rules](references/collation-and-output.md) · [Sources and validation](references/sources.md)

## Add to AGENTS.md

```markdown
For Jellyfish evidence, use `jellyfish-team-insights`. Read EM context and its linked private Jellyfish configuration first. Resolve all identifiers and local setup at runtime; never copy private values into shared skills or example commands.
```

Keep one canonical skill under `skills/jellyfish-team-insights/` with discovery symlinks following the host repository's convention. The skill does not authorise metrics caching, publication, background collection or work-management edits.
