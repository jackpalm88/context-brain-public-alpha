# Context Brain MCP Tools

## Stable Tools (12)

- `query_memory` — Search Context Brain semantic memory and return a grounded answer with citations.
- `search_raw_chunks` — Retrieve raw knowledge chunks with semantic distance scores and graph/hub-aware retrieval signals.
- `health_check` — Check whether Context Brain backend is online and responsive.
- `list_hubs` — List all knowledge hubs (user-defined or AI-suggested navigation entry points).
- `get_hub` — Get details for a specific knowledge hub including aliases, related terms, and linked content.
- `create_hub` — Create a new knowledge hub (navigation entry point).
- `update_hub` — Update an existing knowledge hub. Only provided fields are changed.
- `link_content_to_hub` — Link a stored content item (by content_id) to a knowledge hub.
- `save_and_link_to_hub` — Save content to Context Brain and immediately link it to a hub in one call.
- `search_graph_preview` — Lightweight metadata-first search; no full chunk text returned.
- `load_graph_node_full` — Load full content text plus all Phase 4/5 metadata for a specific node.
- `get_content_by_id` — Retrieve full content text for a known content_id.

## Experimental Tools (20)

- `list_hubs_json` — List all knowledge hubs as structured JSON with count; output format may change before v1.1.
- `get_hub_json` — Get a knowledge hub as structured JSON with linked content and stats; output format may change before v1.1.
- `get_graph_snapshot` — Get full graph snapshot: all hub nodes plus curated edges; output format may change before v1.1.
- `list_graph_snapshot` — Alias for get_graph_snapshot; output format may change before v1.1.
- `list_hub_edges` — List persisted hub-to-hub edges; output format may change before v1.1.
- `create_hub_edge` — Create a manual hub-to-hub edge in the knowledge graph; output format may change before v1.1.
- `update_hub_edge` — Update an existing hub edge; output format may change before v1.1.
- `archive_hub_edge` — Archive a hub edge; output format may change before v1.1.
- `approve_inferred_edge` — Approve an inferred hub-to-hub edge and persist it to the backend; output format may change before v1.1.
- `reject_inferred_edge` — Reject an inferred hub-to-hub edge and persist the rejection to the backend; output format may change before v1.1.
- `create_decision_memory` — Save a decision with its reasoning, context, and supporting references; output format may change before v1.1.
- `explain_decision` — Retrieve full decision record including reasoning, context, and why it matters; output format may change before v1.1.
- `list_decisions` — List decisions, optionally filtered by status or hub; output format may change before v1.1.
- `update_decision_status` — Update a decision's lifecycle status; output format may change before v1.1.
- `get_decision_lineage` — Get the full reasoning chain for a decision; output format may change before v1.1.
- `list_decision_conflicts` — Find active decisions that may conflict with each other; output format may change before v1.1.
- `get_decision_timeline` — Get chronological decision history grouped by status; output format may change before v1.1.
- `set_quick_summary` — Write a short human-readable summary to a stored content item; output format may change before v1.1.
- `classify_content_node` — Assign a semantic node type to a stored content item; output format may change before v1.1.
- `update_node_metadata` — Retrieve lightweight metadata for a stored content item; output format may change before v1.1.

## Internal (decorator removed, Python-only) (4)

- `_governance_lines` — not accessible via MCP protocol; Python-internal only.
- `list_escalations` — not accessible via MCP protocol; Python-internal only.
- `approve_escalation` — not accessible via MCP protocol; Python-internal only.
- `reject_escalation` — not accessible via MCP protocol; Python-internal only.

## Upgrade path

`save_memory (old)` -> `save_and_link_to_hub (current stable)`

## Stability policy

Frozen I/O signatures for stable tools are in docs/mcp_contract_v1.md

## Public-alpha note

This tool inventory is broader than GPT Actions public v1. GPT Actions public v1 is a conservative read-only subset documented in docs/GPT_ACTIONS_SETUP.md.
