# Security policy

## Public repository boundary
This repository intentionally contains no private memories, clinical or personal data, OpenAI exports, production database files, real audit logs, secrets, hostnames, private IP addresses, access tokens, service configuration or internal filesystem paths.

## Deployment safeguards
- Prefer loopback-only listeners for local HTTP and MCP services.
- Use explicit authentication, authorization, TLS and network restrictions for any remote access.
- Do not expose SQLite files, database ports, dashboards or raw retrieval tools publicly.
- Validate input sizes, formats and provenance before ingestion.
- Apply least privilege to agents, filesystem permissions and service accounts.
- Avoid logging prompts or retrieved sensitive text.
- Pin dependencies and run vulnerability/secret scanning before deployment.
- Test dataset isolation and ensure users cannot retrieve each other's records.
- Do not publish real backups, hashes of confidential files or system logs.

## Disclosure
Report suspected security issues privately to the maintainer through GitHub rather than publishing exploit details or secrets in an issue.
