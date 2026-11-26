# Code Review — µOS++ IIIe Suite Rules

The rules of the `code-review` skill that are specific to this project.
They extend `SKILL.md`, and take precedence where they differ. Read
`CLAUDE.md`, `.claude/project.md`, and `.github/skills/project.md` first.

## Scope

- The suite is a superproject: its sources are in the submodules. A
  code review of a file under a submodule follows the rules of that
  submodule: read its `CLAUDE.md`, and its
  `.claude/skills/code-review/project.md` (for the core,
  `rtos/.claude/skills/code-review/project.md`, which holds the
  project checks: errors, interrupt context, critical sections,
  instrumentation, the C API, port delegation, configuration macros,
  portability, language, warnings, and tests). Apply them to the
  ports as well, with the paths prefixed by the folder of the
  submodule.
- The default scope is `rtos/include/`, `rtos/src/`,
  `rtos/tests/sources/`, and the `include/`, `src/`, and `tests/` of
  `targets/architectures/cortexm` and
  `targets/architectures/synthetic-posix`, with the exclusions of
  `rtos/.claude/skills/code-review/project.md` (work-in-progress
  drivers, `tests/deprecated/`, the STM32CubeMX code, and every
  `maintenance/` folder, except for the files of the suite itself).
- The placeholder ports (`aarch32`, `aarch64`, `riscv`) have no code;
  do not review them, except for their metadata.
- The files of the suite (`maintenance/scripts/`, `package.json`,
  `.gitmodules`, `README.md`) are reviewed as in `SKILL.md`, but the
  scripts generated from the templates are not (report a defect to the
  `npm-packages-helper` project, under "Not reported").

## Port Contract

Review the ports against the contract declared in
`rtos/include/cmsis-plus/rtos/os-decls.h` (`namespace os::rtos::port`):

- every declaration of the contract is implemented by each implemented
  port, with the same signature and semantics; and a port does not
  implement or define what the core does not declare;
- the guards follow the portability rule of `.claude/project.md` (no
  toolchain or ABI macros; `#error` where an architecture is not
  implemented);
- state in the report which commit of each submodule was read.

Report a problem found in a submodule against that submodule (the
report names it in the first column of the item); the review document is
written as described under "Review Documents" in
`.github/skills/project.md`.
