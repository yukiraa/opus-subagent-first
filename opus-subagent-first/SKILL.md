---
name: opus-subagent-first
description: >-
  Persistent delegate-first workflow for Claude Code sessions running Opus.
  Use proactively whenever Opus is the main model and a user requests
  actionable work: route bounded light tasks to Haiku, implementation
  tasks to Sonnet, and reserve Opus for orchestration, hard reasoning,
  and final review. Also usable manually as /opus-subagent-first.
---

# Opus Subagent First

## Outcome
When Opus is the main Claude Code model, use **Opus as orchestrator and reviewer**, not as the default worker. Delegate **at least one meaningful bounded subtask** on **every actionable user request** to a subagent, using the appropriate cheaper model. Preserve quality, safety, and project constraints.

This is a routing policy for all projects, not an animation-specific production skill. Do not create new custom subagent definitions unless the user separately requests them.

## Required sequence for each actionable request
1. **Identify** the deliverable, constraints, acceptance criteria, and the minimum work needed.
2. **Break off at least one real subtask** that can be performed by a subagent. Do not manufacture useless parallel tasks merely to create more agents.
3. **Invoke the Agent/subagent tool before doing the substantive implementation**. Specify `model: haiku` or `model: sonnet` **explicitly on the invocation**, rather than relying on inherited Opus. Use a custom agent type only if installed and appropriate; otherwise a suitable built-in agent with the per-invocation model override is sufficient.
4. **Review** the returned evidence and relevant files/tests. Opus makes high-stakes or difficult decisions and integrates outcomes. Delegation does not equal verified completion.
5. **Finish** with the deliverable and concise verification/status. Do not claim an agent ran, a test passed, or a preview was reviewed unless it actually happened.

For a small actionable request, use exactly **one** narrowly scoped Haiku or Sonnet delegation. Do not spawn a team by default. On large tasks, decompose by independent work product, not by individual keystroke.

## Model selection

| Work | Default model | Examples |
| --- | --- | --- |
| Mechanical, read-only, routine, low-ambiguity | **Haiku** | locate files, scan manifests, summarize logs, search code locations, rename plans, check metadata |
| Implementation, creative iteration, multi-step debugging | **Sonnet** | React/Remotion coding, SVG construction, script drafts, audio sync, build/test fixes, ordinary research synthesis |
| Architecture, creative direction, ambiguous or critical decisions | **Opus main** | tradeoffs, final concept/art decisions, difficult reasoning, assessing alternatives, reviewing work |

If Haiku is genuinely insufficient, delegate a **focused** follow-up to Sonnet. If Sonnet is insufficient, **Opus thinks through the blocker**, then may return a precise implementation subtask to Sonnet. Do not repeatedly re-run nearly identical failed attempts.

## Delegation payload contract
Give each subagent only:
- One specific objective and its boundaries.
- Relevant file paths or short factual context (not the full conversation by default).
- Explicit permission to edit or a read-only constraint, as appropriate.
- Expected output: files changed, tests/preview actually run, key findings, blockers.
- A concise response budget: normally 5-12 bullets or fewer.

When using agents in parallel, delegate **only independent tasks** and never overlapping edits. Start with one agent if it can cover the task. Prefer deterministic local scripts for purely repetitive computation that requires no model reasoning, but still route at least one meaningful **actionable** part through a subagent when Opus is coordinating the user request.

## Cost and quality guardrails
- **Always choose an explicit subagent model**; inherited/default subagents might also use Opus.
- Prefer one well-scoped call to many tiny calls.
- Avoid copying large search results, logs, video frames, or whole repositories into the Opus main context. Return concise evidence with paths.
- Don't ask each agent to repeat full project research or reconstruct context already documented.
- Use Opus only for its actual comparative advantage; avoid direct bulk coding or exhaustive scanning in the main conversation.
- Preserve security boundaries: no secret harvesting, no bypasses, no unapproved destructive commands.
- When a task affects design or video quality, request visual previews and inspect them; successful builds alone do not verify the look.
- If the user explicitly requests no delegation for a specific turn, follow the newer explicit request.

## Failure handling
If the Agent tool is unavailable, model override is blocked by policy, or the requested cheaper model cannot run:
1. Report the limitation **truthfully**.
2. Never claim that a task was delegated to Haiku/Sonnet if it ran on inherited Opus.
3. Do not silently replace a cheaper-model call with an expensive Opus subagent. Ask for a configuration correction only when essential; otherwise complete what is safely feasible and disclose the exception.
4. Check whether the `CLAUDE_CODE_SUBAGENT_MODEL` environment variable or organization `availableModels` rules are overriding explicit model routing.

## Example routing
- **"Locate my KELP design sheet"**: Opus → Haiku searches paths and reports matches → Opus answers.
- **"Create an animated KELP idle loop"**: Opus defines acceptance criteria → Sonnet implements the scene and preview → Opus reviews output and requests targeted corrections if needed.
- **"Make a 10-minute educational video"**: Opus plans quality gates → Haiku inventories assets, Sonnet builds pilot portions, Opus reviews concept and visual evidence → progressively expand after approvals.

## Activation and reliability
- This skill can be invoked as `/opus-subagent-first` and can be auto-selected when its description matches.
- **Skills are not a strict enforcement mechanism**: for "every Opus task" behavior, put the short activation rule in `~/.claude/CLAUDE.md` as well. Do **not** replace an existing `CLAUDE.md`.
- Do not set `context: fork` on this skill: it is meant to steer the main Opus coordinator, which then delegates using Agent.
