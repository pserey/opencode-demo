---
name: demo-implement-plan
description: Implement and locally validate one approved plan from the current workspace's artifacts directory, creating cohesive commits without leaking local planning artifacts. Use when the user asks to execute a demo PLAN.md or one of its task files.
version: 0.1.1
---

# Implement an approved plan

Implement exactly one approved plan or task. Invoking this skill is approval; no marker inside the planning files is required.

## 1. Select the work

Treat the directory containing the nearest ancestor `AGENTS.md` as the workspace root. The local planning boundary is one action directory under `<workspace-root>/artifacts/`.

Use the path supplied by the user. Without one, select a plan only when exactly one plausible `artifacts/*/PLAN.md` exists in the current workspace; otherwise ask. Never choose among multiple plans or tasks by inference.

- For a one-shot `PLAN.md`, implement the whole plan.
- For a task file, read it and its parent `PLAN.md`, then implement only that task.
- For a decomposed plan without a selected task, ask for one. Never implement multiple tasks in one session.

## 2. Preflight before editing

1. Read all workspace and repository instructions.
2. Read and follow the `git` skill before making changes because commits are part of this workflow.
3. For stacked delivery, also read and follow the `gh-stack` skill.
4. Verify the repository, dependencies, current branch or stack, and working tree.
5. Confirm predecessor tasks or branches required by the selected task are present.

Never commit directly to trunk. Use the current branch only when it clearly belongs to this task; otherwise create a concise task branch before editing. For stacked delivery, use the planned branch name and create or select the correct stack layer before editing.

Continue around unrelated uncommitted changes, but never modify or stage them. Stop when existing changes overlap the task or ownership is ambiguous. Also stop when the branch, dependency state, or stack topology is wrong.

## 3. Protect the local planning boundary

Planning artifacts in the workspace `artifacts/<action>/` directory are local-only. Never stage, commit, push, or modify `PLAN.md` or task files, including their checklists.

Production code, tests, documentation, configuration, comments, commit messages, and PR content must stand alone. Never reference planning filenames, workspace planning paths, or the planning process. Rewrite useful rationale as durable technical context.

## 4. Implement within scope

Implement only the selected scope. Do not absorb adjacent tasks or opportunistic refactors.

Minor implementation-detail adjustments are allowed when they preserve scope, architecture, public contracts, task boundaries, dependencies, and acceptance criteria. Stop and ask before changing any of those or when the plan is materially ambiguous, incorrect, or infeasible.

Work incrementally. After each complete concern:

1. run its practical local validation;
2. inspect and deliberately stage only that concern;
3. verify the staged diff;
4. create a cohesive commit according to the `git` skill.

Do not prescribe or follow plan-specific commit boundaries unless they express an essential architectural separation.

## 5. Validate locally

Run the plan's bounded local tests, linters, type checks, and builds. Do not expand validation beyond the plan, deploy, or invoke control-plane CLIs such as `aws`, `pulumi`, or `kubectl`. When external validation is required, give the exact command to the user and ask them to run it.

Fix implementation-caused failures. When a failure appears unrelated, establish that it is pre-existing through base-revision or equivalent evidence. A task may be complete with a confirmed pre-existing failure; report it. If a required failure cannot be isolated, do not claim completion. Never weaken tests to obtain a pass.

## 6. Finish

Do not push normal branches. For stacked delivery, use `gh-stack` to push after committing; open PRs only when explicitly requested.

Return a short summary of:

- implemented changes and important design decisions;
- local validation performed and its result;
- confirmed pre-existing failures, remaining risks, deviations, or user-run validation.
