# Project Rules — µOS++ IIIe Suite

This file is owned by the project; it was created from the template
`npm-packages-helper/templates/first-time/.claude/project-liquid.md`
and is never regenerated.

`CLAUDE.md` imports this file. Put here only the rules for this
project; the rules common to all projects are generated into
`CLAUDE.md` and are changed in the `npm-packages-helper` templates.
Where a rule here contradicts `CLAUDE.md`, the rule here applies.

## Project overview

The **µOS++ IIIe Suite** (`@micro-os-plus/iii-suite`) is a
**superproject**: it contains no library code of its own. It groups, as
Git submodules, the repositories that together form the third edition of
µOS++, so that they can be inspected, searched, and tested together. Each
submodule is a separate project, with its own repository, its own
`CLAUDE.md` and `.claude/` files, its own issues, and its own releases.

All submodules track the `xpack-development` branch (see `.gitmodules`).

The suite is not distributed as a package, and applications do not
depend on it; they install only the packages they need with `xpm`: the
RTOS package and **exactly one** architecture package.

Note: the IVe edition (split into separate libraries) is a different
project; this suite is IIIe only.

## Submodules

| Folder | Repository | npm package | CMake target | Status |
| --- | --- | --- | --- | --- |
| `rtos` | `micro-os-plus-iii` | `@micro-os-plus/micro-os-plus-iii` | `micro-os-plus::iii` | the portable core; functional |
| `targets/architectures/cortexm` | `micro-os-plus-iii-cortexm` | `@micro-os-plus/micro-os-plus-iii-cortexm` | `micro-os-plus::iii-cortexm` | Arm Cortex-M port; functional |
| `targets/architectures/synthetic-posix` | `micro-os-plus-iii-posix-arch` | `@micro-os-plus/micro-os-plus-iii-posix-arch` | `micro-os-plus::iii-posix-arch` | synthetic POSIX port (macOS and GNU/Linux); functional |
| `targets/architectures/aarch32` | `micro-os-plus-iii-aarch32` | `@micro-os-plus/micro-os-plus-iii-aarch32` | - | placeholder, not implemented |
| `targets/architectures/aarch64` | `micro-os-plus-iii-aarch64` | `@micro-os-plus/micro-os-plus-iii-aarch64` | - | placeholder, not implemented |
| `targets/architectures/riscv` | `micro-os-plus-iii-riscv` | `@micro-os-plus/micro-os-plus-iii-riscv` | - | placeholder, not implemented |

The `targets/devices` folder is reserved for the device (board and
SoC) support; it is not yet populated, and is not a submodule.

The folder names of the submodules differ from the names of the
repositories; in the other files of the project the repositories are
named with the `-iii-` form (for example `micro-os-plus-iii-cortexm`),
and in this suite the paths are used.

## Repository layout

- `rtos/` - the portable core (a submodule); its own rules are in
  `rtos/.claude/project.md` and `rtos/CLAUDE.md`
- `targets/architectures/*` - the architecture ports (submodules)
- `maintenance/` - the maintenance resources of the suite itself,
  which are not part of the µOS++ runtime: `config/` (formatter
  configuration), `scripts/` (generated common scripts, including
  `update-submodules.mjs` and `update-changelog.mjs`), and
  `build-helper/`
- `README.md`, `package.json`, `LICENSE` - the suite itself
- there is no `src/`, `include/`, or `tests/` folder at the top level,
  and no `xcdl-package.jsonc`; the generic statements of `CLAUDE.md`
  about these folders apply to the submodules, not to the suite

## Working in the submodules

- Each submodule is a full repository, and every change to it is made
  and committed **inside that submodule**, following its own
  `CLAUDE.md`, its own skills, and its own commit conventions. Before
  working in a submodule, read its `CLAUDE.md` (the rules of the suite
  do not replace them). A file under `rtos/` is governed by
  `rtos/CLAUDE.md`, not by this one.
- The suite records only the **commit** of each submodule. After the
  submodules are committed, the updated pointers are committed in the
  suite, separately (a commit that only moves pointers, with a subject
  such as `update submodules`); never mix the pointer updates with
  changes to the files of the suite.
- Do not commit, push, or tag in a submodule without the approval of
  the user, as for the suite.
- A submodule may be on a detached head or behind its branch. Check
  `git submodule status` and `git -C <path> branch --show-current`
  before reading or editing, and say which commit was read.
