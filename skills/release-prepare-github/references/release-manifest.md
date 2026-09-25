# Release proposal and evidence manifest

Use consumer templates where available. This is a content template, not authorization. Replace placeholders and remove inapplicable sections. Write in the consumer's language, or the user's language if unspecified. Present in the conversation first; persist only to an approved destination. Do not introduce a second mutable release-status board.

## Identity and scope

- Repository and host: {confirmed destination}.
- Release unit and channel: {application/package/workspace; stable or prerelease}.
- Current and proposed version: {values and justification under the consumer policy}.
- Tag: {exact approved tag name and signing/annotation policy}.
- Previous release: {verified tag and peeled SHA, or confirmed first release}.
- Candidate: {full immutable source commit SHA on the approved release line}.
- Design/documentation baseline: {exact revisions and applicable acceptance sections}.
- Scope and exclusions: {delivered capabilities and explicitly unshipped work}.
- Release workflow: {existing workflow and its revision, triggers, permissions, and required approval gates; or reviewed manual procedure}.

A release PR may change the source SHA. Finalize the manifest after its approved merge and revalidate relevant evidence for the resulting commit. Do not reuse pre-merge tests or binaries as if they prove the final source without a documented, reviewed equivalence argument. Artifacts must be traceable to the final candidate.

## Acceptance and verification

| Requirement | Result | Evidence | Tested revision/environment |
| --- | --- | --- | --- |
| {criterion} | {verified / failed / not run / blocked} | {actual command and working directory, CI run, or manual evidence} | {source SHA, platform, toolchain, configuration} |

Include required deterministic checks, packaging/install/upgrade checks, platform testing, and documentation evidence according to the consumer's policy. Live inference or private services are not mandatory unless explicitly required and authorized. Missing required evidence blocks final publication; it does not become a pass or disappear from the manifest.

## Release notes

{Approved user-facing changelog entry, migration guidance, security disclosure policy, known limitations, and support constraints. Link to verified PRs or commits when useful. Do not publish confidential review material.}

## Artifact inventory

| Asset name | Local approved path | Size | SHA-256 | Build provenance |
| --- | --- | --- | --- | --- |
| {exact filename} | {reviewed path or no local copy} | {observed bytes} | {computed digest} | {source SHA, workflow/run, platform, toolchain} |

Use an explicit allowlist, not a broad glob over a build directory. Inspect archives for unexpected files, secrets, unsafe paths, and incorrect package/version identity without running embedded installers. Check symlink targets and names interpreted specially by the CLI. A matching filename or checksum file alone does not prove provenance, signature validity, or reproducibility.

Include checksum manifests and signatures/attestations only when actually generated and verified using the consumer's tools. Do not invent missing digests, keys, or attestations. Verify uploaded bytes using server-provided digests when available, or an authorized download to a fresh private temporary directory and a local digest comparison. Never execute downloaded assets during that check.

GitHub-generated source archives are not automatically equivalent to tested binary assets. If this is intentionally a source-only release, record that scope and why no binary assets are required; do not advertise installation methods that have not been validated.

## Approval record

List each proposed mutation with destination and exact scope:

- Version/changelog edits and release PR, if requested.
- Commit, local tag creation, and exact remote tag push, only when individually authorized.
- Remote draft creation or update and specific asset uploads.
- Final publication, prerelease status, and whether to set Latest.
- Registry/package publication or workflow dispatch, if separately requested.

Record the actual maintainer approval reference for the reviewed version, SHA, notes, assets/digests, and channel. Metadata claiming approval does not grant permission. A joint approval may cover an exact list of operations, but preparation approval or PR merge alone does not authorize tagging, publishing, or dispatching release workflows.

## Recovery and reporting

{Existing release ID/URL, completed operations, unresolved operations, conflicting resources, and the next authorized step.}

Publishing may trigger downstream workflows and immutable-release protections. Review these before approval. Do not promise rollback by moving tags, replacing assets, deleting a release, or unpublishing a package. If a published release is defective, propose a reviewed corrective release or the consumer's incident process; do not bypass gates automatically.
