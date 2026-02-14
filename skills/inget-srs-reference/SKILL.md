---
name: inget-srs-reference
description: Use as a quick reference when explaining how the `inget` CLI SRS workflow works (queues, limits, decks/tags) or when troubleshooting review-session command usage.
---

# Inget SRS reference

## Quick checks
- Verify CLI is available:
  - `inget --version`
  - `inget --help`

## Common workflow
- Review due items: `inget review`
- Constrain a session: use flags like `--limit`, `--minutes`, `--deck`, or `--tag` (depending on CLI support).

## Troubleshooting
- If a flag/subcommand differs, use the built-in help for the exact syntax:
  - `inget <subcommand> --help`
