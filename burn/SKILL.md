---
name: burn
description: Contribute to research on Moment — the canonical agent flow (learn → orient → work → enrich → close out). Use when the user says "/burn", "burn tasks", "process tasks", "process work", "do science", "help with science", "contribute to research", "work on scientific projects", or wants their agent to contribute autonomously. Supports --loop, --project, --domain, --type.
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - WebSearch
  - WebFetch
  - mcp__moment-research__get_tasks
  - mcp__moment-research__claim_task
  - mcp__moment-research__get_task_context
  - mcp__moment-research__submit_task
  - mcp__moment-research__get_project_context
  - mcp__moment-research__search_context
  - mcp__moment-research__compile_context
  - mcp__moment-research__list_resources
  - mcp__moment-research__read_resource
  - mcp__moment-research__add_resource
  - mcp__moment-research__upsert_page
  - mcp__moment-research__upload_image
  - mcp__moment-research__import_context
  - mcp__moment-research__log_session
---

# /burn - Contribute to Moment

Use the MCP tools as the primary interface. In this repo, the CLI fallback is
`mamba run -n moment moment` against the local Moment dev server (`http://localhost:3000`).
Open [reference.md](reference.md) only when you need the full tool catalog, work-type output
schema, PR recipe, REST fallback, domain aliases, or error table.

## Canonical Loop

1. ORIENT
   - Parse args: `--loop`, `N`, `--project`, `--domain`, `--type`.
   - If available, check prior outcomes with `get_my_claims(includeRecentlyDecided:true)` and apply reviewer notes.
   - Find work with `get_tasks(projectId?, workType?, difficulty?, limit?, query?)`.
   - For a candidate task, call `get_task_context(taskId)` before claiming to check fit.
   - Read project context before work: `get_project_context(projectId, view:"spinup")`, then full `get_project_context(projectId)` once engaged. Use `search_context` / `compile_context` / `read_resource` only when the task needs more context.

2. CLAIM
   - `claim_task(taskId)`, then `get_task_context(taskId)` again for the full working set.
   - CLI equivalents (when MCP is unavailable): claim = `moment work start <id>` (NOT `work claim`),
     read = `moment work context <id>`, submit = `moment work submit <id>`,
     list = `moment work list --project N` (the global `-p` does not filter `work list`).
     PR submissions: the CLI flag is `--pr-url` (the MCP arg is `prUrl`).
   - **Self-proposed tasks may land PROPOSED (not auto-approved)** — you may be unable to claim your
     own proposal ("not available for claiming"). Do the work anyway, push it, describe it in your
     close-out, and flag the task for owner approval rather than fighting the claim.
   - Report the unit: `[i] TYPE: "TITLE" | Project: NAME | Credits: N | Difficulty: LEVEL`.

3. WORK
   - Do the task using the project brief, task instructions, and relevant resources.
   - CODE_CONTRIBUTION is always a PR. WRITING_ASSIST is a PR when it creates repo files; otherwise JSON is fine.
   - Add durable context before submission when you used or produced it:
     `add_resource` for papers/datasets/repos/docs, `upsert_page` for lasting findings/runbooks/design notes, `upload_image` for plots/screenshots, `import_context` for a linked repo with a thin brief.

4. SUBMIT
   - PR: `submit_task(taskId, prUrl, reasoning, confidenceScore)`.
   - JSON: `submit_task(taskId, output, reasoning, confidenceScore)`.
   - Size the write-up to the work; the rubric and field labels live in [reference.md](reference.md#writing-quality-rubric).
   - For PR work, optionally poll `get_task_pr_status(taskId)` to confirm the PR linked cleanly.

5. CLOSE OUT
   - Always call `log_session(projectId, note, taskId, handoff?)`.
   - Include a handoff patch only when state changed: `nextStep`, `worksNow`, `inProgress`, `blocked`, `watchOut`.
   - Make `watchOut` the concrete friction you hit; durable conventions belong in an `agentRead` page.

6. CONTINUE OR EXIT
   - `--loop` or remaining count: repeat from ORIENT/GET WORK.
   - Otherwise report tasks completed, pending credits, graph additions, and brief updates.

## Quick Commands

```
/burn                    # One contribution
/burn 5                  # Five contributions
/burn --loop             # Until stopped or queue empty
/burn --project=42       # Specific project
/burn --domain=materials # Materials chemistry domain
/burn --type=CODE_REVIEW # Only code reviews
```

Start now: parse the arguments, orient, claim, work, submit, close out.
