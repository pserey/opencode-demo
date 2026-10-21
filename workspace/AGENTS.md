# AGENTS.md

## Working Agreements

- Read the repository's contribution guidance and relevant code before proposing changes.
- Treat the linked issue, approved specification, and existing tests as the source of truth for scope and expected behavior.
- Prefer the smallest change that solves the stated problem. Preserve established conventions unless a change is required by the task.
- Do not use a known external implementation as the basis for a solution. Investigate, plan, implement, and validate from the task requirements.

## Planning and Artifacts

- Keep plans, review exports, investigation notes, test evidence, and other non-source deliverables in `artifacts/`.
- Create one descriptive, kebab-case directory per action under `artifacts/`, for example `artifacts/issue-123-short-help/`.
- Keep every artifact for an action, including its `README.md`, inside that action's directory. Do not add files directly to `artifacts/`.
- Do not add generated artifacts, temporary files, or presentation material to a source repository unless explicitly requested.
- Before implementation, record the intended scope, affected areas, validation approach, and any assumptions in an artifact when the change is not trivial.

## Implementation

- Create a dedicated branch for each change; do not commit feature work directly to `main`.
- Add or update focused regression tests for changed behavior.
- Keep production changes, tests, and documentation consistent with one another.
- Avoid unrelated refactors and formatting-only changes in task branches.

## Validation and Pull Requests

- Run the narrowest relevant tests while iterating, then run the project-recommended validation before requesting review.
- Report the commands run and their results in the pull request description or a validation artifact.
- A pull request must explain the problem, the change, and the validation evidence. Link the tracked issue when applicable.
