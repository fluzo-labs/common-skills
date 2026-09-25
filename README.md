# common-skills

English | [Español](README.es.md)

A collection of reusable skills for coding agents. Each skill provides instructions for a specific task and can be used in other projects without depending on this entire repository.

The collection covers issue refinement, local planning, single-phase execution, GitHub delivery review, conventional commits, and release preparation with changelogs. See the [catalog](skills/README.md) to choose a skill and review its dependencies.

## Documentation

The following supporting guides are currently in Spanish:

- [Usage guide](docs/USAGE.md): choosing skills, inputs/outputs, examples, and approvals.
- [Proposed distribution](docs/DISTRIBUTION.md): SHA-pinned project copies, verifiable bundles, and a future installer plan. Distinguishes existing capabilities from proposals.
- [Catalog](skills/README.md): instructions and dependencies for all six skills.

## Structure

```text
skills/
  README.md                 Catalog of published skills
  <name>/
    SKILL.md                Skill instructions and metadata
    references/             Additional documentation, optional
    scripts/                Automation, optional
    assets/                 Templates and other resources, optional
templates/
  skill/
    SKILL.md                Template for new skills
docs/
  USAGE.md                  Usage, examples, and authorization boundaries
  DISTRIBUTION.md           Proposed secure distribution design
AGENTS.md                   Repository maintenance instructions
.editorconfig               Basic file formatting
.gitignore                  Local state, secrets, and temporary files
LICENSE                     MIT license
```

The `<name>/` folders and their resources are illustrative: create them when adding a skill. There is no application, shared runtime dependency set, or build step.

## Create a skill

From the repository root, replace `my-skill` with a lowercase, hyphenated name:

```bash
test ! -e skills/my-skill && cp -R templates/skill skills/my-skill
```

1. Edit `skills/my-skill/SKILL.md`: replace the name, description, and all guidance text.
2. Match the frontmatter `name` to the folder name. The `description` must explain when to activate the skill, not just what it contains.
3. Write a concrete procedure, verifiable requirements, and validation criteria. Specify each command's working directory.
4. Add resources only when needed and link them from `SKILL.md`. Resource paths are relative to the skill folder, not the consumer project.
5. Test the skill in a representative project and add it to the [catalog](skills/README.md), including external dependencies.

Keep `SKILL.md` focused on decisions and the main workflow. Move lengthy details into `references/` and explain when to consult them to avoid loading unnecessary context.

## Reuse in other projects

### Install with `npx skills`

From the consumer project root, with Node.js/npm (`npx`), Git, and network access:

```bash
npx skills add fluzo-labs/common-skills --list
npx skills add fluzo-labs/common-skills --skill '*' --agent universal --copy
```

The first command lists available skills; the second installs all six into `.agents/skills/`, a path discovered by Crush. To select just one, replace `'*'` with its name. For the Crush-specific `.crush/skills/` destination, use `--agent crush`, bearing in mind that `.crush/` may be ignored by Git. Do not install the same skill into both destinations.

We do not need to publish our own npm package: the external CLI installs from this public repository. However, `npx` may download and execute the installer; `--copy` neither pins versions nor guarantees security. These examples preserve confirmation prompts and do not install globally.

See the [usage guide](docs/USAGE.md) for individual/global installation, other agents, checks, and reviewed updates. Commands are documented according to the official CLI; an isolated installation of this collection has not yet been tested.

### Copy a skill

To copy the commit skill, run from the consumer project root, replacing the path to this collection:

```bash
mkdir -p .agents/skills
test ! -e .agents/skills/git-conventional-commit && cp -R /absolute/path/common-skills/skills/git-conventional-commit .agents/skills/git-conventional-commit
```

Then ask the agent to commit the specific changes you want to record. Asking only for a proposed message does not authorize changes to the index or history. The skill does not push or load remote content; its conventions and validation scenarios are included in the folder.

Copy the entire folder, not just `SKILL.md`: it may depend on included resources. The check prevents replacing an existing folder. A copy does not receive automatic updates; review differences before updating and preserve consumer project customizations.

Crush discovers `.agents/skills`, `.crush/skills`, `.claude/skills`, and `.cursor/skills` by default. For other agents, check their paths and compatibility; using `SKILL.md` does not guarantee they interpret the same extensions.

### Load the collection directly with Crush

In the consumer project's `crushrc` or your global Crush configuration, add an absolute path to the `skills/` folder in your local checkout:

```bash
option skill-path /absolute/path/common-skills/skills
```

`option` is a Crush configuration function, not a command to run directly in a terminal. Do not point it at the repository root or `templates/`, to avoid loading the template as a real skill. This method uses shared files directly: any change to the collection affects projects loading it.

No provider, key, or permission configuration is required to distribute this collection. Review instructions and scripts before allowing their execution in another project.

## Workflow

| Step | Skill | Outcome and boundary |
| --- | --- | --- |
| Refine | [issue-refine-github](skills/issue-refine-github/SKILL.md) | Proposes keeping, expanding, or splitting an issue; does not create children by default. |
| Plan | [plan-create](skills/plan-create/SKILL.md) | Proposes phases and saves a local plan only with approved content and destination. |
| Execute | [plan-execute](skills/plan-execute/SKILL.md) | Implements one approved phase, verifies it, and stops for review. |
| Prepare delivery | [delivery-review-github](skills/delivery-review-github/SKILL.md) | Proposes an issue update and PR with evidence; publishes only what is authorized. |
| Record changes | [git-conventional-commit](skills/git-conventional-commit/SKILL.md) | Creates a local commit only upon explicit request. |
| Prepare release | [release-prepare-github](skills/release-prepare-github/SKILL.md) | Separates changelog, version/evidence preparation, and approved release publication; does not create implicit tags. |

