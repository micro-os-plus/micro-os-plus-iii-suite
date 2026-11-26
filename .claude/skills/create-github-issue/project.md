# Create Issue — µOS++ IIIe Suite Rules

The rules of the `create-github-issue` skill that are specific to this
project. They extend `SKILL.md`, and take precedence where they differ.

## Where the Issue Goes

- The suite repository (`micro-os-plus/micro-os-plus-iii-suite`) is only
  for the cross-project work: the structure of the suite, the
  submodules, the maintenance scripts, and the documentation of the
  whole.
- A problem in the code, the tests, or the documentation of a component
  is opened in the repository of that component, with the rules of that
  repository (for the core, `rtos/.claude/skills/create-github-issue/`):
  `micro-os-plus/micro-os-plus-iii` for `rtos/`,
  `micro-os-plus/micro-os-plus-iii-cortexm` for the Cortex-M port,
  `micro-os-plus/micro-os-plus-iii-posix-arch` for the POSIX port, and
  similarly for the `aarch32`, `aarch64`, and `riscv` placeholders. Name
  the repository in the proposal before opening, and open it with
  `gh issue create --repo <owner>/<repository>`.
- A problem that needs changes in the core and in a port (a change to
  the port contract) is opened in the core, and names the ports that
  need matching changes; open a separate issue in each port repository
  that needs one, each referring to the first.

## Labels and Milestones

Use the labels and the milestones of the repository where the issue is
opened (`gh label list --repo ...`, `gh api repos/.../milestones`);
do not assume that those of the core exist in the others.

## Body

- Name the submodule, and the commit that was read (the submodule
  pointer of the suite, or the commit of the submodule).
- For a problem that depends on the port, name the port and the test
  configuration that shows it (for example
  `qemu-cortex-m0-cmake-gcc-debug`).
