# xcdl — µOS++ IIIe Suite Rules

Not applicable to the suite: it has no `xcdl-package.jsonc` and no
`xcdl-test.jsonc`, and generates no `CMakeLists.txt` or `meson.build`.
The `xcdl-export` action of `package.json` and
`maintenance/scripts/xcdl-export.mjs` come from the common template and
have nothing to export.

The submodules decide for themselves:

- the core (`rtos/`) has no `xcdl-package.jsonc` either; a new, renamed,
  or removed source file is changed by hand in `target_sources()` of its
  top-level `CMakeLists.txt` (see `rtos/.claude/skills/xcdl/project.md`);
- before changing the build files of a port, read its `CLAUDE.md`, and
  check whether it has an `xcdl-package.jsonc` (the `xcdl-export` action
  of its `package.json`); when it has, follow the `xcdl` skill of that
  port, and do not edit the generated files.
