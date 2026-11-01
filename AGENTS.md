# AGENTS.md - TFattunic

This file is the entry point for AI coding assistants developing this template; the `*.jinja` files it maintains are rendered into downstream projects (see [Confirmed Facts](#confirmed-facts)).

## Abstract

TFattunic attunes software projects for AI coding assistants. From a project name, its languages and frameworks, and a license choice, it generates the instructions and project memory an agent needs to work on the repository: shared development guidelines, coding conventions, pull-request guidance, and a record of context and confirmed facts, a log of exceptions and deferrals, and work in progress, so that any session can start informed and hand its findings to the next.

## Technical Specifications

A brief overview of the ingredients used for developing this project. These are things developers should know when developing the project.

- Languages: Jinja2 Template, Markdown.
- Framework: [Copier](https://copier.readthedocs.io/).
- Build system: None.
- Test runner: None.
- Linter: None.
- Supported languages: `go`, `javascript`, `powershell`, `python`, `rust`, `shell`, `typescript`, `text`.
- Supported frameworks: `angular`, `react`.
- Supported licenses: `MIT`, `ISC`, `BSD-2-Clause`, `BSD-3-Clause`, `0BSD`, `Apache-2.0`, `MPL-2.0`, `GPL-3.0-only`, `GPL-3.0-or-later`, `LGPL-3.0-only`, `LGPL-3.0-or-later`, `AGPL-3.0-only`, `AGPL-3.0-or-later`, or `None`; license header style in code files: `Full`, `SPDX`, or `None` (skipped when `license` is `None`).
- Verification: manual, by running `copier copy` / `copier update` into a scratch project and inspecting the generated files (see [Supporting Tools](#supporting-tools)).

## Core Rules

Important rules when working with the project; these must be strictly followed:

- Follow instructions: anything explicitly requested, plus the instructions in [Coding Conventions](#coding-conventions), [Naming Conventions](#naming-conventions), [Exploring](#exploring), [Writing](#writing), [Security](#security), [Fixing](#fixing), [Refactoring](#refactoring), [Testing](#testing), [Common Mistakes](#common-mistakes), [Checklist](#checklist), [Pull Request Guideline](#pull-request-guideline), and [Pull Request Checklist](#pull-request-checklist).
- Resolve conflicts by priority: user requests, local idiomatic style (unless unsafe or broken), the rules in this file.
- Read rule strength from its verb: a rule stated as a plain command (Keep, Use, Write, ...) is mandatory, and so are Always and Must; Never and Do not (don't) are mandatory prohibitions; Prefer and Avoid are defaults, deviate only with a stated reason.
- Plan first: plan by default, unless the user explicitly says to skip it; when the change spans more than one file, share the plan and wait for the user's approval before the first edit.
- Ask when in doubt: if you are unsure about an instruction, need help, or have a better solution, ask: "Do you actually need X?", "Does Y cover it?", "Do you mean Z?", etc.
- Understand the problem: read the task and the code it touches, trace the real flow end to end.
- Follow the simple design priorities: tests pass, no duplicated knowledge, intent expressed, fewest elements.
- Guard against common loopholes: stop and reassess when you catch yourself thinking any of the rationalizations in [Anti-Loopholes](#anti-loopholes).
- Distrust recall: verify every Copier setting and Jinja filter against the installed Copier version, every EditorConfig property against its spec, and every API or tool behavior a fragment names against its official docs.
- Confirm irreversible actions: ask before deleting files outside the change, rewriting history, force-pushing, publishing, or moving a published tag.
- Verify every change: every changed behavior is checked by something that runs; without a test harness, as in this repository, the smallest check that runs (a `copier copy` render into a scratch directory) stands in for each test a rule asks for.
- Commit and push only on request: never create a commit or push unless the user asks for it.
- Report fully: say what ran and what didn't, what was skipped or assumed, and what was found along the way; never present unverified success or a stub or placeholder as finished; name anything that could not run with its remaining risk.

## Layout

List of important folders, files and their roles in the project:

```text
├── .agents/                              # shared AI agent instruction templates (`*.md.jinja`), copied into generated projects
│   ├── attunic-protocol.md.jinja         # defines the `.attunic/` project memory: files, read order, write rules
│   ├── coding-conventions.md.jinja       # formatting, naming, package, type, function, comment, error, and concurrency conventions; plus per-language and per-framework sections
│   ├── development-guideline.md.jinja    # exploring/writing/security/fixing/refactoring/testing rules, common mistakes, coding checklist; plus per-language and per-framework sections
│   ├── license-header.md.jinja           # license header to put at the top of every new code file
│   ├── memory-template.md.jinja          # skeleton for `workstream.md`, copied when an audit or cleanup workstream starts
│   ├── project-structure.md.jinja        # standard project structures of languages and frameworks, kept for reference
│   └── pull-request-guideline.md.jinja   # guideline and checklist run before opening/updating a PR
├── .attunic/                             # project memory templates, pre-seeded into generated projects
│   ├── context.md.jinja                  # renders `context.md`: abstract, specs, layout, layer dependencies with deliberate exceptions, tools, confirmed facts
│   └── memory.md.jinja                   # renders `memory.md`: append-only log of exceptions, deferrals, and open questions; `workstream.md` created on demand (gitignored)
├── frameworks/<framework>/               # framework fragments included by root templates for every selected framework; included, never copied
│   ├── coding-conventions.md.jinja       # framework-specific coding conventions
│   ├── development-guideline.md.jinja    # framework-specific development guideline
│   └── project-structure.md.jinja        # framework-specific standard project structure
├── languages/<language>/                 # language fragments included by root templates for every selected language (`typescript` also pulls in `javascript`'s rules); included, never copied
│   ├── .editorconfig.jinja               # language-specific editor formatting rules
│   ├── .gitignore.jinja                  # language-specific ignore patterns
│   ├── coding-conventions.md.jinja       # language-specific coding conventions
│   ├── development-guideline.md.jinja    # language-specific development guideline
│   └── project-structure.md.jinja        # language-specific standard project structure
├── licenses/                             # license text and notice fragments (not copied), included by the license renderers below and `.agents/license-header.md.jinja`
├── .copier-answers.yml.jinja             # emits `.copier-answers.yml` in generated projects; never hand-edited (replayed by `copier update`)
├── .editorconfig.jinja                   # generic editor formatting rules; includes the fragment of every selected language
├── .gitignore.jinja                      # generic ignore rules; includes the fragment of every selected language
├── AGENTS.md                             # agents' entry point of this repo itself; maintained manually, not copied
├── AGENTS.md.jinja                       # renders `AGENTS.md`, the agents' entry point; this flat `AGENTS.md` is maintained manually in parallel
├── CLAUDE.md                             # this repo's pointer to `AGENTS.md`; not copied
├── CLAUDE.md.jinja                       # renders `CLAUDE.md`, which points to `AGENTS.md`
├── COPYING.jinja                         # renders `COPYING` for GPL-3.0-only/or-later, the GPL part of LGPL-3.0-only/or-later, AGPL-3.0-only/or-later
├── COPYING.LESSER.jinja                  # renders `COPYING.LESSER` with the LGPL text, additionally for LGPL-3.0-only/or-later
├── LICENSE                               # MIT license text of this template; not copied
├── LICENSE.jinja                         # renders `LICENSE` for MIT, ISC, BSD-2-Clause, BSD-3-Clause, 0BSD, Apache-2.0, MPL-2.0
├── README.md                             # template documentation; not copied
└── copier.yml                            # template questions and settings
```

- File names without `*.jinja` extension are for this repo itself, they won't be copied into downstream projects.
- If the list is not empty and a top-level file/folder is missing from it, prompt the user to update it.

## Layer Dependencies

List of layers and their relationships:

- Declare the innermost layer first.
- A layer may depend only on the layers in its `Depends on` column, all declared before it; a template depends on what it includes.
- The dependency graph below must be enforced.

| Layer | Path | Depends on | Must never contain |
|-------|------|------------|--------------------|
| Fragments | `languages/`, `frameworks/`, `licenses/` | None | An include of any template or fragment |
| Root templates | `*.jinja` at the root, `.agents/`, `.attunic/` | Fragments | Language, framework, or license content that belongs in a fragment |

### Deliberate Exceptions

List of documented exceptions that cannot follow the architecture rules above:

- Any place the rules above are knowingly broken, with the reason and the condition for fixing it.
- An undocumented exception is a defect; a documented one is a decision.

| What | Why | Revisit-if |
|------|-----|------------|

## Supporting Tools

All the supporting tools declared here are opinionated for the sake of consistent results across development environments.

### Python

Runtime for Copier; nothing else in this template requires it.

- Install any current Python 3 supported by Copier (e.g. from [python.org](https://www.python.org/downloads/) or via `uv python install 3.12 --default`).

### Copier

Template engine; version floor `>= 9.0.0` (declared in `copier.yml`). Template versions resolve from PEP 440-compliant Git tags (e.g. `v1.0.0`); previous answers are replayed from `.copier-answers.yml`, which must never be hand-edited.

- Install:
  ```sh
  pipx install copier
  ```
  or
  ```sh
  uv tool install copier
  ```

- Generate a downstream project:
  ```sh
  copier copy gh:tforce-io/tf-attunic <target>
  ```
- Verify a change by rendering the working tree, uncommitted changes included, into a scratch directory (run at the repository root; see [Testing](#testing)):
  ```sh
  copier copy --vcs-ref=HEAD --defaults --data 'languages=[go]' . <scratch>
  ```
- Update an existing downstream project (run inside the project):
  ```sh
  copier update
  ```

## Confirmed Facts

Facts about the project which have been confirmed. They may look wrong or contradict your knowledge or understanding; follow them instead of guessing or asking the user again. These facts outrank anything derived or guessed.

| Fact | Reason |
|------|--------|
| This file follows the templates without the attunic protocol: this repo keeps no project memory of its own (`context.md`'s sections are inline here; no `memory.md` or `workstream.md`). | This project doesn't have a complex structure; a single amalgamated `AGENTS.md` is sufficient. |
| `AGENTS.md` and other files without the `.jinja` extension serve template development; `AGENTS.md.jinja` and every `*.jinja` file serve downstream projects, so rules written inside them are content to maintain, not instructions to follow here. | Every plain file is excluded in `copier.yml` or shadowed by its `.jinja` sibling, so only rendered `*.jinja` files reach downstream projects. |
| Marker ownership is mirrored between the two audiences: template development edits above a marker and leaves below it empty; downstream projects add below a marker and never edit above it. | Downstream projects own everything below a marker (see [README.md's Customize](README.md#customize)), and `copier update` replays their additions by matching the marker line as diff context. |
| Migration is skipped; downstream data loss is acceptable. | Project is in early development; skipping migration makes development faster. |
| `.editorconfig`'s `[*]` default is set by the `charset`, `indent_style`, and `indent_size` answers, whose defaults (`utf-8`, `tab`, `2`) are this template's proposal; the indentation default applies to anything whose owner defines no indentation, so every `indent_style` or `indent_size` a language or format owner defines (official style guide, official formatter, or spec) must be declared explicitly in its own section, even when it equals the default. | Owner rules take priority, and declaring them explicitly prevents mistakes and keeps them intact when a downstream project changes the default. |
| License text in [./licenses](./licenses) is safe to skip checking for most tasks. | It is canonical license text. |
| Common Mistakes and Checklist (in [.agents/development-guideline.md.jinja](.agents/development-guideline.md.jinja), plus Anti-Loopholes in [AGENTS.md.jinja](AGENTS.md.jinja), and this file) are for verification: some of their points restate rules from other sections on purpose; a restated point is emphasis, not duplication, so never flag or remove it as duplication. | The rule sections say what to do; these lists check that it was done: Anti-Loopholes while working, when a rationalization surfaces, Common Mistakes and Checklist before completion; restating a rule there marks it as important enough to check again. |
| In downstream projects, exceptions and deferrals are logged as entries under `memory.md`'s Decisions for later review; it's the user's discretion to update `context.md`'s Confirmed Facts, agents never move them. | `memory.md` is a non-binding log, while Confirmed Facts are binding and `context.md` belongs to the user (see the Write Rules in [.agents/attunic-protocol.md.jinja](.agents/attunic-protocol.md.jinja)). |
| The `Layout` sections of the language/framework project-structure fragments are reference layouts: they may name tool-specific files (e.g. `uv.lock`, `clippy.toml`, `deny.toml`, each package manager's lockfile); every other part of the language/framework guides stays tool-neutral. | A reference layout lists files a project may already have so agents recognize them; listing one doesn't pick the tool, which stays the downstream project's choice. |

## Coding Conventions

Template authoring conventions for this repository:

- End each section of every Markdown template (`*.md.jinja`, fragments included) with a `<!-- Project-specific -->` or `<!-- Project-specific / <Section> -->` marker; content above the marker is template-owned, content below is project-owned.
- Use a single plain `# Project-specific` comment as the last line of `.editorconfig.jinja` and `.gitignore.jinja`, below the included fragments; their language fragments carry no marker.
- In a section that ends with a fenced block, place the marker inside the fence, before its closing line, when the block is a layout tree, so project additions extend the tree; otherwise place it below the closing fence.
- Keep marker text byte-identical across template versions: Copier matches the marker line as diff context, so a whitespace or punctuation change can break the anchor and produce spurious conflicts on `copier update`.
- Write template content only above a marker; leave everything below it empty for downstream projects, which add their project-specific rules there (see [README.md's Customize](README.md#customize)).
- Keep [.agents/coding-conventions.md.jinja](.agents/coding-conventions.md.jinja) and [.agents/development-guideline.md.jinja](.agents/development-guideline.md.jinja) to generic principles: language/framework agnostic and tool-neutral; put language and framework specifics only in the `languages/` and `frameworks/` fragments.
- Keep the language/framework guides tool-neutral; it's up to the downstream project to pick the tools and declare them in the context.
- Keep the language/framework guides version-neutral: state rules that hold across the language's or framework's maintained versions, with no "since vN" claims; where versions differ, defer to what the project already uses.
- Include all sections declared in [.agents/coding-conventions.md.jinja](.agents/coding-conventions.md.jinja) in the language/framework Coding Conventions, all sections but Checklist declared in [.agents/development-guideline.md.jinja](.agents/development-guideline.md.jinja) in the language/framework Development Guidelines, and the Guideline, Layout, and Layer Dependencies sections, in that order, in the language/framework Project Structures, even when empty; add new sections after the shared ones.
- Order the rules in each section by strength: mandatory, default, mandatory prohibitions, default prohibitions; keep the existing order within each group.
- Use ASCII punctuation and straight quotes by default; use non-ASCII characters only where the content requires them (names, quoted text, symbols with no ASCII form, box-drawing in layout trees); keep encoding UTF-8 without a BOM.
- End every file with a single final newline; strip trailing blank lines.
- Separate headings from body text with exactly one blank line; never indent paragraphs or headings except intentional block quotes.
- Never add a marker to a file without sections: `CLAUDE.md.jinja`, `.copier-answers.yml.jinja`, the license renderers, and the `licenses/` fragments; there it would be rendered into the generated file as content.
- Never move or duplicate a marker: Copier anchors downstream additions to the marker line, so either change can misplace them or produce conflicts on `copier update`; treat it as a contract break (see [Writing](#writing)).
- Never delete or reword comments a tool reads downstream, such as the `SPDX-License-Identifier` line emitted by [licenses/spdx-notice.jinja](licenses/spdx-notice.jinja); markers follow the rules above.

## Naming Conventions

- Use one word per concept across templates and docs; never one word for two concepts (e.g. *fragment* always means an included, never-copied file under `languages/`, `frameworks/`, or `licenses/`).
- Name markers uniquely after the section they close: `<!-- Project-specific / <Section> -->`.
- Name a fragment's markers `<!-- Project-specific / <Name>-<Section> -->`, where `<Name>` is the language or framework name as in its heading (e.g. `Go-Errors`, `React-Hooks`); spell `<Section>` as its heading does, spaces included.
- Name template files after the generated file with the `.jinja` suffix (e.g. `AGENTS.md.jinja` renders `AGENTS.md`).
- Name language folders with the lowercase codes listed in the `languages` choices in `copier.yml`: `go`, `javascript`, `powershell`, `python`, `rust`, `shell`, `typescript`, `text`.
- Name framework folders with the lowercase codes listed in the `frameworks` choices in `copier.yml`: `angular`, `react`.
- Name license fragments `<license-id>.jinja` and `<license-id>-notice.jinja` (e.g. `mit.jinja`, `mit-notice.jinja`).
- Never number or version a name instead of replacing the original (`coding-conventions-v2.md.jinja`, `_new`, `_copy`).

## Exploring

Reuse before writing new content; go through these in order and stop at the first that applies:

- Does it already exist in this codebase? Reuse the fragment, include pattern, or wording that's already here; don't rewrite it.
- Does a closely matching template or fragment that needs adaptation exist in this codebase? Report it and propose extending it before writing anything new.
- Does a Jinja built-in filter, test, or tag already do this? Use it.
- Does a Copier setting cover it (e.g. a question's `when`, `choices`, or `validator`, or `_exclude`)? Use it.
- Does a filter bundled with Copier solve it (`jinja2_ansible_filters`, e.g. `regex_search`, `to_nice_yaml`)? Use it.
- Would a well-maintained Jinja extension solve it? Propose it with its name, version, and license before adding it; its license must be compatible with the project's, and it is an unsafe feature (see [Security](#security)).

## Writing

When writing templates, follow these rules:

- Write the minimum that satisfies the task; never cut safety: answer validation and security; see [Security](#security).
- Make targeted edits, never whole-file regeneration; every changed line must trace to the request.
- Keep one job per unit at every scale (bullet, section, template); if describing its job needs "and", split it.
- Find every usage of a template variable, marker, section anchor, or file you change, by text search, including `README.md` and this file; make sure a rename or behavior change doesn't introduce a new bug or leave a bug half-fixed.
- Report unrelated findings instead of fixing them.
- Remove what your change orphaned (unused fragments, variables, sections, files).
- Update the docs that describe changed behavior (`README.md`, this file) in the same change.
- Keep this file in sync with the templates it is an instance of (`AGENTS.md.jinja` and the `.agents/` and `.attunic/` templates): when a change alters a rule or section this file mirrors, apply the same change here in the same change, adapted to template development, or report why it doesn't apply.
- Treat `copier.yml` question names, marker text and placement, and generated file paths as contracts with downstream projects; ask before breaking one, and call out every break, since migration is skipped (see [Confirmed Facts](#confirmed-facts)).
- Never hand-edit rendered output to verify a change; change the template and render again.

## Security

When writing templates or verifying changes, follow these rules:

- Validate answers where they enter: give a question a `validator` when an empty, malformed, or out-of-range answer would break a generated file.
- Prefer allowlists over denylists in validators (e.g. the `project_name` pattern).
- Never render an answer into YAML or a file path as raw text; constrain it with `choices` or a `validator`, or emit it through an escaping filter such as `to_nice_yaml`.
- Never add an unsafe Copier feature (`_jinja_extensions`, `_tasks`, `_migrations`) without the user's agreement: it runs code on the downstream user's machine, and Copier refuses it unless that user trusts the template (`--trust`).
- Never hardcode secrets or commit them to the repository.

## Fixing

When fixing issues, follow these rules:

- Fix the root cause, not just the symptom.
- Reproduce the bug first with a render that shows the wrong output, then render the same answers again to confirm the fix.
- Search for the same defect elsewhere, including sibling fragments, and report other occurrences; fix them only when the task covers them.

## Refactoring

When restructuring templates without changing their output, follow these rules:

- Render the affected templates before and after with the same answers; the generated files must not change.
- For deduplication, only merge true duplication (copies that must always change together, owned by the same actor); merging accidental lookalikes is harder to undo than leaving them apart.
- Run cleanup or audit work spanning more than one batch as a workstream: record the baseline before the first change, keep one intent per batch, verify after each batch, and keep the batch list current in the plan shared with the user.

## Testing

When verifying a change with `copier copy` renders (see [Supporting Tools](#supporting-tools)), follow these rules:

- Render from the working tree with `--vcs-ref=HEAD`; without it, Copier renders the latest tag and the change is not in the output.
- Check at least three expectations in the generated files for each changed behavior, spread across renders as needed, and make sure at least one does not hold in a render of the unchanged templates.
- Cover answer boundaries, not just the defaults: `license` `None`, `license_header` `None`, no frameworks, `typescript` without `javascript`, several languages at once, and an invalid answer each changed validator must reject.
- Keep renders reproducible: pass every answer with `--data` (or `--data-file`) and `--defaults`, into a fresh scratch directory.
- Inspect the generated files, not just the template diff.
- Never drop an answer set because its render fails or looks wrong: fix the template, or report the conflict and stop.

## Common Mistakes

Check your diff against these failure patterns; each must be fixed before completion:

- Hallucinated API: a Copier setting, Jinja filter, EditorConfig property, or fragment-named API that the installed version or its official docs don't have => verify it, replace or remove.
- Unverified dependency: a new Jinja extension without checking its registry name, version, license, or whether a Jinja built-in or a filter bundled with Copier does the job => confirm the exact name/version (an invented name may be attacker-registered) and that its license is compatible with the project's; prefer what Copier bundles.
- Context loss: a stale or partial read that contradicts existing content, restores deleted content, or drops a marker or validator in a rewrite => re-read the target files, the templates that include them, and `README.md`.
- Scope creep: changed lines that trace to no request such as drive-by renames, reformatting, dependency bumps => revert them and report instead.
- Duplicated implementation: a rule, fragment, or include pattern paralleling one that exists => search before writing, extend the original, merge only true duplication, never content owned by different actors.
- Wrong-file gravity: a rule added to whatever file was open, a god file growing, a new file at the repository root or current directory => place by role per [Layout](#layout).
- Phantom success: "should work now" with nothing run; a stub, placeholder, or TODO presented as done => run the render and quote the result, name what did not run, and state plainly what remains.
- Silent structure drift: a fragment including a root template or a sibling fragment, or fragment content hard-coded into a root template, unless listed in [Deliberate Exceptions](#deliberate-exceptions); a rule edited in its wrong home; template content below a marker; names off [Naming Conventions](#naming-conventions) => check every changed include against [Layer Dependencies](#layer-dependencies), keep includes as the only wiring, one home per rule, names per convention.
- Missing validation: an answer accepted unchecked, including empty or out-of-range values, where it would break a generated file => add a `validator` to its question in `copier.yml`.
- Missing error handling: an undefined variable rendering as empty text (Copier's default `jinja2.Undefined` doesn't fail), such as a misspelled answer name => check every name against `copier.yml` and inspect the render for gaps.
- Edited generated file: a hand edit to rendered output => revert it, change the template, and render again.
- Stale docs: `README.md` or this file still describing the old behavior => update them in the same change.
- Unannounced breaking change: a `copier.yml` question name, marker text or placement, or generated file path changed without the user's agreement => restore compatibility or ask, then call out the break.

## Anti-Loopholes

Stop and reassess when you catch yourself thinking:

| Rationalization | Reality |
|-----------------|---------|
| "I will clean this up while I am here." | Unrelated work unless the task needs it; report it instead. |
| "A framework will make this cleaner." | A dependency is a cost, and a one-sided commitment; prove the need. |
| "This abstraction will help later." | Later requirements can pay for later abstraction. |
| "The code is bad, so a rewrite is cleaner." | Rewrites need scope, tests, migration risk control. |
| "There are no tests, so verification is impossible." | Use the best available check and report remaining risk. |
| "I will put it here for now." | "For now" placements become permanent. Place it correctly once. |
| "The user asked for cleanup, so everything is in scope." | Workstream mode has a protocol: baseline, batches, verification; see [Refactoring](#refactoring). |
| "Clean code means following this document over local style." | Local, idiomatic style wins unless unsafe or broken. |
| "It is only one include; the include structure still basically holds." | One wrong-direction include is the violation. Structure is a rule, not a tendency. |
| "Splitting it into services will decouple it." | A process boundary is not a boundary; shared data still couples through it. |
| "These two blocks are identical, so I will extract a helper." | Only if they must always change together. Check who owns each one. |
| "We will clean it up after the deadline." | The pressure that created the shortcut never abates. |

## Checklist

Before considering any coding task complete, verify the following to ensure nothing is missed during implementation:

- [ ] The task's requirements are fulfilled; acceptance criteria (if any) are verified against the generated files, not assumed from the template diff.
- [ ] Changed templates render from the working tree into a scratch directory with the answer sets they need, and the generated files are inspected (see [Testing](#testing)).
- [ ] Changed files follow project convention (see [Coding Conventions](#coding-conventions) and [Naming Conventions](#naming-conventions)).
- [ ] The complete final diff has been read line by line; every change is intentional and understood.
- [ ] Changes are reviewed for security requirements (see [Security](#security)).
- [ ] Changes are reviewed for common mistakes (see [Common Mistakes](#common-mistakes)).
- [ ] Every usage of changed template variables, markers, section anchors, and files is checked; no rename or behavior change left half-applied (see [Writing](#writing)).
- [ ] No unused fragments or variables, debug output, or commented-out blocks left behind.
- [ ] Template rules and sections this file mirrors are in sync with their changes, or the reason they don't apply is reported (see [Writing](#writing)).
- [ ] New or moved top-level files and folders are reflected in [Layout](#layout) (prompt the user per its rule).
- [ ] Lasting choices and confirmed facts are recorded in [Confirmed Facts](#confirmed-facts), each once the user explicitly confirms it (unconfirmed ones are named in the report).
- [ ] Every applicable item above was verified by actually running the check; anything not run is explicitly reported, not claimed.

## Pull Request Guideline

- Keep one logical change per pull request; split unrelated work into separate ones.
- Title the pull request with a short imperative summary of the change (e.g. `Add Rust coding conventions`).
- Follow the repository's pull request template when it has one; otherwise write the description in these parts, writing "None" for an empty one:
  - Motivation: the problem or request, with linked issues/tickets.
  - Changes: what changed, grouped by intent.
  - Verification: the `copier copy` renders that ran, with their answers and what was inspected; the checks that did not run, and why.
  - Breaking changes: every broken downstream contract (see [Writing](#writing)), and what downstream projects must do.
  - Follow-ups: findings reported but not fixed.

## Pull Request Checklist

Before opening or updating a pull request, verify:

- [ ] Development checklist is completed (see [Checklist](#checklist)).
- [ ] Commit messages are clear and describe the *why*, not just the *what*.
- [ ] PR is scoped to a single logical change.
- [ ] PR description explains the motivation and summarizes the change.
- [ ] Breaking changes to downstream contracts are called out explicitly (see [Writing](#writing)).
- [ ] Duplicated template content is justified.
- [ ] Any new dependency is justified.
- [ ] Linked issues/tickets are referenced, if applicable.
