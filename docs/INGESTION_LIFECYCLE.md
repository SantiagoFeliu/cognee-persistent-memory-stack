# Ingestion lifecycle

This is a **reference workflow**, not a verbatim production script.

## States
```text
PENDING -> VALIDATED -> BACKED_UP -> INGESTING -> VERIFIED
    |          |            |            |
  REJECTED   REJECTED     FAILED        PARTIAL / FAILED

Identical, already-verified input -> SKIPPED
```

## Suggested procedure
1. **Approve:** verify that the source is permitted for memory ingestion.
2. **Validate:** check file type, size, encoding and required metadata.
3. **Fingerprint:** compute SHA-256 over original bytes.
4. **Check duplicate:** compare against the ingest registry and dataset scope.
5. **Back up:** create a timestamped, access-controlled copy.
6. **Ingest:** submit using the API supported by the pinned Cognee version.
7. **Verify:** retrieve a known non-sensitive marker from the intended dataset.
8. **Record:** write status, source ID, hash, dataset, timestamp and verification evidence.
9. **Recover:** verify again after service restart; do not equate service readiness with memory readiness.

## Example audit record (fictional)
```json
{
  "source_id": "synthetic-001",
  "dataset": "demo_dataset",
  "sha256": "<computed-at-runtime>",
  "status": "VERIFIED",
  "verification": "synthetic marker retrieved",
  "sensitive_data": false
}
```

## Failure policy
- **PASS:** approved input ingested, correct dataset verified and evidence recorded.
- **PARTIAL:** ingestion completed but retrieval, isolation or restart verification is incomplete.
- **FAIL:** integrity, ingestion or recovery checks failed.
- **SKIPPED:** identical approved input already processed under the same rules.

Never suppress failures or claim verified memory based solely on a successful write response.
