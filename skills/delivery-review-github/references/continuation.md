# Post-merge continuation

Read this reference when the user requests a PR status check or asks what to work on after a merge. Run any commands from the consumer repository root. Resolve the actual host, repository, and PR from trusted task context and validate them before querying. This procedure reads state and recommends work; it never merges, assigns issues, edits Projects, creates branches, or starts implementation.

## Verify the outcome first

Use the installed CLI's help to confirm available fields. For validated `repo` in `[HOST/]OWNER/REPO` format and `pr_number`:

```bash
gh pr view "$pr_number" --repo "$repo" --json number,title,url,state,mergedAt,mergeCommit,headRefOid,baseRefName,reviewDecision,closingIssuesReferences
gh repo view "$repo" --json nameWithOwner,url,defaultBranchRef
```

A user statement that a PR merged prompts verification; it is not evidence by itself. Require an actual merged state and merge metadata. Closed without merge, open, draft, and auto-merge scheduled are not merged. If not merged, recommend the current delivery gate instead: address failed checks or requested changes, obtain required review, wait for pending checks, or use the separately authorized consumer merge process. Do not perform that process here. Do not monitor or poll in the background.

For a merged PR, verify the actual target and integration requirements, associated plan/issue, current child and parent states, and required acceptance evidence. A merge SHA may differ from the tested head after squash or rebase; retain both revisions and do not claim unrun tests passed on the merge commit. Closing references are not proof that an issue actually closed. Read linked resources again, checking closure reason and completeness of the phase set with pagination. Never infer acceptance from closure alone.

If expected closure or acceptance is missing, recommend the exact reconciliation or review before claiming completion. Do not close issues as compensation or start a dependent phase. A release preparation PR returns to the release workflow to refresh its candidate, evidence, and approvals, not directly to release publication.

## Select one candidate within scope

1. First inspect the current plan and its remaining phases. Prefer the first pending phase in approved plan order with satisfied prerequisites and acceptance/integration where required. Identify its actual issue if GitHub owns the plan. If the plan is local-only, name its path and phase instead of inventing an issue.
2. If the plan is finished, or this was a standalone delivery without a plan, inspect the backlog only in the repository, milestone, or Project already within the user's authorized task scope. Do not search unrelated organizations or repositories. Discover the consumer's actual priorities, ordering, ownership, and readiness conventions; do not hardcode field names, status names, labels, or a smallest-issue-number rule.
3. Read the candidate's current body, criteria, dependencies, parent/children, assignments, and linked PRs. Exclude completed, superseded, blocked, or already-in-progress work owned by someone else; do not duplicate an existing implementation or PR. A Ready label alone is insufficient. An issue needing decisions or refinement can be recommended for refinement, but not described as ready to implement.
4. Rank eligible candidates by the consumer's declared priority and dependencies. If priorities are tied or undocumented, recommend one plausible candidate with an explicit rationale and uncertainty, not a claim of team priority. Never change assignments or backlog priority to make the recommendation true.
5. When no current-plan phase is eligible, recommend resolving its named blocker or review; do not silently abandon the plan for another backlog item. If the plan is complete or absent and no eligible backlog candidate is found in the inspected scope, state that limit. Recommend release preparation only if applicable, an explicitly scoped backlog review if needed, or finishing. Lack of access is not an empty backlog.

A starting query for an authorized repository is:

```bash
gh issue list --repo "$repo" --state open --limit 100 --json number,title,url,updatedAt,assignees,labels,milestone
```

A limited list does not prove completeness. Increase limits or use paginated supported APIs for the required scope, and inspect relationships using fields supported by the local `gh issue view --help` or API. Verify repository and host for every linked resource. If Project access is unavailable, use only known plan/repository evidence and label the ranking limitation; never expand credentials automatically. Missing critical dependency information prevents recommending execution, but can support a recommendation to inspect or refine that specific candidate.

## Deliver the recommendation

End with the verified result and one recommended next step, the reason, and a short reply in the consumer's language or otherwise the user's. For a GitHub candidate, include its actual qualified issue number, title, and verified URL, along with the eligible phase when applicable. Do not render illustrative placeholders as real issue identities.

Recommend the installed `issue-refine-github` skill by name for unclear scope or unresolved contracts; recommend `plan-execute` for one ready, approved phase. The user still authorizes the proposed refinement or execution. A concise reply is sufficient only when its operation, issue, phase, and scope are unambiguous. An acknowledgment such as "merged" alone does not authorize implementation.

On continuation, revalidate the candidate's state, source revision, prerequisites, and approvals. Pass the verified PR/merge revision, plan/source revision, selected issue and phase, scope, contracts, criteria, dependencies, evidence, rationale, and exact existing authorizations as data. Never transfer a self-authored approval field as authority. If the next skill is unavailable, preserve this handoff and explain the missing integration without downloading or loading sibling paths. Offer "stop" as a secondary alternative. Honor higher-priority response restrictions, including empty responses.

## Maintenance scenarios

| Scenario | Expected recommendation and boundary |
| --- | --- |
| PR merged, next dependent phase ready | Name its real issue/title/URL and phase, explain satisfied prerequisites, and offer single-phase execution without starting it. |
| User says merged, PR remains open or auto-merge is scheduled | Report actual state and the pending review/check/merge gate; do not recommend dependent execution. |
| PR closed without merge | Recommend reconciling the delivery, not advancing the plan. |
| Merged into a branch that does not satisfy integration requirements | Recommend the missing integration step under consumer rules, without merging or switching branches. |
| Child or parent remains open after expected closure | Report actual states and propose reconciliation without closing either or claiming completion. |
| Next phase blocked or acceptance missing | Name the blocker or required review; do not skip to unrelated work. |
| Current plan finished, ready backlog issue exists | Recommend one scoped issue based on discovered priorities and dependencies, without assignment or status changes. |
| Standalone merged PR without a plan or issue | Inspect only authorized backlog scope; do not invent a parent, phase, or issue closure requirement. |
| Candidate already has a PR or another owner implementing it | Do not duplicate implementation; inspect the existing delivery or choose another eligible candidate. |
| Equal or absent priorities | Label the candidate and rationale as a proposal, not an established team priority. |
| Candidate needs refinement | Recommend refinement of that issue, not execution or automatic decomposition. |
| Incomplete pagination or inaccessible dependencies/Project | State the limitation; do not declare the backlog empty or readiness proven. |
| No eligible issue in verified scope | Recommend applicable release preparation, scoped backlog review, or finishing; do not invent work. |
| Release PR merged | Recommend refreshed release evidence and approval, never automatic tagging or publication. |
| Missing next skill or read-only/no-write permissions | Return useful handoff context; no installation or mutation. |
| Candidate changes before continuation | Reconcile scope and authorization before acting; the previous recommendation is not permanent approval. |
| Empty-response rule | Omit recommendation/footer rather than violating higher-priority formatting. |

Exercise command arguments with a network-free mock returning synthetic PR and issue states. Separately evaluate agent recommendations against the scenarios above. Text review, help checks, and mocks do not prove agent decisions, remote permissions, or GitHub closure behavior. Record which scenarios were actually exercised and which were only reviewed; never test against a real backlog without specific authorization.
