# Query recipes

## Worked example: query configured squads and groups

Set `JELLYFISH_CONTEXT_FILE` to the configuration discovered through EM context, not to a path embedded in the skill. The following executable example resolves private paths and scope IDs at runtime, queries exact monthly aggregates sequentially, and prints only retrieval status. Run it with Python 3; it does not read the token or display metric payloads.

```sh
python3 - "$JELLYFISH_CONTEXT_FILE" <<'PY'
import datetime, json, os, pathlib, subprocess, sys
from zoneinfo import ZoneInfo

config_path = pathlib.Path(sys.argv[1]).expanduser().resolve()
cfg = json.loads(config_path.read_text())
if cfg.get('schemaVersion') != 1:
    raise SystemExit('Unsupported private configuration version')
def local_path(value):
    path = pathlib.Path(value).expanduser()
    return path if path.is_absolute() else config_path.parent / path
cli = cfg['cli']
runtime = cli['runtimeCommand']
if not runtime or not all(isinstance(arg, str) and arg for arg in runtime):
    raise SystemExit('Invalid runtime configuration')
command = list(runtime)
if cli.get('envFile'):
    command.append('--env-file=' + str(local_path(cli['envFile'])))
command.append(str(local_path(cli['entryPoint'])))
env = os.environ.copy()
if cli.get('baseUrl'):
    env['JELLYFISH_API_BASE_URL'] = cli['baseUrl']
today = datetime.datetime.now(ZoneInfo(cfg['reporting']['timezone'])).date()
end = today.replace(day=1) - datetime.timedelta(days=1)
start = end.replace(day=1)
scopes = list(cfg['squads'])
if cfg['reporting'].get('includeGroupThroughput', False):
    scopes += cfg.get('groups', [])
for scope in scopes:
    scope_id = scope['id']
    if type(scope_id) is not int or scope_id <= 0:
        raise SystemExit('Invalid configured scope ID')
    params = {'team_id': [scope_id], 'start_date': str(start),
              'end_date': str(end), 'unit': 'month', 'series': False}
    try:
        result = subprocess.run(
            command + ['--retries', '0', 'get', 'team_metrics',
                       '--params', json.dumps(params)],
            cwd=local_path(cli['cwd']), env=env, text=True,
            stdout=subprocess.PIPE, stderr=subprocess.PIPE, timeout=45,
        )
    except (OSError, subprocess.TimeoutExpired):
        raise SystemExit('CLI startup failed or access timed out')
    if result.returncode:
        raise SystemExit('CLI failed; inspect its safe error code without exposing raw output')
    if len(result.stdout.encode()) > 20 * 1024 * 1024:
        raise SystemExit('Response too large; narrow query')
    response = json.loads(result.stdout)
    rows = response['data']
    if not isinstance(rows, list) or not rows:
        raise SystemExit('No comparable aggregate rows returned')
    if any(row.get('team_id') != scope_id or
           row.get('timeframe', {}).get('start') != str(start) or
           row.get('timeframe', {}).get('end') != str(end) for row in rows):
        raise SystemExit('Returned scope or period differs from requested aggregate')
    print(json.dumps({'status': 'ok', 'period': [str(start), str(end)],
                      'aggregateRows': len(rows), 'meta': response['meta']}))
PY
```

This is a retrieval smoke check, not the whole reporting workflow. Before reporting, verify the configured mappings as below. For analysis, retain responses in memory and extract the minimum requested metrics by slug; include scope labels/provenance only in the authorised report. For large responses, use streaming subprocess reads with an in-flight output cap rather than the example's post-capture guard. Never log a child traceback or raw error output that might contain private values.

Use requested dates instead of the example's default when the caller supplies a period. The runtime date and private timezone determine the default; no historic example dates are operational defaults.

## Recipe parameters

Construct JSON with a serializer, substituting values from private configuration and the requested period. Inspect `endpoints <name>` before unfamiliar fields. Never paste real configuration values into these instructions.

| Question | Endpoint | Parameters and scope |
| --- | --- | --- |
| Confirm names/IDs | `search_teams` | `name`: configured API names as a string array, or `team_id`: configured integer ID array. Filter exact active matches by ID, name and hierarchy; do not pick the first name match. |
| Confirm group coverage | `list_teams` | `hierarchy_level`: configured group's integer level; `include_children:true`. Select only the configured group ID and `child_teams`, discarding unrelated rows. This endpoint has no ID filter. |
| Month overview | `team_metrics` | One scope per call; `team_id:[scope.id]`, `start_date`, `end_date`, `unit:month`, `series:false`. Include configured groups when requested/defaulted. |
| Weekly trend | `team_metrics` | Same scope and dates, `unit:week`, `series:true`. Check boundary weeks; never sum overlapping buckets into a month. |
| Sprint evidence | `team_sprint_summary` | Scalar integer `team_id:scope.id`, plus `start_date`, `end_date`; validate sprint bounds against authoritative planning records. |
| Investment totals | `allocations_summary_by_investment_category` | `team_id:[str(scope.id)]` (strings), period, `unit:month`, `series:false`, `max_n_allocation_card_keys:0`. Keep hierarchy levels separate. |
| Deliverables | `search_deliverables` | `team:[scope.name]`, `timeframe_start`, `timeframe_end`, `limit:50`. Use `activity_status:[completed]` only for completed-work questions. Preserve team attribution and deduplicate stable IDs. |
| DevEx | `devex_insights_by_team` | Scalar integer `team_id:scope.id`, dates, `unit:month`. Preserve survey response counts and suppression. |
| AI adoption | `ai_team_adoption_analytics` | Integer array `team_id:[scope.id]`, dates, `unit:week`. Preserve cohort exclusions/counts. |
| AI outcomes | `ai_team_impact_analytics` | Integer array `team_id:[scope.id]`, dates, `unit:week`. Association does not prove causality. |

## Scope and completeness

A backlog organisational level is a configured containing group, not a count of waiting Jira issues. Query its upstream aggregate separately; do not add parent and child counts. Group coverage can exceed the managed squads, and current child mappings do not prove historical membership. If group access fails, label the gap; a deduplicated managed-squad subtotal must not be presented as the full group.

`team_metrics` returns scope/timeframe rows with `metrics` objects containing `name`, `slug`, `unit` and `value`. Display names can repeat; match by slug. Preserve returned units, including unknown/null units. PR-derived metrics omit PRs without a Jira ticket resolving to the scope.

`allocations_by_team` returns company-wide data at a level, so prefer filtered allocation summaries. `company_metrics` is not a team fallback. `unlinked_pull_requests` lacks a team-ID filter; shared repository membership does not prove ownership.

`search_deliverables` returns `items`, `limit` and optional `hint` within `data`. An item count reaching the limit can mean truncation even when CLI metadata says complete; narrow the query rather than inventing pagination. Automatic `--all` only supports `results`/`next`. Follow a meaningful empty-result hint rather than looping through arbitrary filters.
