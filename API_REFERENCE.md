# API REFERENCE (Public Alpha)

## Public GPT Actions v1 (read-only)

Approved operation IDs:
- health_check
- query_memory
- search_raw_chunks
- list_hubs_json
- get_hub_json
- get_content_by_id

## Excluded from public GPT Actions v1

Write/admin/decision/graph-mutation operations are excluded from public v1.

## Schema boundary

- Public GPT Actions target schema: openapi/context-brain-actions.public.openapi.yaml
- Internal/generated reference: openapi.json

## Non-claims

- This document does not claim full API stability across all endpoints.


## Public contract note

The public GPT Actions schema is curated and intentionally does not mirror the full generated openapi.json.
Operation IDs in the public schema are stable public action names, not FastAPI-generated internal operation IDs.

No claim of full MCP/API parity in public alpha.
