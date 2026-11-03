# Sources and validation

## Generic implementation sources

Resolve the installed CLI through private configuration. Relevant files within that checkout are:

- `docs/agent-guide.md` — agent invocation and interpretation boundaries.
- `docs/cli-reference.md` — commands, output, exit codes and pagination.
- `src/modules/jellyfish/catalog/endpoints.json` — actual names, paths, request schemas and source scoping notes; inspect through `endpoints <name>`.
- `src/config/runtime.config.ts` — environment-only authentication and allowed origins.

Use actual installed files rather than assuming another version has an identical catalogue. Validate endpoint names and parameter types offline, then apply a narrow live query using private configuration. Do not embed the resulting scope identities or account-specific validation records here.

## Vendor references

- [Official Jellyfish client repository](https://github.com/Jellyfish-AI/jellyfish-mcp) — vendor API/client reference. Confirm its current contract against the installed CLI and authenticated schema.
- [1Password local environment mounts](https://www.1password.dev/environments/local-env-file) — optional mounted-file setup and concurrent-read limitation.

Use `schema` through the configured CLI for the current account contract. A schema path count does not equal the number of available operations; do not assume catalogue/account parity.

## Verification procedure

1. Validate skill frontmatter, relative links, shell/Python syntax and canonical discovery links.
2. Scan all shared skill files and their README index row for personal names, IDs, home paths, employer domains, timezone/locale preferences and private validation traces.
3. Confirm private configuration and verification files are ignored by Git, and the EM context link resolves to the intended configuration.
4. Have a fresh agent discover configuration through EM context, resolve team/group scopes and apply one narrow query without copying identifiers into shared instructions or printing raw responses.
5. Keep live validation provenance only in private context. Update the skill version for changes to the generic configuration contract.
