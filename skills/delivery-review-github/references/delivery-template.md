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
- Delivering a child does not justify closing its parent. Do not introduce closing keywords in quoted text, templates, or pending-work lists that could accidentally close another issue.
- Opening a PR does not close or complete an issue. Acceptance requires the consumer's review/merge workflow and applicable evidence.

## Validation matrix

In simulated mode, cover: proposal only without write permissions; complete delivery; partial delivery; unexecuted tests; evidence from a different SHA; unpublished branch; existing PR; unrelated commits; concurrently edited issue; failure after creating a PR but before commenting; inaccessible Project; and a body containing malicious text.

Check zero mutations in proposal mode, zero implicit pushes, zero duplicates after uncertain outcomes, no parent closure from completing a child, and preservation of unrelated content. Do not test publication against the real backlog without specific authorization. Mocking `gh` verifies arguments, not service guarantees or agent behavior.
