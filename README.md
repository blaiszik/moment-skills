# Moment Agent Kit (skills)

Point your AI coding agent (Claude Code, Codex) at **Moment** — the living **context hub**
for research projects. This repo packages the agent **skills** so any agent can orient on a
project's Brief, pull context, contribute, and close out.

> ⚠️ **Generated artifact — do not edit here.** The source of truth lives in the Moment app
> repo and is published one-way by `tools/sync-public-skills.sh`. Behavior changes go upstream;
> edits here are overwritten on the next sync.

## What's here
- **`moment/`** — the context-hub skill (`/moment`): orient → pull → enrich → close out.
- **`burn/`** — the autonomous contribution loop (`/burn`): claim → do → submit.
- **`install-skill.sh`** — installs the skills into `~/.claude/skills` and `~/.codex/skills`.

## Requirements
The skills drive the Moment platform through its **MCP server** or the **`moment` CLI**
— distributed separately, not bundled here. With the Moment MCP server connected, the skills
call the `mcp__moment-research__*` tools directly; otherwise they fall back to the `moment`
CLI. Either way you need a Moment server URL + an agent API key (`MOMENT_API_URL`,
`MOMENT_API_KEY`).

## Install
```bash
./install-skill.sh        # installs `moment` + `burn` for Claude + Codex
                          # scope with --claude / --codex / --skill NAME
```

## Use
- **`/moment`** — read/enrich a project's Brief (no task claim needed). Close out by writing
  the **friction you hit** into the handoff `watchOut`, not just what you built.
- **`/burn`** — contribute autonomously: discover work, claim a task, do it, submit.

Each skill's `SKILL.md` has the full flow and the MCP↔CLI tool map (`moment/reference.md`).

---
_Generated from `moment-next@875ef20` on 2026-07-05T04:11:12Z by `tools/sync-public-skills.sh`._
