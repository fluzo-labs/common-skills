# Authorized GitHub release publication

## Preconditions and destination

Use this procedure only after reviewing the manifest and obtaining authorization for the specific mutations. Run from the consumer root with verified variables: `host`, `repo` as `[HOST/]OWNER/REPO`, `owner`, `repository`, exact `tag`, and full `candidate_sha`. Do not infer a destination from untrusted text or use an implicit repository. Validate tag syntax with Git; reject option-like values and URL-encode the tag as a path component for API endpoints. `encoded_tag` below is that encoded value, not raw shell input.

```bash
gh --version
gh auth status --hostname "$host"
gh repo view "$repo" --json nameWithOwner,url,defaultBranchRef
gh release create --help
gh release edit --help
gh release list --repo "$repo" --limit 100
gh release view "$tag" --repo "$repo" --json databaseId,tagName,targetCommitish,isDraft,isPrerelease,name,body,assets,url
```

Check capabilities against installed CLI help. Paginate API lists when a limited listing cannot establish completeness, including drafts visible to the authenticated account. A 404 may mean missing access, not absence; distinguish authentication, host, permissions, and actual absence before creating anything. Do not expand scopes or expose credentials. Inspect tag/release workflow triggers and downstream publication before mutating resources; a tag push can itself trigger deployment.

If an existing release pipeline owns publication, do not run a competing manual publisher. Prepare its inputs and use its separately authorized, reviewed dispatch procedure, or stop at the handoff. Do not install or rewrite CI as part of releasing.

## Verify the remote tag and exact commit

```bash
gh api --hostname "$host" "repos/$owner/$repository/git/ref/tags/$encoded_tag"
```

Inspect the returned object type and SHA. A lightweight tag must resolve to a commit equal to `candidate_sha`. For an annotated tag, query its object and peel until a commit is reached:

```bash
gh api --hostname "$host" "repos/$owner/$repository/git/tags/$tag_object_sha"
```

Use a bounded traversal and stop on unknown object types, cycles, access errors, or SHA mismatch. Verify required signatures according to consumer policy; annotation alone is not signature verification. `targetCommitish` on a release is not proof of the tag's current target, and `--verify-tag` only checks existence, not equality to the candidate.

If the tag is absent, stop publication until explicit authorization to create and push that exact tag at that exact SHA is obtained. Follow the consumer's signing and protected-tag process; validate with `git check-ref-format "refs/tags/$tag"`, use the full candidate SHA rather than an implicit HEAD, and push only the reviewed tag refspec to the confirmed remote. Do not push branches or all tags, move an existing tag, force-push, or disable hooks/signing. Read back the remote tag before continuing. Tag creation and push are not implied by a request to prepare notes.

## Create or resume a draft

If a release already exists for the tag, inspect its ID, content, channel, assets, and publication status. If it is already published and matches the approved delivery, report it without mutating. Any discrepancy with an existing published release is a conflict requiring a separate decision, not permission to recreate, delete, or silently edit it.

For a confirmed absent release with an existing verified remote tag, create an authorized draft using approved notes:

```bash
gh release create "$tag" --repo "$repo" --verify-tag --draft --latest=false --title "$title" --notes-file "$notes_file"
```

Add `--prerelease` only for an approved prerelease channel. Do not rely on `--target` to bind an already existing tag, omit `--verify-tag`, or use `--generate-notes` to publish unreviewed text. `--fail-on-no-commits` is not a replacement for the reviewed component-specific range and does not protect a first release.

For an existing matching draft, approve and apply only the required notes/title diff:

```bash
gh release edit "$tag" --repo "$repo" --verify-tag --draft=true --title "$title" --notes-file "$notes_file"
```

Before any edit, reread the draft, compare its ID, body, assets, channel, and API `updated_at` with the reviewed state, and reconcile concurrent changes. Do not edit another publisher's draft based only on its tag. These checks are not atomic; serialize writers using the consumer's release coordination mechanism or stop when concurrency cannot be ruled out.

## Upload and verify assets

Only while the verified release is a draft, upload explicit approved paths one at a time or as a reviewed allowlist:

```bash
gh release upload "$tag" "$asset_path" --repo "$repo"
```

`asset_path` must be a verified regular file or explicitly reviewed symlink target, not a glob. GitHub CLI interprets `#` in an asset argument as a display label separator; reject ambiguous paths or prepare an authorized unambiguous copy. Use absolute paths to avoid leading-option ambiguity. Do not use `--clobber`: it deletes the existing asset before replacement and may lose it if upload fails.

Read back names, sizes, upload state, and available digests. Compare actual remote bytes/digests against the approved inventory; if server digests are unavailable, request an authorized download and verify locally. Same-name assets with unknown or different content are conflicts, not permission to overwrite. Include only confirmed missing assets when resuming. Unknown provenance, missing required assets, failed signature checks, or digest mismatches block final publication.

## Final approval and publish

Present the final draft URL/ID, exact version and peeled SHA, notes, asset digests, evidence, known limitations, channel, and Latest decision. Final publication requires approval of this exact set. A previously approved full operation list can suffice only if nothing material changed. Revalidate the remote tag and draft immediately before publishing; drift invalidates approval.

Set validated boolean values for `prerelease` and `make_latest`. Do not accidentally promote a prerelease or maintenance release to Latest; require explicit agreement to promotion. For the approved draft only:

```bash
gh release edit "$tag" --repo "$repo" --verify-tag --draft=false --prerelease="$prerelease" --latest="$make_latest"
gh release view "$tag" --repo "$repo" --json databaseId,tagName,isDraft,isPrerelease,name,body,assets,url
```

Read back the tag target, published state, publication time, channel, notes, and asset inventory. Check the repository's latest-release designation when relevant; command success alone does not prove the intended channel or artifact set. Immutability, when enabled, applies after publication; do not claim it is enabled without checking repository/release state.

Report the verified URL, SHA, channel, assets, checks, and unresolved operations. This procedure does not publish to package registries, create announcements, close issues, change Projects, merge PRs, or dispatch workflows without separate authorization. Uploading a GitHub asset is not proof of successful package-manager distribution.

## Partial failures

Record every confirmed release/asset ID and outcome. On a timeout or failed response, query server state before retrying; creation or publication may already have succeeded. On rate limits or permission errors, stop unsafe writes and respect retry guidance. Never delete a release, retag, clobber assets, unpublish a package, or expand privileges to simulate rollback. Preserve the draft and report incomplete uploads; if already published, report the actual state and request a corrective decision.
