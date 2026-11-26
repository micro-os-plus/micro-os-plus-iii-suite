# Commit — µOS++ IIIe Suite Rules

The rules of the `github-commit` skill that are specific to this
project. They extend `SKILL.md`, and take precedence where they differ.

## Which Repository

- A commit of the suite contains only the files of the suite (the
  Markdown files, `package.json`, `.gitmodules`, `.claude/`, `.github/`,
  and `maintenance/`), or only the updated submodule pointers; never
  both in the same commit.
- A change to a file inside a submodule is committed in that submodule
  (`git -C <path> ...`), with the conventions of that repository
  (for the core, `rtos/.claude/skills/github-commit/project.md`, which
  also lists the `CHANGELOG.md` rule and the tests to run), and only
  with the approval of the user. Check `git -C <path> status` and the
  current branch first.
- The commit of the pointers is made last, after the commits of the
  submodules, with a subject such as `update submodules`, and a body
  that lists, for each submodule, the old and the new short hash and
  the subject of each commit that moved in. It records a set of commits
  that were tested together (see "Tests" in
  `.github/skills/project.md`); say in the body which tests were run.
- A change to the port contract is committed in the core and in each
  implemented port (each with its own message, which says which commits
  of the other repositories it needs), and the pointer commit follows.

## Issues

- A commit of the suite that fixes an issue of the suite refers to it,
  and the closing comment is added to that issue, as in `SKILL.md`.
- The issues of the components are closed by the commits of the
  components, in their repositories; the pointer commit of the suite
  does not close them (do not use `Fixes #N` there, since the number
  would refer to the suite issues).

## Files Not to Commit

See "Files Not to Commit" in `.github/skills/project.md`; in particular,
a submodule that shows as modified is staged only when the user says so.
