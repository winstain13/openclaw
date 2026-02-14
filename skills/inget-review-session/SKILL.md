---
name: inget-review-session
description: Use when the user asks to run an SRS/spaced-repetition review session using the `inget` CLI ("start a review", "review my cards", "do an SRS session", "inget review"). Guide an interactive session: choose deck/scope, run the command, and iterate through prompts.
---

# Inget review session (SRS)

## Goal
Run a focused, interactive review session via the `inget` CLI.

## Flow (default)
1) Confirm *what to review* (deck, tag, or default queue) and *session length* (time or cards).
2) Run the appropriate `inget` command.
3) For each prompt, help the user answer quickly (keep momentum) and record the result.
4) End with a short recap + next suggested review time.

## Commands (common)
> Adjust to the installed `inget` subcommands in this repo/version.

- See help:
  - `inget --help`
  - `inget review --help`

- Start a session (typical patterns):
  - `inget review`
  - `inget review --deck <name>`
  - `inget review --limit <n>`
  - `inget review --minutes <n>`

## Coaching prompts
- "Want to do a quick 5–10 minute session or a longer one?"
- "Any deck/topic you want to prioritize today?"
- "When in doubt: answer fast; we can flag hard cards for later refinement."