- To bring all the submodules to the latest commit of their branch,
  preview with `npm run update-submodules-check`, then
  `npm run update-submodules`; the script leaves submodules with
  uncommitted changes, or with local commits, untouched, and never
  commits the suite.
- The suite has its own issues, for the cross-project work (the
  structure of the suite, the maintenance scripts, the documentation of
  the whole). The issues of a component go to the repository of that
  component.

## The port contract

The core never builds on its own; every application, and every test
configuration, links the core and exactly one architecture port. The
contract between them is declared in the `os::rtos::port` namespace in
`rtos/include/cmsis-plus/rtos/os-decls.h`, and each port provides:

- `include/cmsis-plus/rtos/port/os-decls.h`, `os-c-decls.h`, and
  `os-inlines.h`;
- `src/rtos/os-core.cpp` (the non-inline port code);
- for the POSIX port, also `src/diag/trace-posix.cpp`.

In the suite, the ports are the sibling folders of `rtos/` under
`targets/architectures/`, so a change to the contract is made in the
same working tree as its consumers. The rule is:

- when changing anything in `namespace port` in the core, in the
  `rtos/port/*.h` includes, or in the semantics of the scheduler,
  thread, or clock internals, **check and update all the implemented
  ports** (`cortexm` and `synthetic-posix`);
- when changing a port, check that it still satisfies the contract as
  declared in the core;
- the changes are made as separate commits, one in each repository,
  and the suite pointers are updated last, to a set of commits that
  were tested together. Say in each commit message which commits of the
  other repositories it needs.

The `aarch32`, `aarch64`, and `riscv` submodules are placeholders
(only a README, a `package.json`, and a licence). Do not add untested
code to them, and do not count them as ports when checking a contract
change; where the core needs an architecture-specific implementation
for them, it uses an explicit
`#error "... not yet implemented for this architecture"`.

## Build and test

There is no build at the top level of the suite. The tests of the whole
are the tests of the core, in `rtos/tests`, which are driven with `xpm`
(Node.js >= 20, xpm >= 0.20.8) and which link a port:

```sh
xpm run test -C rtos/tests            # native, system compiler
xpm run test-selected -C rtos/tests   # native, QEMU M0, M7F, M33
xpm run test-latest -C rtos/tests     # all platforms
```

The ports are not tested by their own repositories: the Cortex-M port
is exercised by the QEMU platforms of `rtos/tests`, the POSIX port by the
`native` platform. The ports have only trivial smoke tests under their
own `tests/` folders, which nothing builds.

By default `xpm` installs the ports as read-only packages from the
central xPacks store. To test the core with the ports of the **suite
working tree**, register the submodules as linkable packages (once),
and link them into the test configurations:

```sh
xpm link -C targets/architectures/synthetic-posix
xpm link -C targets/architectures/cortexm

xpm run link-deps-all -C rtos/tests
# or, for one configuration
xpm run link-deps --config native-cmake-clang-debug -C rtos/tests
```

Do not use `git-clone-deps` of `rtos/tests`: it clones fresh copies
under `~/Work/...`, which are not the submodules of the suite. When a
test fails in CMake before compiling, check first that the links point
to the submodules, and the branches of the submodules.

The details (the platforms, the test applications, the configuration
names, the physical boards, which tests to run for which change) are in
`rtos/.claude/project.md` and `rtos/.claude/skills/project.md`; the
runs take minutes, so run them in the background when they may exceed
the tool timeout.

## Formatting and conventions

- Each submodule has its own formatter configuration; format only the
  files you touch, with the configuration of the submodule that owns
  them. The coding conventions of the C++ code (naming, errors,
  portability, Doxygen, licence headers) are those in
  `rtos/.claude/project.md`, and apply to the ports as well.
- The files of the suite itself (the Markdown files, the scripts in
  `maintenance/scripts/`) are formatted with
  `npm run format-md` and the other `format-*` actions of
  `package.json`; the scripts are generated from the
  `npm-packages-helper` templates, do not edit them by hand.
- Use British English in comments and documentation, and "folder"
  rather than "directory".

## Branches and releases

- `master` is the branch of the suite; the submodules follow their
  `xpack-development` branches (the stable releases are on their
  `xpack` branches).
- The suite has no releases of its own. Each component is released from
  its own repository, with its own procedure (for the core, see
  `rtos/maintenance/docs/README-MAINTAINER.md`); a release of the core
  that changes the port contract requires matching releases of the
  ports, in the order: core, then ports.
- Keep the table of the submodules in this file and the list in
  `README.md` in step with `.gitmodules`.
