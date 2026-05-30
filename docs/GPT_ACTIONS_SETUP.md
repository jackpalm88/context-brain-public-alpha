# GPT Actions Setup (Public Alpha v1)

## Contract for GPT Actions

Use the public schema:

- openapi/context-brain-actions.public.openapi.yaml

Do not use custom_gpt_schema.json as the public setup contract.

## Supported v1 operation IDs (read-only)

- health_check
- query_memory
- search_raw_chunks
- list_hubs_json
- get_hub_json
- get_content_by_id

## Explicit exclusions

Public GPT Actions v1 excludes write/admin/decision/graph-mutation operations.

## Internal reference note

openapi.json remains an internal/generated reference and may contain non-public routes.

## Non-claims

- No claim that GPT Actions write tools are supported.
- No claim of full MCP/API parity in public alpha.
- No claim of production-readiness guarantee.

Use the public schema: openapi/context-brain-actions.public.openapi.yaml


## Public contract note

The public GPT Actions schema is curated and intentionally does not mirror the full generated openapi.json.
Operation IDs in the public schema are stable public action names, not FastAPI-generated internal operation IDs.
