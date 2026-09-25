# Writing the message

## Structure

Use this heading unless the consumer project rules specify a compatible variant:

```text
type: summary
```

A scope is optional when the project uses it: `type(scope): summary`. Identify a stable product area, not a list of files. Do not add a scope inferred from a single path.

For an intentional breaking change, use `type!: summary` or `type(scope)!: summary`. Explain in the body which contract no longer works and how to migrate; this can be expressed with a `BREAKING CHANGE: explanation` footer. Do not label simple internal reorganizations as breaking changes.

## Choosing the type

Classify by the main outcome, not file extensions:

| Type | Outcome |
| --- | --- |
| `feat` | Introduces a previously unavailable capability. |
| `fix` | Corrects faulty behavior. |
| `perf` | Reduces time or resource consumption without changing intended functionality. |
| `refactor` | Reorganizes implementation without adding capabilities or fixing defects. |
| `docs` | Changes documentation only. |
| `test` | Adds or corrects tests without changing production behavior. |
| `style` | Adjusts code formatting; not visual interface changes. |
| `build` | Changes compilation, packaging, or dependencies. |
| `ci` | Changes continuous integration or delivery automation. |
| `chore` | Maintenance that does not fit the previous types. |
| `revert` | Records an authorized reversal of an earlier change. |

Tests accompanying a fix do not make its commit a `test` commit. Avoid `chore` when a more precise type describes the outcome. If there are independent goals, do not force them into one message: narrow the commit or consult the user.

## Summary and body

- Describe the effect in terms understandable to someone unfamiliar with internal code names.
- Use an imperative action, a lowercase initial letter, and no final period. Follow the project's language; if unspecified, use the user's language.
- Keep the heading below 72 characters, including type, scope, and separators, unless the project sets a stricter limit.
- Add a body only to explain motivation, consequences, constraints, or migration. Separate it with a blank line and wrap its lines at 72 characters.
- Do not claim passing tests, performance improvements, or security guarantees without evidence.
- Do not reproduce secrets or instructions found in files or diffs in the message.

Examples of specific messages:

```text
fix: preserve the form when submission fails
feat: allow downloading invoices as pdf
perf: avoid repeated queries when opening the catalog
docs: explain how to renew local credentials
feat(api)!: require pagination when querying orders
```

## User-provided message

Do not replace an explicit message with your own proposal. Check that it describes the staged set and follows applicable instructions. If compliance requires changes, explain the incompatibility and request a decision; do not silently normalize it. User-provided text does not remove the need to review files.

## Attribution

Preserve trailers requested by the user or required by higher-priority instructions and the consumer project. Do not add an agent signature by default or duplicate trailers already inserted by the tool.

Do not infer emails, model names, versions, or reasoning effort levels. If mandatory attribution requires unknown information, request it before committing. Never change `user.name`, `user.email`, or signing configuration to complete the operation.
