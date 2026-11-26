# Design Review — µOS++ IIIe Suite Rules

The rules of the `design-review` skill that are specific to this
project. They extend `SKILL.md`, and take precedence where they differ.
Read `CLAUDE.md`, `.claude/project.md`, and `.github/skills/project.md`
first.

## Scope

The suite is a superproject; the design under review is that of the
components, and of the way they fit together. The rules of the core
(`rtos/.claude/skills/design-review/project.md`) apply to `rtos/`
unchanged, with the paths prefixed by `rtos/`: the order of the parts,
the exclusions, the restrictions (the deliberate decisions), the
header self-containment check, the newcomer tasks and the common
mistakes, and the embedded-target measurements. Read them, and do not
report as problems the decisions that they list.

Unless the user names a part, review in this order:

1. the core, as in the rules of the core;
2. the port contract, as seen from the ports: the declarations of
   `os::rtos::port` in `rtos/include/cmsis-plus/rtos/os-decls.h`, and
   their implementations in `targets/architectures/cortexm` and
   `targets/architectures/synthetic-posix` (the headers
   `rtos/port/os-decls.h`, `os-c-decls.h`, and `os-inlines.h`, and
   `src/rtos/os-core.cpp`);
3. the structure of the suite (the submodules, their names, and the
   instructions to the users).

Not reviewed: the placeholder ports (`aarch32`, `aarch64`, `riscv`),
which have no design yet, and every `maintenance/` folder.

## The Port Contract

This is the subject that only the suite can review, since it needs the
core and the ports together:

- is the contract complete and minimal: does the core call anything
  that the contract does not declare, and does a port provide anything
  that the core does not use;
- do both ports implement the same semantics (the critical sections,
  the interrupt state, the context creation and switch, the clock
  tick, the idle wait), and where they differ, is the difference
  forced by the architecture, or an accident; the native port is
  cooperative (the `SIGALRM` signal stands for the interrupts) and the
  Cortex-M port is preemptive, so a design that works only on the
  first is a finding;
- is the contract usable for the architectures that are not yet
  implemented (Armv7-A and Armv8-A cores, AArch64, RISC-V): look for
  assumptions that a Cortex-M or a POSIX port happens to satisfy (the
  stack grows downwards, the interrupt mask is a single register, the
  tick is a single timer, the context switch runs in a handler);
- the ports are not reviewed in themselves, only as implementations of
  the contract; a defect found in a port is listed under "Not
  reported", for the repository of the port.

## Probes

Build the probes as described under "Probes" in
`.github/skills/project.md`, with the test builds of `rtos/tests`;
state whether the ports of the build are the submodules of the suite
(linked) or the packages of the xPacks store. Probe the contract
questions on both ports: natively, and on QEMU for the questions that
depend on the target or on preemption.
