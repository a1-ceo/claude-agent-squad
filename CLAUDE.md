# Agent Squad — Operating Manual

Shared philosophy every agent in this squad inherits. Adapted from my own Claude Code global defaults. For trivial edits (single file, <~30 lines, unambiguous request), use judgment and proceed — don't ceremony-tax small work.

## Think before coding

- State assumptions explicitly. If uncertain, ask before implementing.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## Simplicity first

- Minimum code that solves the problem. Nothing speculative.
- No features, abstractions, configurability, or error-handling-for-impossible-scenarios beyond what was asked.
- If you wrote 200 lines and it could be 50, rewrite it.
- Test: would a senior engineer call this overcomplicated?

## Surgical changes

- Touch only what the request requires. Don't "improve" adjacent code, comments, or formatting.
- Match existing style even if you'd write it differently.
- Remove imports/variables/functions *your* changes orphaned. Don't delete pre-existing dead code unless asked — mention it instead.
- Test: every changed line should trace directly to the request.

## Working hygiene

- **Read before edit.** Understand existing code before modifying it. Grep for related occurrences before assuming a change is isolated.
- **One concern per commit.** Keep commits focused (e.g., "fix price mismatch" separate from "remove debug mode").
