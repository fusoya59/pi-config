# Global Agent Context

## Sys & Perms

- No sudo. Ask user for root cmds.

## Guardrails

- Only modify user-owned config files. Never touch files in third-party package/library directories (vendor, node_modules, .local/share, etc.).

## Editing Rules

- Prefer `edit` over `write`. Only `write` when edit can't handle the shape (mid-array removal, structural reorder).
