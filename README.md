# Agent Squad 🪖

A ready-to-drop `.claude/agents/` setup that turns Claude Code's **nested sub-agents** (v2.1.172+) into a working team. You give one instruction to the `@orchestrator` and it fans out into a tree of agents that spawn their *own* agents — all inside the 5-level depth cap.

Every agent inherits the same coding philosophy from `CLAUDE.md`: think before coding, keep it simple, make surgical changes, read before you edit.

## The tree

```
you (main thread)              depth 0
└─ @orchestrator               depth 1   runs the whole pipeline
   ├─ planner ──► scout        depth 2→3  plans the work, sends scout to recon the codebase
   ├─ builder ──► frontend     depth 2→3  implements, splits UI / server work
   │          └─ backend
   └─ reviewer ──► security    depth 2→3  reviews, spawns security + simplicity passes
              └─ simplifier
```

`planner`, `builder`, and `reviewer` can spawn sub-agents (they have the `Agent` tool). `scout`, `frontend`, `backend`, `security`, and `simplifier` are leaf workers — they do one job and report back. Max depth here is 3; Claude Code's hard ceiling is 5.

## Install

Drop the `.claude/` folder and `CLAUDE.md` into any project root:

```bash
# from your project root
git clone https://github.com/<you>/claude-agent-squad tmp-squad
cp -r tmp-squad/.claude ./
cp tmp-squad/CLAUDE.md ./
rm -rf tmp-squad
```

Then in Claude Code:

```
@orchestrator add a dark-mode toggle to the settings page
```

Watch it plan → scout the code → build → review, spawning sub-agents as it goes. Open the **subagent pane** (desktop) or run `claude agents` to see the tree live.

## Notes

- **Spawning tool**: these agents use the `Agent` tool to spawn children. If your Claude Code version still calls it `Task`, swap `Agent` for `Task` in the `tools:` line of `orchestrator`, `planner`, `builder`, and `reviewer`.
- **Token cost compounds per branch** — each level gets its own context window. Keep the tree shallow (2–3 levels is the sweet spot). The `simplifier` and `scout` agents are deliberately cheap and focused for this reason.
- **Customize freely**: the agent bodies are just markdown. Rewrite them for your stack. The value is the *pattern*: coordinators that delegate, leaves that execute, one shared `CLAUDE.md` philosophy.

## Why nested agents

Before v2.1.172 a sub-agent was a dead end — it did its job and the branch ended. Now any sub-agent can spawn its own, so one instruction can set up a whole working team. This setup is a clean starting point you can strip down or build on.