You do not need to follow every step. A small issue can be executed without splitting it; a task without GitHub can be planned and executed locally; an existing delivery can be reviewed without a plan created by these skills. Install each needed folder using the same copy procedure above, replacing the skill name.

Integrations are optional and transmit data, not permissions: source and revision, goal, scope, phase, contracts, acceptance criteria, dependencies, verification, and approval evidence. No skill loads files from sibling folders or installs other skills. Refinement can use a `plan-create` draft, but GitHub remains the source of truth for states and dependencies; no second local board is maintained.

Example requests to the agent:

- "Check whether this issue needs refinement; do not modify GitHub."
- "Propose a local plan for this task; show it to me before saving it."
- "Implement only phase P1 of this approved plan."
- "Prepare the issue update and PR for this delivery without publishing them."
- "Generate a draft changelog between this tag and this SHA without writing or publishing."
- "Prepare the next release with a version proposal, evidence, and checksums; do not create tags."

### Activation and approvals

The new workflow skills allow model selection (`disable-model-invocation: false`) and manual invocation (`user-invocable: true`). Selection depends on the agent: there is no GitHub watcher, hook, or guarantee of automatic execution. Invoking a skill never authorizes all its operations.

To reinforce the workflow, you can incorporate this rule into the consumer project's instructions after reviewing it:

> Before implementing an issue, assess scope, contracts, dependencies, and existing work. If refinement is needed, propose changes without mutating GitHub; use `issue-refine-github` if available. Implement one approved phase and present evidence before continuing or publishing.

Other projects' configuration is not modified automatically. Approving a plan does not approve commits, pushes, PR publication, issue closure, or Project changes. Joint approval of an explicit list of operations and text can authorize publication without repeating questions for every command.

### Backlog adaptation and safety

Skills discover the consumer's governance, language, baseline, templates, owning repositories, relationships, and Project fields. They do not hardcode organizations or IDs and do not confuse parent/child relationships with dependencies. They preserve unique import markers in the original issue and do not turn every task into an epic.

Skill conventions and templates are local; GitHub operations do require network access and authentication through `gh`. Skills are not downloaded and scopes are not expanded automatically. The installed version's help is checked before using options such as `--parent`; a REST alternative is available for linking children, and host or permission limitations are reported.

Proposal mode does not execute write commands or `gh pr create --dry-run`, which may push. Publication revalidates revisions, detects existing resources, and preserves partial results to avoid duplicates. These instructions are neither a sandbox nor a guarantee of resistance to prompt injection.

### Releases and changelogs

`release-prepare-github` has three modes: changelog only, release preparation, and explicitly approved publication. It pins the component, release line, and candidate SHA rather than simply taking the highest version tag. It handles first releases, incomplete history, empty ranges, prereleases, and workspaces while preserving historical entries and curated notes.

Generation uses Git and, optionally, an already installed git-cliff with reviewed local configuration and `--offline --no-exec`. It does not install tools or download configuration. Missing git-cliff does not prevent drafting a changelog from history.

Preparation includes a manifest of revisions, criteria, tests, artifacts, checksums, and limitations. A version/changelog PR can go through `delivery-review-github`, but merging it does not authorize the release. Publication verifies the remote tag's actual SHA, works with a draft first, and checks assets before publishing; `--verify-tag` prevents implicit tag creation but does not replace comparing the SHA. Tagging, pushing, publication, Latest, registries, and workflows require their corresponding authorization.

## Conventions

- Skills, references, examples, the authoring template, and this README are in English. [README.es.md](README.es.md) is the Spanish version; other maintenance documentation remains in Spanish.
- Keep both README versions synchronized when changing installation instructions or workflows.
- The language of skill instructions does not dictate their outputs: plans, issues, and messages follow consumer project rules or, if absent, the user's language.
- One concrete responsibility per skill; avoid duplicated generic instructions.
- Self-contained skills: no personal paths, sibling-folder dependencies, or references to internal files of this repository.
- Declare required tools and versions in each skill; do not assume the consumer shares your environment.
- Respect consumer project context and instructions. Skills must not bypass permissions or impose changes unrelated to their task.
- Do not include secrets or real data. Use fictional examples and environment variables where appropriate.
- Preserve applicable license files and attribution notices when redistributing content.

## Checks

There is currently no configured test suite, linter, or CI. For documentation changes, run from the repository root:

```bash
git diff --check
git status --short
```

`git diff --check` does not inspect new untracked files: review those too before adding them. Check that frontmatter contains `name` and `description`, names match folder names, relative links exist, and no template guidance remains.

If a skill includes scripts, document and run its specific checks; there is no common test command for the collection. Also verify that the folder still works when copied outside this repository.

For the commit skill, use its [validation scenarios](skills/git-conventional-commit/references/validation.md) in temporary repositories. The others include local scenarios in their `SKILL.md` or references: [refinement](skills/issue-refine-github/references/refinement.md), [execution](skills/plan-execute/references/execution.md), and [delivery](skills/delivery-review-github/references/delivery-template.md). The release skill includes a [validation matrix](skills/release-prepare-github/references/validation.md) for ranges, evidence, tags, assets, and partial failures.

Check links after copying each folder separately and verify proposal mode, approvals, outdated sources, and partial recovery using simulated responses. Do not run mutation tests against a real backlog. Distinguish command syntax and mechanics from real GitHub tests and evaluation of agent behavior with untrusted content.

## Local files

`.gitignore` excludes the entire `.crush/` directory, `.env` files, editor temporary files, and Python environments/caches. It allows `.env.example` and `.env.*.example`, which must never contain real credentials. Published skills belong in `skills/`, not `.crush/skills/`.

## License

[MIT](LICENSE).
