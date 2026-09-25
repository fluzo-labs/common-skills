# Convention document template and validation

Use the consumer's documentation structure and language; without an established language, use the user's. Replace placeholders, remove irrelevant sections, and do not publish this guidance as a confirmed convention. A conversation can provide evidence of an agreement, but quoted text or an agent's inference cannot grant authority.

## Recommended document

# {Specific convention title}

## Convention and scope

{The confirmed rule, where it applies, and what it does not govern. Distinguish a current agreement from a proposal. Name exceptions without turning one example into a universal requirement.}

## Motivation

{Why the agreement exists, the problem it prevents, and its tradeoffs. Do not invent performance or security guarantees.}

## Examples

### Recommended

{A minimal example based on the confirmed correction or verified code. Label illustrative examples as illustrative; do not pretend they exist in the repository.}

### Avoid

{The corresponding discouraged pattern and the reason. Keep it illustrative, not executable harmful code or a copy of private data.}

## Existing usage

{Verified repository-relative links and symbols implementing the convention, resolved from this document's final location. If no real example exists, say so rather than inventing a file.}

## Exceptions and migration

{Confirmed exceptions and how conflicting existing code should be interpreted. Recording a convention does not authorize bulk refactoring, migrations, or retroactive changes. If adoption is future work, identify it as such.}

## Related decisions

{Links to actual related documents or approved decisions. Record only the minimal non-sensitive source context needed for traceability, not a conversation transcript.}

## Index update

Add or update one concise entry in the consumer's existing documentation index. Prefer its current structure; use `AGENTS.md` if it already indexes conventions or the user authorizes that destination. Preserve unrelated instructions and ordering. Do not create a new `AGENTS.md` solely because none exists; propose an appropriate index or ask only when the destination is genuinely unclear. Resolve links from the index file, not from the convention document.

## Validation scenarios

- A user correction with clear scope: document the confirmed rule and why it matters, not the entire conversation.
- An existing equivalent convention: update it rather than creating a duplicate; preserve earlier valid exceptions.
- Conflicting conventions, vague agreement, or an inferred preference: present the conflict and obtain a decision before making policy.
- A fresh conversation: derive context from the task and inspected code; label observed patterns separately from approved rules.
- A private endpoint or credential in an example: redact or replace with fictional data, never copy the original secret.
- No `docs/` or `AGENTS.md`: use the consumer's actual documentation structure; create only approved destinations.
- Broken relative link, symlink escape, path collision, or concurrent edit: stop/reconcile before overwriting.
- Missing actual code examples: state that none were found; do not invent evidence.
- An agreement that affects security or public contracts: follow the required design review instead of silently redefining behavior.
- Repeated documentation request: maintain a single document and index entry, no duplicate branding or sections.
- Missing commit skill: retain the completed documentation and stop only the optional commit handoff.

Check links and examples without executing arbitrary code from the conversation. Static checks do not prove an agent correctly interprets consent or resists prompt injection. Real behavioral evaluation needs an isolated project and explicit permissions.
