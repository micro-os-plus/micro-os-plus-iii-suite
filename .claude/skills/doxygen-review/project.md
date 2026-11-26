# Doxygen Review — µOS++ IIIe Suite Rules

The rules of the `doxygen-review` skill that are specific to this
project. They extend `SKILL.md`, and take precedence where they differ.

## Scope

- The suite has no Doxygen configuration and no documented sources of
  its own; the comments are in the submodules, and the review of a file
  under a submodule follows the rules of that submodule, with the paths
  prefixed by its folder (for the core,
  `rtos/.claude/skills/doxygen-review/project.md`, which holds the
  configuration overlay, the checks to run from the folder `rtos/doxygen`,
  and the conventions of the comments).
- Run the automatic checks from the folder of the submodule that holds
  the files, not from the root of the suite; the scripts are those of the
  skill folder of the suite (`.github/skills/doxygen-review/scripts/`),
  or of the submodule, and are given the paths relative to the working
  folder.
- Filter the outputs of the checks to the files named by the user, or
  changed by the current work, and give the counts for those files, not
  for the whole tree (the documentation of the core is far from
  complete; see the rule of the core).
- The ports document the contract that the core declares: the comment of
  a port function must agree with the comment of the declaration in
  `rtos/include/cmsis-plus/rtos/os-decls.h` (the interrupt notes, the
  `@retval` lists, and the preconditions), and must not repeat it
  differently; when the two disagree, decide which one matches the code,
  and report the other.
- The placeholder ports have no sources; there is nothing to review.
