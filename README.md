# conductor

A Claude Code skill for doing any task, from a one-line fix to a multi-hour build, with one smart manager and many cheap workers. The main session (Opus at low effort) agrees a checkable goal with you, splits the work, briefs Haiku agents, verifies the results and keeps state in small files so long work survives a fresh context.

It combines two earlier projects: [crew](https://github.com/7mz410/crew) (how a manager briefs and reviews Haiku workers, lazy code, terse replies, UI rules) and the harness design lessons from [epoptes](https://github.com/ramigb/epoptes) by Rami GB (checkable done definitions, read-only checkers, capped state files, fresh-context cycles). The coding rules also draw on [ponytail](https://github.com/DietrichGebert/ponytail) 5 by Dietrich Gebert and the [Karpathy guidelines](https://github.com/multica-ai/andrej-karpathy-skills).

## Why it works

- **It sizes the work first.** Small edits are done directly. Medium work goes through a goal and the crew loop. Large work adds file-based state. No ceremony where none is needed.
- **Done is a command, not a feeling.** Before spending tokens, the manager agrees the outcome, done checks a command can verify, non-goals, and what needs your approval. You sign off on one screen.
- **Briefs Haiku can't misread.** Every agent gets a fixed template: goal, the context already found, exact files, scope fences, a "done when" check, and a capped report.
- **A checker that never edits.** A Haiku checker only runs commands and reports facts, with timestamps, so stale results can't pass as fresh.
- **Failure has a ceiling.** At most two fix rounds per task. Then the manager does it, or reverts that task's files, marks it blocked and moves on.
- **Parallel without collisions.** Tasks that run together own separate files, and parallel writers use git worktrees.
- **Memory lives in files.** A goal, backlog, handoff, decisions and lessons, each with a size cap. Any new session continues from them.
- **The time box is a limit, not a target.** It finishes when the checks pass and never invents work to fill time.
- **Laziest code that works.** Before writing, it maps everything the change must reach (callers, tests, config) and what it could break. Reuse, stdlib, native platform and installed dependencies come before new code. Known shortcuts are marked `shortcut: <limit>, <when to upgrade>`.
- **Surgical changes.** Every changed line traces to the request: no drive-by refactors or reformatting, and unrelated dead code is reported, not deleted. A request with two readings gets both shown instead of a silent pick.
- **Bug fixes start with a repro.** A failing test first, then one fix at the root cause in the shared code.
- **Honest endings.** Every reply ends with what was skipped or not checked, and any risk.
- **UI quality and safety built in.** Design tokens, light and dark themes, WCAG contrast, keyboard access, RTL support, careful deletes, no secrets in files, no push or deploy without review.
- **Self-contained.** Every rule is in one file. Optional skills are used when present and never required.

## Install

```sh
git clone https://github.com/7mz410/conductor.git
cp -r conductor/conductor ~/.claude/skills/
```

Start a session with `/conductor`. For the intended setup, run the main session on Opus with low effort; the skill sends agents to Haiku.

## Controls

- `stop voice`, `stop build`, `stop design`, `stop crew`: turn off one part.
- `normal mode`: turn everything off.
- `/conductor voice lite|full|ultra`, `/conductor build lite|full|ultra`: change intensity. For build, lite names the smaller option and builds what was asked, full is the default, ultra also pushes back on parts of the request the need doesn't justify.

Your project's own `CLAUDE.md`, `PROJECT.md` or `README.md` wins where it conflicts with the skill.

## Credits

- Harness design ideas from [epoptes](https://github.com/ramigb/epoptes) by Rami GB (MIT).
- Coding rules adapted from [ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert (MIT) and [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) by multica-ai (the skill file is marked MIT).

## License

MIT
