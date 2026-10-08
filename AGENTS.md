# AGENTS.md - TFattunic

This file is the entry point for AI coding assistants working on this repository.

## Abstract

TFattunic attunes software projects for AI coding assistants. From a project name, its languages and frameworks, and a license choice, it generates the instructions and project memory an agent needs to work on the repository: shared development guidelines, coding conventions, pull-request guidance, and a record of context, settled decisions, and work in progress, so that any session can start informed and hand its findings to the next.

## Technical Specifications

A brief overview of the ingredients used for developing this project. These are things developers should know when developing the project.

- Languages: Jinja2 Template, Markdown.
- Framework: [Copier](https://copier.readthedocs.io/).
- Build system: None.
- Test runner: None.
- Linter: None.
- Supported languages: `go`, `typescript`, `text`.
- Supported frameworks: `angular`, `react`.
- Supported licenses: `MIT`, `ISC`, `BSD-2-Clause`, `BSD-3-Clause`, `0BSD`, `Apache-2.0`, `MPL-2.0`, `GPL-3.0-only`, `GPL-3.0-or-later`, `LGPL-3.0-only`, `LGPL-3.0-or-later`, `AGPL-3.0-only`, `AGPL-3.0-or-later`, or `None`; license header style in code files: `Full`, `SPDX`, or `None` (skipped when `license` is `None`).
- Verification: manual, by running `copier copy` / `copier update` into a scratch project and inspecting the generated files (see [Supporting Tools](#supporting-tools)).

## Core Rules

Important rules when working with the project; these must be strictly followed:

- Follow instructions: anything explicitly requested, plus the instructions in [Exploring](#exploring), [Writing](#writing), [Security](#security), [Fixing](#fixing), and [Testing](#testing); priority order: user requests, template.
- Plan first: plan by default, unless the user explicitly says to skip it.
- Ask when in doubt: if you are unsure about an instruction, need help, or have a better solution, feel free to ask questions: "Do you actually need X?", "Does Y cover it?", "Do you mean Z?", etc.
- Understand the problem: read the task and the code it touches, trace the real flow end to end.
- Follow the simple design priorities: tests pass, no duplicated knowledge, intent expressed, fewest elements.
- Avoid common loopholes: stop and reassess when you catch yourself thinking any of the rationalizations in [Anti-Loopholes](#anti-loopholes).
- Write clean code: safe and maintainable, per [Writing](#writing).
- Never trust memory: verify every API, function, option, and config key against this codebase and installed dependency versions.
- Report honestly: say what actually ran; never claim unverified success or present a stub or placeholder as finished; anything that could not run is named with its remaining risk.

## Layout

List of important folders, files and their roles in the project:

```text
├── .agents/                              # shared AI agent instruction templates (`*.md.jinja`), copied into generated projects
│   ├── attunic-protocol.md.jinja         # defines the `.attunic/` memory layer: files, read order, write rules
│   ├── coding-conventions.md.jinja       # naming, formatting, comment, and error conventions; plus per-language and per-framework sections
│   ├── development-guideline.md.jinja    # exploring/writing/security/fixing/testing rules, common mistakes, coding checklist
│   ├── license-header.md.jinja           # license header to put at the top of every code file
│   ├── memory-template.md.jinja          # skeleton for `wip.md`, copied when an audit starts
│   ├── project-structure.md.jinja        # standard project structures of languages and frameworks, kept for reference
│   └── pull-request-guideline.md.jinja   # guideline and checklist run before opening/updating a PR
├── .attunic/                             # project memory layer templates, pre-seeded into generated projects
│   ├── context.md.jinja                  # renders `context.md`: specs, layout, layer dependencies, tools, confirmed facts
│   └── memory.md.jinja                   # renders `memory.md`: append-only decisions and open questions; `wip.md` created on demand (gitignored)
├── frameworks/<framework>/               # framework fragments included by root templates for every selected framework; included, never copied
│   ├── coding-conventions.md.jinja       # framework-specific coding conventions
│   └── project-structure.md.jinja        # framework-specific standard project structure
├── languages/<language>/                 # language fragments included by root templates for every selected language; included, never copied
│   ├── .editorconfig.jinja               # language-specific editor formatting rules
│   ├── .gitignore.jinja                  # language-specific ignore patterns
│   ├── coding-conventions.md.jinja       # language-specific coding conventions
│   └── project-structure.md.jinja        # language-specific standard project structure
├── licenses/                             # license text and notice fragments (not copied), included by the license renderers below
├── .copier-answers.yml.jinja             # emits `.copier-answers.yml` in generated projects; never hand-edited (replayed by `copier update`)
├── .editorconfig.jinja                   # generic editor formatting rules; includes its language fragment
├── .gitignore.jinja                      # generic ignore rules; includes its language fragment
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

- File names without *.jinja extension are for this repo itself, they won't be copied into downstream projects.
- If a top-level file/folder is missing from the list, prompt the user to update it.

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

- Generate a downstream project (also the manual verification path: copy into a scratch directory and inspect the generated files):
  ```sh
  copier copy --trust gh:tforce-io/tf-attunic <target>
  ```
- Update an existing downstream project (run inside the project):
  ```sh
  copier update --trust
  ```

## Confirmed Facts

Facts about the project which have been confirmed; they outrank anything derived or guessed.

- Project is in early development, migration is skipped for faster development, downstream dataloss is acceptable.
- License text in `./licenses` are canonical text, it's safe to skip checking them for most tasks.

## Coding Convention

Template authoring conventions for this repository:

- Every template file ends each section with a `<!-- Project-specific -->` or `<!-- Project-specific / <Section> -->` marker; content above the marker is template-owned, content below is project-owned.
- Never add, edit, or delete anything above a marker; never move or duplicate a marker.
- Keep marker text byte-identical across template versions: Copier matches the marker line as diff context, so a whitespace or punctuation change can break the anchor and produce spurious conflicts on `copier update`.
- In `.editorconfig` and `.gitignore`, the marker is a plain `# Project-specific` comment.
- Project-specific rules supplement the template rules; add them below the marker of the section being overridden, or below the final `Project-specific` section's marker for general additions.

## Naming Convention

- Name markers uniquely after the section they close: `<!-- Project-specific / <Section> -->`.
- Name template files after the generated file with the `.jinja` suffix (e.g. `AGENTS.md.jinja` renders `AGENTS.md`).
- Name language folders with the lowercase codes listed in the `languages` choices in `copier.yml`: `go`, `typescript`, `text`.
- Name framework folders with the lowercase codes listed in the `frameworks` choices in `copier.yml`: `angular`, `react`.
- Name license fragments `<license-id>.jinja` and `<license-id>-notice.jinja` (e.g. `mit.jinja`, `mit-notice.jinja`).

## Exploring

When exploring for a solution to the task, try to reuse existing code following this priority order and stop at the first one that is satisfied; this step is needed to make the project easy to maintain and understandable in the long run:

- Does it already exist in this codebase? Reuse the helper, util, or pattern that's already here; don't rewrite it.
- Does the standard library already do this? Use it.
- Does a native platform feature cover it? Use it.
- Does an already-installed dependency solve it? Use it.
- Does a closely matching implementation that needs adaptation exist in this codebase? Report it.
- If none of the above applies, writing new code is fine.

## Writing

When writing code, follow these rules:

- Explore carefully before writing; make use of what is available in the codebase first; see [Exploring](#exploring).
- Write the minimum that satisfies the task; never cut safety: validation, error handling, security, accessibility; see [Security](#security).
- Make targeted edits, never whole-file regeneration; every changed line should trace to the request.
- Keep one job per unit at every scale (function, type, package); if describing its job needs "and", split it. Parsing, domain rules, persistence, external calls, presentation, and wiring stay in separate homes.
- Use the narrowest access modifier; keep the public API surface small.
- Grep every caller of the function/method you touch to make sure a signature or behavior change doesn't introduce a new bug or leave a bug half-fixed.
- Report unrelated findings; never do drive-by renames, reformats, or dependency bumps.
- Prefer small, focused functions and files; avoid large "god" packages or files.
- Prefer surrounding code's local, idiomatic style over general rules unless it is unsafe or broken.
- Prefer early exits for invalid or terminal cases over nested conditionals.
- Prefer allowlists over denylists.
- Remove what your change orphaned (unused code, imports, tests, files).
- When assigning values to object fields, follow the order of field declarations if possible.
- When renaming types, methods, functions, remember to check relevant tests.
- When moving files, use the source control move command to retain history.
- Avoid magic numbers or strings; use a named constant for the meaning, or attach an inline *why* comment.
- Avoid sibling variants (`_v2`, `_new`, `_final`, `_copy`); replace the original.
- Avoid unrelated code in `utils`/`helpers`/`common`; name the domain concept instead.
- Avoid unnecessary abstraction; don't introduce an interface until there are two or more real implementations.
- Never modify arguments of functions/methods as side effects; return values instead.

## Security

When writing code or verifying changes, follow these supplementary rules:

- Use parameterized queries and safe APIs.
- Keep authorization checks beside the operation they protect, or centralized in one enforced policy layer.
- Perform input validation at trust boundaries. Boundaries are external APIs, databases, file systems, clocks, queues, UI events, network calls, subprocesses, generated code.
- Prefer well-maintained standard libraries for crypto, parsing, auth, and serialization.
- Never log secrets, tokens, and sensitive data in error messages and logs.
- Never hardcode secrets or commit them to the repository; read them from the environment or a secret store.

## Fixing

When fixing issues, instructions of [Writing](#writing) apply, plus:

- Fix the root cause, not just the symptom.
- Write a test that reproduces the bug first, watch it fail, then make it pass.
- For refactoring, run the relevant tests before and after; behavior must not change.
- For deduplication, only merge true duplication (copies that must always change together); merging accidental lookalikes is harder to undo than leaving them apart.

## Testing

When writing tests, follow these rules:

- Write one concept per test; split a test whose name needs "and"; prefer behavior-focused names.
- Write at least three deterministic expectations, and make sure at least one fails on the untouched fixture.
- Keep the three parts of a test visibly distinct: arrange, act, assert; this is about code structure, don't place comments to distinguish these parts.
- Keep tests F.I.R.S.T.: fast, independent, repeatable, self-validating, timely.
- Keep results deterministic: no sleeps or timing guesses, no real network or filesystem call.
- Assert on outcomes, not implementation.
- Never weaken, skip, or delete failing tests: restore the test, fix the code, or report the conflict and stop.

## Common Mistakes

Check your diff against these failure patterns; each must be fixed before completion:

- Hallucinated API: a function, method, option, or config key not in this codebase or the installed version => verify at the installed version, replace or remove.
- Unverified dependency: a new package or import without checking its registry name, version, or whether an installed dependency does the job => confirm the exact name/version (an invented name may be attacker-registered); prefer what is installed.
- Context loss: a stale or partial read that contradicts existing code, restores deleted code, or drops error handling in a rewrite => re-read target files, callers, tests.
- Scope creep: changed lines that trace to no request such as drive-by renames, reformatting, dependency bumps => revert them and report instead.
- Duplicate implementation: a helper or sibling variant paralleling one that exists => search before writing, extend the original, merge only true duplication, never code owned by different actors.
- Wrong-file gravity: logic added to whatever file was open, a god file growing, a new file at the repository root or current directory => place by role per the project layout, see [Layout](#layout).
- Phantom success: "should work now" with nothing run; a stub, pass, placeholder, or demo value presented as done => run the check and quote the result, name what did not run, and state plainly what remains.
- Test weakening: an assertion loosened; a test skipped, deleted, or rewritten to match the bug, a snapshot re-accepted unread => restore the test and fix the code, or report the conflict and stop.
- Silent structure drift: a fragment including a root template or a sibling fragment; fragment content hard-coded into a root template; a rule edited in its wrong home; content above a marker; names off [Naming Convention](#naming-convention) => keep includes as the only wiring (root templates include fragments, never the reverse), one home per rule, names per convention.
- Missing validation: an input from a trust boundary accepted unchecked, including empty, null, or boundary values => validate at the boundary where the data enters.
- Missing error handling: a failing path swallowed or ignored, such as an unchecked error or an empty catch => handle it, wrap it, or propagate it explicitly.

## Anti-Loopholes

Stop and reassess when you catch yourself thinking:

| Rationalization | Reality |
| --- | --- |
| "I will clean this up while I am here." | Unrelated work unless the task needs it; report it instead. |
| "A framework will make this cleaner." | A dependency is a cost, and a one-sided commitment; prove the need. |
| "This abstraction will help later." | Later requirements can pay for later abstraction. |
| "The code is bad, so a rewrite is cleaner." | Rewrites need scope, tests, migration risk control. |
| "There are no tests, so verification is impossible." | Use the best available check and report remaining risk. |
| "I will put it here for now." | "For now" placements become permanent. Place it correctly once. |
| "The user asked for cleanup, so everything is in scope." | Campaign mode has a protocol: baseline, batches, wip, verification. |
| "Clean code means following this document over local style." | Local, idiomatic style wins unless unsafe or broken. |
| "It is only one include; the include structure still basically holds." | One wrong-direction include is the violation. Structure is a rule, not a tendency. |
| "Splitting it into services will decouple it." | A process boundary is not a boundary; shared data still couples through it. |
| "These two blocks are identical, so I will extract a helper." | Only if they must always change together. Check who owns each one. |
| "We will clean it up after the deadline." | The pressure that created the shortcut never abates. |

## Checklist

Before considering any coding task complete, please verify the following to ensure nothing is missed during implementation:

- [ ] The task's requirements are fulfilled; acceptance criteria (if any) are verified against real behavior, not assumed from passing tests.
- [ ] Changed code files follow project convention (see [Coding Convention](#coding-convention) and [Naming Convention](#naming-convention)).
- [ ] Changes are scoped to the task, unrelated fixes are reported, not bundled in.
- [ ] The complete final diff has been read line by line; every change is intentional and understood.
- [ ] Changes are reviewed for security requirements (see [Security](#security)).
- [ ] Changes are reviewed for common mistakes (see [Common Mistakes](#common-mistakes)).
- [ ] No secrets, tokens, or sensitive payloads in logs, error messages, or hardcoded values.
- [ ] No unused code, debug prints, or commented-out blocks left behind.
- [ ] New or moved top-level files and folders are reflected in [Layout](#layout) (prompt the user per its rule).
- [ ] Lasting choices and confirmed facts are recorded in [Confirmed Facts](#confirmed-facts).
- [ ] Every applicable item above was verified by actually running the check; anything not run is explicitly reported, not claimed.
