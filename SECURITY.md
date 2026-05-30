# SECURITY (Public Alpha Scope)

## Scope

This document describes security posture and constraints for the public-alpha surface only.

## Public-alpha boundaries

- GPT Actions v1 is read-only and limited to approved operations.
- Write/admin/decision-mutation routes are excluded from public GPT Actions v1.
- openapi.json is internal/generated reference, not the public GPT Actions contract.

## Secrets and credentials

- Never commit .env or secrets.
- Never publish private IDs, tokens, or infrastructure credentials in docs/schemas.

## Non-claims

- No claim of complete hardening for every deployment environment.
- No claim of full API-wide security guarantees beyond documented public-alpha scope.

openapi/context-brain-actions.public.openapi.yaml
