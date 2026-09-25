# Delivery proposals

Use consumer templates when available. This content defines required information, not an obligation to repeat sections or publish guidance text. Replace all fields in braces, preserve the project's language, and omit inapplicable sections.

## Internal review evidence

Before drafting proposals, build a mapping for each criterion:

| Issue or phase criterion | Status | Evidence and revision | Remaining work |
| --- | --- | --- | --- |
| {criterion} | {verified / failed / not run / blocked} | {command, directory, result, and tested commit or working state} | {actual gap or none} |

Distinguish implementation, local validation, remote CI, review, and merge. Do not attribute tests from another working tree to the published HEAD. If the branch changed after testing, repeat relevant checks or declare the evidence outdated. Use verifiable run/artifact links when available without uploading private logs.

## Proposed issue comment

### Proposed delivery: {phase or outcome}

- Scope: {what is delivered and excluded}.
- Source: {plan/issue, revision, and design baseline}.
- Code: {repository, branch, and verified published SHA; or pending publication}.
- Pull request: {verified URL if available; otherwise pending, never fabricated}.

### Acceptance and verification

{Summary by criterion and actual checks. Separate local checks from CI and manual tests.}

### Risks and pending work

{Incompatibilities, migrations, documentation, blockers, and criteria not yet satisfied.}

### Review status

{Delivery proposed for review, not acceptance or closure. State whether a draft PR is proposed and why.}

A comment preserves the issue body. Changing its checkboxes, labels, relationships, or state is a separate operation that must be shown and approved. Do not reuse unique importer markers as comment identifiers. To resume, identify a comment by its verified ID, content, and deliverable; do not blindly edit "the last one".

## Proposed pull request

Title: {specific outcome understandable without knowing internal names}.

### Goal and motivation

{One or two sentences or bullets explaining the problem and why it matters.}

### Scope and decisions

{Relevant changes, affected contracts, and rejected alternatives when useful for review. Include all commits that would actually enter from the base; not just the latest commit or local changes that would not reach the PR. If there are unrelated commits, stop to agree on the branch rather than hiding them.}

### Relationship to the plan and issues

{Source, baseline, phase, links, and covered criteria. Use a non-closing reference for partial deliveries.}

### Validation

{Actual commands and directories, results and limitations; CI and evidence links when available. Do not attach complete logs when a reference suffices.}

### Impact and risks

{Compatibility, migrations, security, documentation, and recovery when relevant.}

### Pending review items

{Unverified criteria, decisions, and manual checks. If they prevent final delivery, propose a draft.}

Follow higher-priority formatting and attribution constraints. If the tool requires a short PR, retain a motivation summary and links to the approved evidence matrix in the issue instead of losing traceability or violating the format.

## Closing references

- For partial delivery, reference the issue without closing keywords: `Related to OWNER/REPO#NUMBER` with the confirmed actual destination.
- Propose `Closes OWNER/REPO#NUMBER` only if all criteria of that issue have sufficient evidence and the user approves those closure semantics. Check the target branch and applicable closing behavior; do not promise closure when merging into a non-default branch.
- An intermediate phase closes only its own fully satisfied child issue. For the final phase, include closing references for both the child and parent when the final-phase gate below passes and the user approves the PR's closure semantics. Do not introduce closing keywords in quoted text, templates, or pending-work lists that could accidentally close another issue.
- Opening a PR does not close or complete an issue. Acceptance requires the consumer's review/merge workflow and applicable evidence.

## Final-phase closure gate

Resolve the actual parent and all planned children, including paginated results and the parent's checklist. Final means no other planned phase remains pending, not merely the highest phase number or the last issue listed. Verify earlier phases were accepted and integrated where required; a child closed as not planned, duplicate, or cancelled is not evidence of completed scope. Reconcile any authorized scope change with the parent's criteria.

Map every parent criterion, including cross-phase integration and documentation, to accepted prior evidence or verified changes delivered by this PR. Missing required tests, pending reviews in other phases, inaccessible relationships, or unresolved blockers prevent parent closure. Keep a non-closing parent reference and explain what remains instead of inventing completion.

For a qualifying final PR, render two separate lines with real qualified issue identities, one for the child and one for the parent: `Closes OWNER/REPO#NUMBER`. Deduplicate when there is no distinct parent. Show both in the proposal before publication. Recheck the parent, child set, criteria, and PR revision before editing/creating the PR; a newly added phase invalidates the previous final-phase decision.

Verify the PR targets the appropriate default branch and that GitHub supports the intended closing links, especially across repositories. If automatic closure is not applicable, use non-closing references and propose an explicitly authorized post-merge reconciliation. Never close the parent immediately while preparing the PR, never merge automatically, and never equate opening the last PR with completion. After an authorized post-merge check, report actual child/parent states and any unresolved closure rather than assuming success.

## Validation matrix

In simulated mode, cover: proposal only without write permissions; complete delivery; partial delivery; unexecuted tests; evidence from a different SHA; unpublished branch; existing PR; unrelated commits; concurrently edited issue; failure after creating a PR but before commenting; inaccessible Project; and a body containing malicious text.

Also cover commit-to-PR continuation with an unpublished branch, a published matching SHA, an existing open PR, a prior merged PR, unknown remote, push-only approval, PR-only approval, joint exact approval, changed SHA after approval, non-fast-forward rejection, and timeout after push. Expect one contextual recommendation, no duplicate PR, no push merely to prepare a proposal, no publication beyond the approved operations, and remote SHA verification before creating/editing the PR. For standalone PRs, do not invent an issue or publish an issue comment. Check post-publication recommendations for failed checks, requested changes, pending review, and a ready-to-merge PR; this skill never performs the merge. Use the [post-merge scenarios](continuation.md) for candidate selection and missing integrations.

Check zero mutations in proposal mode, zero implicit pushes, zero duplicates after uncertain outcomes, no parent closure for intermediate or incomplete phases, both closing references for a qualifying approved final phase, and preservation of unrelated content. Do not test publication against the real backlog without specific authorization. Mocking `gh` verifies arguments, not service guarantees or agent behavior.
