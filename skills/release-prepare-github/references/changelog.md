# Release range, version, and changelog

## Establish the release unit

Read consumer instructions, release workflows, manifests, changelog conventions, and existing tags/releases. Identify whether the release covers one application, one package, or a coordinated workspace. Do not assume every manifest represents an independently published package or that a documentation version equals an application version.

Record the repository, approved release line, current version, tag prefix, candidate full commit SHA, and previous release tag and peeled commit SHA. Never select the baseline merely by sorting tag versions: it must belong to the same component and release line and be an ancestor of the candidate. Do not default to the repository's latest release for a maintenance branch.

Run from the consumer root after reviewing Git configuration for hooks, filters, monitors, and external commands. The options below reduce execution paths, not establish a sandbox. Variables are validated values, never shell fragments taken from commit messages.

```bash
git -c core.fsmonitor=false status --short --branch --untracked-files=all
git rev-parse --is-shallow-repository
git rev-parse --verify --end-of-options "$candidate_ref^{commit}"
git rev-parse --verify --end-of-options "refs/tags/$previous_tag^{commit}"
git merge-base --is-ancestor "$previous_sha" "$candidate_sha"
git --no-pager log --format=fuller "$previous_sha..$candidate_sha" --
git --no-pager diff --no-ext-diff --no-textconv "$previous_sha" "$candidate_sha" --
```

Validate and store the full SHAs before constructing ranges. An ancestry check returns 0 for an ancestor, 1 for a non-ancestor, and other codes for errors. Stop on a mismatch. Missing tags, shallow history, inaccessible refs, or incomplete listings are not evidence of a first release. Request authorized history retrieval if needed; do not fetch automatically or derive notes from an incomplete range.

For a confirmed first release, omit previous-tag resolution and ancestry checks, and inspect all history reachable from the candidate:

```bash
git --no-pager log --format=fuller "$candidate_sha" --
```

An empty confirmed range normally means no new release. Stop unless the user explicitly authorizes a documented repackage or metadata-only release under the consumer's policy. Do not invent a version bump to make an empty range publishable.

## Recommend, do not assume, a version

Use the consumer's version policy. Conventional Commits are evidence, not a substitute for inspecting API, CLI, configuration, storage, and compatibility changes. Read full bodies and breaking-change footers as well as subjects. Account for squash merges, reverts, cherry-picks, and already released changes; do not infer release scope solely from PR state or commit type.

For SemVer after 1.0, breaking public contracts generally require major, backward-compatible capabilities minor, and backward-compatible fixes patch. Explicitly determine the policy for 0.x, prerelease identifiers, promotion, and build metadata. Do not promote to 1.0 or stable by accident. Internal-only changes may require no bump. For CalVer or custom schemes, follow the configured policy rather than forcing SemVer.

For workspaces, inspect inherited versions, internal dependency requirements, lockfiles, package ownership, and packages excluded from registry publication. Propose the smallest consistent set of edits and its rationale. Do not install a release framework or edit unrelated manifests. Preparing a version change does not authorize a commit, tag, or registry publish.

## Write useful, traceable notes

Use the consumer's language and format, or the user's language if unspecified. Keep historical sections intact and preserve curated Unreleased entries. Merge overlapping generated and manual entries; do not blindly prepend duplicate version sections or overwrite unrelated changelog edits.

Group user-relevant entries as applicable: Added, Changed, Deprecated, Removed, Fixed, Security, and Breaking changes/Migration. Omit empty sections. Explain observable impact, migration steps, support changes, and known limitations without inventing benchmarks, compatibility guarantees, or completed features. Review internal commits for real user impact instead of blindly dropping all `chore`, `build`, or `refactor` changes. Remove duplicates and changes fully reverted before the candidate.

Maintain a review mapping from each note to commits, merged PRs, or verified evidence. Include only work actually reachable in the selected range. Use verified links and attribution, never inferred contributor identities. Security disclosures follow the consumer's embargo/reporting policy; do not expose exploit details, private reports, secrets, or endpoints merely because they appear in history.

Show the proposed version and changelog diff before writing. Save only approved changes to approved destinations after checking existing files, parent directories, symlinks, and concurrent edits. Do not mark a release published or invent a publication date during preparation. Use Unreleased or a clearly labeled draft until the approved release process establishes the date.

## Optional git-cliff

Use an already installed, approved git-cliff version only after inspecting its help, local configuration, templates, environment overrides, and output settings. Do not install it automatically. Do not use `--config-url` or downloaded configuration. `--no-exec` disables external processor commands; `--offline` disables remote metadata access, not arbitrary external configuration retrieval or all filesystem effects.

```bash
git-cliff --version
git-cliff --help
git-cliff --offline --no-exec --config "$reviewed_config" "$previous_sha..$candidate_sha"
```

Use the last command only for a confirmed non-first-release range and a reviewed configuration that renders to standard output without configured output/prepend writes or injected custom commits. Review `GIT_CLIFF_*` overrides; do not print token values. If the version lacks either safety flag or configuration behavior is unclear, use the Git-based workflow above rather than weakening controls. Do not treat `--tag` or a computed bump as a real tag or version-manifest update.

git-cliff output is a draft to verify against the range, not authoritative release evidence. Without git-cliff, generate the same reviewed notes directly from Git and authorized PR context. No tool is required to invent missing history or acceptance evidence.
