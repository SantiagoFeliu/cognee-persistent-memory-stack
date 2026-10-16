# Cognee Persistent Memory Stack
### Cognee · Python · MCP · SQLite · Linux · Automation

**A local-first reference architecture for auditable persistent memory in AI-agent workflows.**

[Architecture](docs/ARCHITECTURE.md) · [Ingestion lifecycle](docs/INGESTION_LIFECYCLE.md) · [Validation checklist](docs/VALIDATION.md) · [Security](SECURITY.md) · [Earlier experiments](https://github.com/SantiagoFeliu/agent-memory-lab)

## Overview
AI agents need more than a vector search index: they need a repeatable way to ingest knowledge, retrieve it, track changes, detect duplicates and recover after restart.

This repository explains a Cognee-centered architecture developed after earlier experiments with MemPalace and Graphiti. It focuses on **system design, operational controls and safe local deployment**, not on publishing private memory or copying a production environment.

**Status: architecture documentation and synthetic educational examples.** This repository is not yet a turnkey installer or a certified production deployment. Runnable Cognee integration and compatibility tests should be added only after validating them against pinned upstream versions.

## Architecture
```text
Synthetic documents / approved input
                 |
        Validation + provenance
                 |
       Python ingestion controller
          /       |         \
      Backup    Digest    Duplicate check
          \       |         /
             Cognee API
                 |
       Local persistence layer
                 |
       Retrieval / verification
                 |
         MCP-facing adapter
                 |
            AI agents

Linux service lifecycle + audit logs + recovery checks
```

## Core engineering principles
1. **Explicit ingestion:** only approved sources enter the memory pipeline.
2. **Traceability:** record source identifiers, content hashes and ingest outcomes.
3. **Idempotency:** avoid unintentional reprocessing of identical inputs.
4. **Verification:** successful writes alone do not prove successful recall.
5. **Recoverability:** test retrieval again after a service restart.
6. **Isolation:** keep local memory services and databases private.
7. **Change control:** treat schema or dataset migrations as deliberate operations.

## Repository guide
| Resource | Purpose |
|---|---|
| [Architecture](docs/ARCHITECTURE.md) | Component roles and boundaries |
| [Ingestion lifecycle](docs/INGESTION_LIFECYCLE.md) | Approved → backed up → ingested → verified |
| [Validation](docs/VALIDATION.md) | What PASS / PARTIAL / FAIL should mean |
| [Synthetic dataset](examples/README.md) | Public-safe illustrative inputs |
| [Security](SECURITY.md) | Privacy, threat boundaries and disclosure |

## Stack and integration boundaries
- **Cognee:** knowledge processing and retrieval; confirm version-specific APIs before running examples.
- **Python:** orchestration and verification.
- **SQLite:** example local persistence/control concepts; exact Cognee backends vary by configuration.
- **MCP:** agent-facing memory interface; no public endpoint is provided.
- **Linux:** process lifecycle, backup and operational checks.
- **Node.js / n8n:** optional integration patterns; **not claimed as implemented in this public repository**.

## Why Cognee?
This architecture was preferred for the original project's requirements around ingestion discipline, auditability and operational recovery. It is **not** a claim of universal superiority or a measured head-to-head benchmark.

## License
MIT. Upstream projects and dependencies are separately licensed.
