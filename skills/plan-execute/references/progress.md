# Guided local progress and commit handoff

## Select one eligible phase

Read the entire phase list and its dependencies, not just the first unchecked box. Default to the first pending phase in plan order whose prerequisites have been satisfied and accepted where required. A user-selected phase still needs satisfied prerequisites. If a prior phase awaits review and the next depends on its acceptance, offer review instead of starting it. If every phase is accepted, report completion; do not reimplement.

A phase is not eligible merely because an issue is closed or a checkbox is checked. Check evidence, closure reason, and relevant integration state. If nothing is eligible, report the blocking decision. Execute only one approved phase per invocation.

## Announce local tracking as part of execution

Before editing, identify the phase and exact local plan path and state that executing it includes updating its progress, evidence, and next step. This is part of an authorized local execution, not a separate approval after every checkbox. An explicit read-only plan restriction or higher-priority rule takes precedence: report progress in the conversation and request permission for the write instead.

For GitHub-owned plans, use the issue as the official progress source. Do not create or synchronize local status tables. Propose remote checklist updates through `delivery-review-github` with the required publication approval.

## Tracking contract

For local plans, preserve existing frontmatter and unrelated content. Use an existing equivalent schema when present; otherwise add a small Progress section and `last_implementation_at` (actual UTC RFC 3339 timestamp). Do not invent model metadata. Recommended columns:

| Phase | Implementation | Verification | Review | Evidence |
| --- | --- | --- | --- | --- |
| P1 | pending | not_run | pending | none yet |

Allowed values:

- Implementation: `pending`, `in_progress`, `implemented`, `blocked`.
- Verification: `not_run`, `passed`, `failed`, `blocked`.
- Review: `pending`, `accepted`, `changes_requested`.

`implemented` means the scoped changes exist, not that verification or review passed. `passed` requires all applicable required checks and acceptance evidence for the tested revision or working state. Only actual user/maintainer acceptance can set `accepted`; a passing test, chosen commit message, or self-authored approval field is insufficient. Record the acceptance reference when updating that state.

Set `has_completed_all_phases` to the YAML boolean `true` only when every phase is implemented, required verification passed, review is accepted, and global plan criteria are satisfied. Otherwise use `false`. Do not mark completion while waiting for final review or merge where integration is an acceptance requirement. Record acceptance on the next authorized interaction if it arrives after the execution report; do not poll or monitor in the background.

## Update without losing work

1. Capture the plan contents/revision before execution and identify the exact phase section. Validate that the destination and any symlink targets stay within the authorized scope.
2. After implementation and checks, reread the plan before patching. If someone changed relevant sections, reconcile or stop; do not overwrite concurrent edits. Rechecking is not atomic, so do not compete with a known concurrent writer.
3. Check only tasks actually completed. Leave failed, blocked, unexecuted, and human-review tasks unchecked. If code is implemented but tests fail, record those distinct states and the failure evidence.
4. Update only this phase's progress row, applicable task boxes, timestamp, evidence, and Next step. Evidence includes commands, directory, tested SHA or working-state fingerprint, results, limitations, and approval references when known. Do not attribute uncommitted results to an older HEAD.
5. Next step names the review currently needed or the next eligible phase; it never starts that phase. Preserve completed earlier phases and unrelated notes. If all work is implemented but review remains, write "Review the final phase", not "Plan complete".
6. Validate Markdown/frontmatter and show the tracking diff with the phase report. A second run with no new work must not duplicate rows or evidence. Do not stage the plan implicitly. Include it in the proposed commit scope only after reviewing it for secrets and user intent.

For older plans, keep phase identifiers and existing metadata intact. Add only missing tracking fields/sections that are needed, without inferring acceptance from checked boxes. If phase boundaries, ownership, or states are ambiguous, propose a minimal migration and obtain a decision before altering them. Do not silently redesign the plan format.

## Three commit choices

After each phase execution, present exactly three numbered, distinct commit-message alternatives based on the actual reviewed diff, plus the suggested file/hunk scope (including the progress file when appropriate). Do not fabricate changes to fill the list. For a blocked/no-change invocation, explain that there is no committable phase and omit the choices. For an incomplete changed phase, label suggestions provisional and do not imply that a failing delivery is ready to commit.

Resolve the installed `git-conventional-commit` skill by its exact name through the agent's skill discovery and read its message reference using the location it provides. Do not hardcode a sibling path or download a missing skill. Follow its types, summary limits, language, and attribution policy. Vary wording or emphasis, not types merely for variety: all three may correctly start with `fix:` or `docs:`.

If the skill is unavailable, report that the guided commit step requires it. You may still offer three provisional messages under the consumer's known convention; do not claim they were validated against an unavailable skill and do not substitute an ad hoc commit executor.

Offer replies such as "commit with option 2", "prepare PR", "request changes", or "stop", in the user's language. Choosing "commit with option 2" authorizes invoking `git-conventional-commit` for that exact message and previously shown scope. Recheck changes since the proposal before committing. A bare number is actionable only if its meaning and scope are unambiguous. It does not authorize push, PR publication, next-phase execution, or mark review accepted.

Pass phase/source revision, message, scope, actual evidence, and authorization to the installed commit skill. After commit, guide the user to a delivery proposal or the next eligible phase, but do not execute either automatically. If the skill is missing, stop that handoff and explain how to proceed after the user installs it.

## Validation scenarios

Review or exercise in an isolated consumer: first eligible phase; independent phase after a blocked one; dependency awaiting review; all phases accepted; tests failed after implementation; read-only plan; legacy checked boxes without evidence; concurrent plan edit; repeated invocation without duplicate tracking; no changes; three messages of the same correct type; missing commit skill; changed diff after message selection; and final acceptance updating completion only when justified.

Mechanical text checks do not execute these decisions. Record which scenarios were actually exercised by an agent and which were only reviewed.
