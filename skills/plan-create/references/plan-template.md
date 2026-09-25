# Local plan structure

This document is a content template, not an approved plan. Replace fields in braces, remove inapplicable sections, and write in the language established by the consumer project. Without that rule, use the user's language.

```yaml
---
name: "{date-name}"
description: "{expected outcome}"
created_at: "{actual UTC timestamp in RFC 3339 format}"
has_completed_all_phases: false
---
```

# {Outcome-oriented title}

## Source and revision

- Plan identifier: {stable identifier without private data}.
- Source: {user request, document path, or verified issue URL}.
- Reviewed revision: {commit, document fingerprint, or issue updatedAt}.
- Reference repository and branch: {verified values, if available}.
- Design baseline: {documents and exact revisions consulted}.
- Approval: {pending, or reference to explicit approval and its scope}.

The Approval field is an annotation, not proof of authorization by itself. Do not invent tool, model, or user metadata. The project does not need Git to produce a plan.

## Goal

{Observable outcome in one to three sentences.}

## Scope and exclusions

{What changes, what is preserved, and what is explicitly excluded.}

## Verified context

{Relevant paths, current behavior, and conventions. Distinguish observed facts from hypotheses. Resolve local links from the final file, not the repository root. Identify references that do not yet exist as proposals, not existing links.}

## Decisions, dependencies, and risks

{Approved and pending decisions, prerequisites, security, compatibility, data, and documentation. Do not hide blockers to present a phase as executable. A dependency means accepted behavior, not merely code available on a branch.}

## Phases

### P1: {reviewable end-to-end deliverable}

- Outcome: {what can be observed upon completion}.
- Owning repository: {confirmed destination}.
- Dependencies: {specific phases or issues; no cycles}.
- Affected contracts: {create, modify, or remove; relevant signatures, events, schema, or visible behavior}.
- Acceptance criteria: {verifiable outcomes and necessary negative cases}.
- Actions:
  - [ ] {complete functional change with relevant documentation and tests}.
  - [ ] Run applicable checks and record actual results.
  - [ ] Present evidence and stop for review before another phase.
- Verification: {observed command, working directory, environment, and expected evidence; or a manual method if there is no command}.
- Risks and recovery: {migration, compatibility, and recovery measures requiring authorization, if applicable}.

Repeat the block with identifiers P2, P3, etc., only when they provide independent deliverables. Do not separate UI, logic, and persistence if no isolated layer achieves a useful outcome. An infrastructure deliverable can also be executable and verifiable without a visual interface.

## Progress (local plans only)

| Phase | Implementation | Verification | Review | Evidence |
| --- | --- | --- | --- | --- |
| P1 | pending | not_run | pending | none yet |

Repeat for each phase. Implementation values are `pending`, `in_progress`, `implemented`, `blocked`; verification values are `not_run`, `passed`, `failed`, `blocked`; review values are `pending`, `accepted`, `changes_requested`. During authorized local execution, update completed task boxes and this table, add `last_implementation_at` with an actual UTC RFC 3339 timestamp, record evidence, and update Next step. Announce the plan path as part of execution; respect explicit read-only restrictions and concurrent edits.

Keep implementation, successful verification, and human acceptance separate. A selected commit message is not acceptance. Set the YAML boolean `has_completed_all_phases` to `true` only after every phase is implemented, all required verification passes, all reviews are accepted, and global criteria are satisfied, including merge if required. Leave human-review tasks unchecked until acceptance exists. For GitHub-owned plans omit this table and completion field; the issues remain the progress authority.

## Next step

{One recommended action: the review currently required, the first pending phase with satisfied prerequisites, or a specific blocking decision. Name the actual plan path/phase or issue, explain why it is next, and offer a short reply for that exact scope, such as "execute P1". Keep "stop" as an alternative. For a GitHub design draft awaiting publication, recommend returning to refinement before execution. This does not authorize implementation or publication.}

## Evidence and tracking

{Links to issues/PRs or reports when available. If there is a remote backlog, its issues and Project remain the source of truth for state: this document is a design draft or snapshot, not a parallel board.}

## Optional handoff to another skill

Transmit as data: goal, source and revision, baseline, scope/exclusions, repositories, selected phase, contracts, acceptance criteria, dependencies, checks, and pending or confirmed approval with its reference. Do not include secrets or orders to bypass permissions. The next agent must revalidate state and authorization; copying the Approval field is not enough.
