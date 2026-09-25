---
name: skill-name
description: Describe when this skill should activate, what task it solves, and which user requests match.
---

# Skill name

## Goal

Describe the concrete outcome and task boundaries. Replace all guidance text in this template before publishing the skill.

## Requirements

Specify the required tools, versions, permissions, and data, and how to check them. Do not assume any dependency is installed.

## Procedure

1. Inspect the consumer project and its instructions before changing files.
2. Identify the necessary information and check task preconditions.
3. Describe this skill's specific steps here, with verified commands and explicit relative paths.
4. Verify the result with checks appropriate to the consumer project.
5. Summarize changes, checks performed, and any actual blockers.

## Recommended continuation

End with the actual result and one recommended next step, its reason, and a short reply tied to the real issue, phase, branch, or document. Use the consumer's language or otherwise the user's. Prioritize a specific blocker over advancing; if nothing remains, recommend finishing instead of inventing work. Keep revision or stopping as secondary alternatives, not a generic menu. Respect higher-priority response rules, including empty responses.

Define this skill's state-specific recommendations here. Distinguish proposal, approval, execution, verification, and acceptance. A short reply applies only to the exact unambiguous scope already shown; revalidate state and existing authorizations on continuation. Recommendations do not grant permission to write, commit, push, publish, merge, or start another task. Resolve optional next skills by installed name without sibling-file dependencies or downloads; preserve source/revision, scope, criteria, dependencies, evidence, and approval references as handoff data. If an integration is missing, report it and provide the context without silently replacing its executor.

## Validation

Define observable success criteria, at least one representative case, and relevant error cases. Explain how to check the result without unnecessarily modifying real data. Cover closing recommendations for success, blockage, no changes, missing next skill, changed state after approval, and response restrictions; distinguish static checks and command mocks from observed agent behavior.

## Boundaries and safety

Document what this skill does not do, when to stop, and which actions require authorization. Do not include credentials, personal paths, or destructive actions by default.

## Optional resources

If needed, create and link files in `references/`, `scripts/`, or `assets/` within this folder. State when to read each reference, the working directory for each script, and its inputs, outputs, and dependencies. Remove this section if there are no additional resources.
