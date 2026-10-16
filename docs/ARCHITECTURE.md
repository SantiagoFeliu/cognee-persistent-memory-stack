# Architecture and responsibility boundaries

> Educational reference design. Not a copy of any private production system.

## Components
| Component | Responsibility |
|---|---|
| Source approval | Accept only intentional, non-sensitive documents |
| Python controller | Input checks, stable IDs, ingestion coordination |
| Backup | Preserve recoverable pre-ingestion copies |
| SHA-256 | Track exact bytes and detect repeat submissions |
| Cognee | Transform, store and retrieve knowledge through supported APIs |
| Persistence | Keep state across process restarts; backend depends on configuration |
| MCP adapter | Expose restricted, documented retrieval tools to agents |
| Linux service manager | Start, stop, restart and observe the service |
| Audit | Record accepted, skipped, failed and verified operations |

## Trust boundaries
```text
[untrusted input] --validate--> [ingestion controller]
                                     |
                              [local Cognee]
                                     |
                             [retrieval boundary]
                                     |
                               [authorized agent]
```

A local HTTP listener, if used, should bind to `127.0.0.1` unless access is explicitly protected. Do not assume that a local service is safe merely because it uses HTTP.

## SQL and Node.js
SQLite can be useful for local metadata and transactional controls. This documentation does not assert a particular internal Cognee schema. A Node.js client may invoke an authorized adapter, but no unverified Node.js integration is presented as working code.

## Operational caveats
- Hash equality proves byte identity, **not** semantic correctness.
- An ingestion acknowledgement proves submission, **not** successful recall.
- A retrieval hit proves an answer can be found, **not** that it is current.
- Dataset isolation, concurrent writes and schema migration require separate validation.
