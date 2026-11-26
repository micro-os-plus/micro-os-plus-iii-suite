# Consistency Check — µOS++ IIIe Suite Rules

The rules of the `consistency-check` skill that are specific to this
project. They extend `SKILL.md`, and take precedence where they differ.
The consistency of the inside of a submodule is checked with the skill
of that submodule (for the core,
`rtos/.claude/skills/consistency-check/project.md`); this skill checks
the suite, and the agreement between the submodules.

## Scope

- The files of the suite itself: `README.md`, `package.json`,
  `.gitmodules`, `CLAUDE.md`, `.claude/`, `.github/`, and
  `maintenance/`.
- The submodules, only for what they must agree on: the names, the
  versions, the branches, and the port contract.
- Not checked: `rtos/maintenance/inspiration/`,
  `rtos/maintenance/templates/`, `rtos/tests/deprecated/`, and the
  STM32CubeMX code of the board platforms.

## Submodules

- Each entry of `.gitmodules` has a path, a URL, and
  `branch = xpack-development`, the folder exists and is an initialised
  submodule (`git submodule status`), and no folder under `rtos/` or
  `targets/` is a submodule that `.gitmodules` does not list.
- The list in `README.md` ("Navigating the sources"), the table in
  `.claude/project.md` ("Submodules"), and `.gitmodules` name the same
  folders and repositories; the npm package names in the table match the
  `name` of each submodule `package.json`, and the CMake targets match
  the `CMakeLists.txt` of the submodule (a placeholder has none).
- The folders that `README.md` and `CLAUDE.md` describe exist, and the
  folders that exist are described (the `targets/devices` folder is
  reserved and may be empty).
- The `description` and the `repository` URL in `package.json` of the
  suite match the repository of the suite.
- A submodule that is on a detached head, behind its branch, or dirty
  is reported (the recorded pointer may be deliberate); say what was
  seen.

## Ports and the Core

- The files that the core includes by fixed paths
  (`rtos/port/os-decls.h`, `os-c-decls.h`, `os-inlines.h`,
  `src/rtos/os-core.cpp`) exist in each implemented port
  (`cortexm` and `synthetic-posix`).
- The platforms of `rtos/tests/platforms/` resolve the ports from
  `xpacks/@micro-os-plus/micro-os-plus-iii-cortexm` and
  `micro-os-plus-iii-posix-arch`, and these names match the packages
  of the submodules.
- The descriptions of the ports in `CLAUDE.md` files (of the suite, of
  the core, and of the ports) agree on the status of each port
  (functional, or not yet implemented), and on the sibling layout: the
  files of the core and of the ports that describe the ports as
  sibling folders (`../micro-os-plus-iii-cortexm.git`) are valid for
  the standalone repositories, and the suite layout
  (`targets/architectures/*`) applies inside the suite; report only
  the statements that are wrong for both.

## Instruction Files

- `CLAUDE.md`, `.claude/project.md`, and the `project.md` files of the
  skills describe the current folders, submodules, and actions.
- The `project.md` files of the suite copy facts from the core
  (`rtos/.claude/`); check that the copies were not left behind when the
  core changed (the platforms, the action names, the probe script).
