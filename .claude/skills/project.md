# Project Facts for the Skills — µOS++ IIIe Suite

This file is owned by the project; it was created once from the
template `npm-packages-helper/templates/first-time/.claude/skills/project-liquid.md`, and it is never regenerated.

The skills in `.github/skills/` read the facts of the project from
`.github/skills/project-defaults.md` (generated, with the defaults for
the organisation and the project) and from this file. Add below, under
the same heading, only what differs from the default: the text of a
heading extends the default, or overrides it where the two disagree (a
heading that says "None", or "Not applicable", removes the default). A
heading that is absent keeps the default. The headings are: Tests,
Probes, Generated Files, Files Not to Commit, Review
Documents, Breaking Changes, Documentation Updates, and Paths
(see `.github/skills/README.md`).

A skill folder may also have its own `project.md`, for the rules of that
skill only; it takes precedence over this file.

`CLAUDE.md` and `.claude/project.md` describe the project (a
superproject of Git submodules, the port contract, the build); read them
first. The headings below give only what the skills need from them, and
what differs from the defaults.

## Scope of the Suite

The suite has no sources of its own; see "Repository layout" and
"Working in the submodules" in `.claude/project.md`. A review of a
file under a submodule follows the skills of **that submodule**, with
its `project.md` files; the skills of the suite are for the files of
the suite and for reviews that cross the submodules (the port contract,
the consistency of the suite).

- The default scope, in this order, unless the user names a part:
  1. `rtos/` and the implemented ports (`cortexm` and
     `synthetic-posix`), the ports read as the implementations of the
     contract declared in `rtos/include/cmsis-plus/rtos/os-decls.h`;
  2. the files of the suite itself (`README.md`, `package.json`,
     `.gitmodules`, `maintenance/`, `.claude/`, `.github/`).
- The placeholder ports (`aarch32`, `aarch64`, `riscv`) have no code;
  check their metadata only.
- Not reviewed: `rtos/maintenance/inspiration/`,
  `rtos/maintenance/templates/`, `rtos/tests/deprecated/`, and the
  STM32CubeMX code of the board platforms
  (`rtos/tests/platforms/*/*cubemx/`). The `maintenance/` folders of
  the submodules are not part of the runtime; review them only for the
  files of the suite itself.

## Architecture Ports

The ports and the contract are described in `.claude/project.md`
("Submodules" and "The port contract"). A review that touches
`namespace port`, the `rtos/port/*.h` includes, or the scheduler,
thread, or clock internals reads the port sources, and states which
versions were read: the submodule commits (`git submodule status`), or
the packages that the test build used, under
`rtos/tests/build/<config>/xpacks/@micro-os-plus/` (links to the
submodules, or packages from the xPacks store; say which). The `smp`
branch of the `rtos` repository holds the experimental SMP scheduler;
compare with it when a change concerns the lists, the clocks, or the
critical sections.

## Tests

This overrides the default. The commands, the linking of the
submodules, and the platforms are in `.claude/project.md` ("Build and
test") and in `rtos/.claude/skills/project.md`; what to run:

- after minor changes, the native tests with clang
  (`test-native-cmake-clang`);
- after major changes (the scheduler, the synchronisation objects, the
  clocks, the startup, the C API, the port contract, or a port),
  `test-selected`;
- for wide changes, `test-latest`;
- a change limited to the Cortex-M port needs only the QEMU tests, and
  one limited to the POSIX port only the native tests;
- state in the report whether the run used the linked submodules or the
  packages of the xPacks store;
- the physical boards are run only after the user confirms that the
  board is connected and powered;
- the suite itself has no tests; a change to a maintenance script is
  checked by running it (`update-submodules.mjs` with `--dry-run`).

## Probes

This overrides the default: the sources cannot be compiled alone, they
need a port, the application configuration, and the platform files. Use
the probe script of the core, from the root of the suite, with a test
build folder of `rtos/tests` (see "Probes" in
`rtos/.claude/skills/project.md`):

```sh
node rtos/.claude/skills/review-and-fix/scripts/build-probe.mjs \
  rtos/tests/build/native-cmake-clang-debug <scratch>/probe/p1.cpp
```

The native port is non-preemptive; a race that depends on preemption by
the tick must be probed on QEMU.

## Generated Files

This overrides the default:

- The suite has no `xcdl-package.jsonc`, no generated `CMakeLists.txt`,
  and no `meson.build`; the `xcdl-export` script in `maintenance/scripts/`
  comes from the common template and has nothing to export. The
  generated files of each submodule are described in its own
  `.claude/skills/project.md`.
- The common files of the suite (the skills, `.gitignore`, `.npmignore`,
  `.github/`, `.vscode/settings.json`, `maintenance/config/`, and the
  scripts in `maintenance/scripts/`) come from the `npm-packages-helper`
  templates, and are regenerated with `npm run generate-top-commons`;
  do not edit the generated copies. The generated skill files are
  read-only; the `project.md` files are owned by the project.
- The `.github/skills` folder is a link to `.claude/skills`.

## Files Not to Commit

- The `*.tgz` packages left by `npm pack`, `node_modules/`, the
  `build/` folders of the submodules, and `.DS_Store`.
- The files that the user is editing at the same time; never include
  them in a commit or a stash unless the user says so.
- A change inside a submodule is never committed from the suite: the
  suite records only the new commit of the submodule, in a separate
  commit that moves the pointers.
- A submodule that shows as modified (`-dirty`, or a pointer to a
  different commit) because of the work in progress is not staged
  unless the user says so.

## Review Documents

The reports of the reviews of a submodule are written in that
submodule, in its `maintenance/docs/` folder, in the format of its own
skills (for the core, `rtos/maintenance/docs/CODE-REVIEW-1.md` has an
older format; see `rtos/.claude/skills/project.md`). A review that
crosses the submodules (for example, of the port contract) is written in
`maintenance/docs/` of the suite (create the folder when needed), in the
format of `.github/skills/review-common.md`, with the numbering
independent of the reports of the submodules.

## Breaking Changes

In addition to the default, a breaking change is anything that breaks
the combination of a core and a port released separately:

- the port contract (the `os::rtos::port` declarations and the
  `rtos/port/*.h` files that the ports provide); it requires matching
  releases of the ports;
- the names of the packages and of the CMake targets, the layout of the
  installed packages, and the headers that the application supplies
  (`os-app-config.h`, `platform.h`);
- the structure of the suite (the folders of the submodules, since
  other projects and the website may refer to them).

The breaking changes of a component (the C API, the configuration
macros, the observable behaviour of the RTOS) are described in the
`project.md` of that component.

## Documentation Updates

This overrides the default ("The project has no website"):

- The suite has no reference documentation of its own. The reference
  pages (<https://micro-os-plus.github.io/reference/>) are generated by
  Doxygen from the comments of the core, and the user manual
  (<https://micro-os-plus.github.io/user-manual/>) is in the website
  repository; when a change contradicts them, tell the user, and do not
  edit them from here.
- Update by hand: `README.md` of the suite (the list of the submodules,
  in step with `.gitmodules`, and the installation example), the table
  of the submodules in `.claude/project.md`, and `CLAUDE.md` when the
  structure changes. The `CHANGELOG.md`, the READMEs, and the
  documentation comments of a component are updated in that component.

## Paths

- The paths are relative to the root of the suite
  (`rtos/include/cmsis-plus/rtos/os-thread.h`,
  `targets/architectures/cortexm/src/rtos/os-core.cpp`). When a report
  is about a single submodule, the paths may be relative to the root of
  that submodule, once the report says so.
