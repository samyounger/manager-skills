# Private configuration contract

## Discovery and ownership

The manager's EM context is the entry point. Follow its Jellyfish configuration link; otherwise look for `jellyfish-context.json` beside `em-context.md`. A caller may supply a different private configuration explicitly. Read an existing configuration before asking for values. Do not search unrelated vaults or credential files.

Private configuration, lookup metadata and installation-specific validation belong under the caller's `.agents` directory. Keep only this generic contract and procedures in the skill. Never put actual team names/IDs, employer URLs, user paths, account details, language/timezone preferences or live validation records into shared skill files or README indexes.

Paths in the configuration may be absolute or relative to that JSON file. Paths in EM context are relative to the EM context file. Require resolved existing paths before execution. Use subprocess argument arrays, not shell interpolation/eval. Only execute the configured CLI and runtime for the authorised query.

## JSON fields (schemaVersion 1)

| Field | Type | Meaning |
| --- | --- | --- |
| `schemaVersion` | integer | Configuration format version; currently 1. |
| `cli.cwd` | string | Working directory for runtime selection and CLI execution. |
| `cli.entryPoint` | string | Built CLI entry point. |
| `cli.runtimeCommand` | string array | Runtime executable and prefix arguments, such as a runtime-manager invocation. |
| `cli.envFile` | string, optional | Secret-provider mount/reference file passed through Node's `--env-file`; omit for an inherited environment. Never store its contents here. |
| `cli.docsDirectory` | string | Actual local CLI documentation directory. |
| `cli.baseUrl` | string, optional | Explicit authorised Jellyfish origin, passed as `JELLYFISH_API_BASE_URL`; otherwise preserve an inherited override or the CLI default. |
| `reporting.timezone` | string | IANA timezone for period selection. |
| `reporting.locale` | string | User's language/locale preference. |
| `reporting.defaultPeriod` | string | `last-completed-calendar-month` unless context says otherwise. |
| `reporting.includeGroupThroughput` | boolean | Whether an overview also queries configured containing groups. |
| `squads` | object array | Managed team scopes. |
| `groups` | object array | Containing scopes, including backlog organisational levels where appropriate. |
| `excludedScopes` | object array | Known false matches or excluded scopes, with reasons. |
| `verification` | object | Last verified timestamp/date, private validation reference and other durable provenance. No raw metrics. |

Each squad/group provides `name` (API lookup name), optional `label` (report label), integer `id`, integer `hierarchyLevel`, `hierarchyLevelName`, `active`, and optional verification date. Groups may include a dated `children` array with the same scope identity fields. Children describe coverage; they do not automatically become managed squads. Exclusions carry the identity and exclusion reason. Avoid mixed parent/child IDs in aggregate filters.

Validate required fields, unique IDs within each scope list, non-empty runtime arguments, positive integer IDs/levels and explicit timezone before querying. Missing optional groups are allowed; if group throughput is requested without a configured group, resolve it from private context or ask a targeted question. Do not invent an ID or choose a parent by similarity.

## Runtime and authentication

Read `cli.docsDirectory/agent-guide.md` and `cli-reference.md` when behaviour is unfamiliar. Check the runtime meets the installed CLI's requirements. A version-manager prefix applies only to that invocation: use `cli.runtimeCommand` on every call, with `cli.cwd`. Never hardcode a user's runtime installation path.

For offline `--version`, help and catalogue commands, omit `--env-file`; they need no secret-provider access. For live commands, place `--env-file` before the entry-point path if configured. Without an env file, inherit `JELLYFISH_API_TOKEN` through the environment. Do not print, validate by echoing, or save its value. Serialise access when the configured provider uses mounted/FIFO files; set process timeouts and output-buffer limits.

If a build/runtime/configuration path is missing, report the exact missing setup without installing, rebuilding or changing shell configuration unless authorised. A provider prompt may require the user to authorise access; do not bypass it.

| CLI exit | Action |
| --- | --- |
| 1 | Internal failure: report it; do not repeat indefinitely. |
| 2 | Inspect endpoint metadata and correct parameters/configuration. |
| 3 | Let the user authorise the configured provider or renew access; never request a token value. |
| 4 | Report the safe upstream error; narrow oversized queries or deliberately adjust page limits; respect rate limits. |
