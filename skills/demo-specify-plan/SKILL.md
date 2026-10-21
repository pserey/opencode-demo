---
name: demo-specify-plan
description: Research and write or revise a concise, implementation-ready plan in the current workspace's artifacts directory, splitting it into one-repository tasks when needed. Use when the user asks to specify, plan, or decompose implementation work before coding in the demo workspace.
version: 0.1.1
---

# Specify an implementation

Produce a local-only handoff that a fresh implementation agent can execute from the repository and planning files. Do not implement the change.

## 1. Confirm context and location

Before drafting, read workspace instructions and inspect the repositories and documents already named or readily discoverable.

Treat the directory containing the nearest ancestor `AGENTS.md` as the workspace root. Store this skill's local-only planning artifacts in one descriptive, kebab-case action directory: `<workspace-root>/artifacts/<action>/`. Use `PLAN.md` for a one-shot plan and `tasks/` beneath that action directory for decomposed plans. If there is no discoverable workspace root or `artifacts/` directory, ask before creating or selecting a location.

In the first response, briefly list:

- the context sources found or supplied;
- the proposed planning directory.

Ask whether any other essential source exists and whether to proceed there. Do not repeat this checkpoint when the current conversation already answers both questions.

Workspace instructions take precedence when they prescribe a different repo-agnostic planning directory. Otherwise, use the workspace `artifacts/` directory; do not put planning files inside the target repository. If an exceptional requested location is inside a Git repository:

- use only untracked paths and never overwrite a tracked planning file;
- add the exact planning paths to `.git/info/exclude`;
- never modify `.gitignore` for planning artifacts.

Planning artifacts are local-only: never stage, commit, push, or version them.

## 2. Establish feasibility

Before writing the plan:

- verify referenced repositories, files, packages, classes, symbols, and integration points;
- inspect existing tests and project scripts to derive real local validation commands;
- use cheap, read-only discovery where useful;
- resolve questions that could change scope, architecture, task boundaries, or validation.

Do not modify implementation code or perform deep implementation/runtime validation. Record only non-blocking assumptions; resolve blocking questions first.

## 3. Choose the handoff shape

Keep all details in `PLAN.md` when the work is one reviewable unit. Create task files when:

- more than one repository is involved; or
- conceptual complexity or review size calls for multiple PR-sized units.

Enforce these rules:

- one task modifies exactly one repository;
- dependencies between tasks are allowed;
- use roughly 400 meaningful hand-written implementation lines as a decomposition heuristic, excluding tests, documentation, generated artifacts, snapshots, and lockfiles;
- do not record LOC estimates unless a substantially larger task needs a short justification;
- suggest stacked delivery when same-repository tasks form a dependency chain, but mark it as stacked only after user confirmation;
- delivery defaults to a normal branch, and cross-repository tasks cannot share a Git stack.

For multiple tasks, use `artifacts/<action>/tasks/01-<short-name>.md`, `artifacts/<action>/tasks/02-<short-name>.md`, and so on. Numeric order is the recommended execution order; explicit dependencies are authoritative.

## 4. Write the plan

Keep prose short. Prefer concrete bullets. Do not include pseudocode, code blocks, progress checklists, approval metadata, or an implementation diary.

A one-shot `PLAN.md` contains:

- context, goal, scope, and non-goals;
- exact source paths or URLs and relevant sections;
- the implementation approach and reviewed decisions;
- repositories, likely files/symbols, required behavior/interfaces, and constraints;
- local validation commands and expected outcomes;
- user-run external validation, assumptions, and risks when applicable.

A decomposed `PLAN.md` contains shared context, scope, cross-task decisions, a relative-linked task index, dependencies, and end-to-end validation without duplicating task details.

Each task file contains:

- outcome;
- repository as a workspace-relative path and remote slug when available;
- dependencies and delivery (`normal branch` or `stacked branch`);
- a branch name only for stacked delivery;
- relevant files/symbols and required behavioral or interface changes;
- implementation constraints and decisions;
- local validation commands and expected outcomes.

Summarize every source-derived requirement needed to implement the work. References provide deeper context; they never substitute for requirements. If an implementer may lack source access, bake the necessary facts into the plan.

Validation stays local and bounded. Do not direct the agent to invoke deployment or control-plane CLIs such as `aws`, `pulumi`, or `kubectl`; put necessary commands under user-run validation instead.

After writing, report the created paths and invite review. When revising, keep links and dependencies consistent and remove obsolete local task files. Stop without implementing.
