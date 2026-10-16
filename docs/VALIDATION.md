# Validation plan

No tests in this repository have been run against a live Cognee installation. This checklist describes the tests required before claiming a reproducible release.

| Test | Expected evidence |
|---|---|
| Fresh installation | Pinned dependency list and recorded platform |
| Synthetic ingestion | Exact accepted count and API response |
| Exact re-ingestion | Documented SKIPPED or deduplication behavior |
| Recall | Known marker retrieved with provenance |
| Restart recovery | Marker retrieved after service restart |
| Dataset isolation | Query does not cross authorized dataset boundary |
| Malformed input | Rejected without leaking sensitive content |
| Concurrency | Deterministic behavior under overlapping requests |
| Backup and rollback | Restoration tested from a synthetic dataset |
| Secret scanning | No credentials or private records in files/history |

## Test reporting
Use **PASS**, **PARTIAL** or **FAIL** with evidence paths, timestamps, versions and reproducible commands. Never publish a PASS badge without corresponding results.

## Version policy
Pin and verify the actual Cognee, Python, database and MCP dependency versions when adding runnable code. Avoid assuming APIs are stable across major or minor releases.
