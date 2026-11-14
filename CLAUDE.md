# brain: rules for every agent, in every repo

This file loads in every session that has `brain` attached, whichever repo the work is in. It is the master for rules
that are the same in every repo. A rule specific to one repo stays in that repo's `CLAUDE.md`, and that repo's file
wins where the two differ.

Brain loaded: starter version, 2026-11-13. (The loading test on Zartust/common-table-platform#60 checks that this line
is in context.)

## Where things are
- **What each repo is, and where its rules and memory live:** `repos.md`. Read the section for the repo you are about
  to change, then that repo's own `CLAUDE.md`.
- **Plugins and skill suites:** `plugins.md`. ECC is installed but off; turn it on only for a task that needs it.
- **Outside skill repos we've reviewed**, what we took and what we turned down: `skill-sources.md`. Check it before
  adding a suite or copying a skill.
- **Tasks:** GitHub issues. The index of priorities across all repos is Zartust/common-table-platform#38.

## Working on this repo
- Shared rules move in here one repo at a time, each in its own pull request that also removes them from that repo's
  `CLAUDE.md` and `AGENTS.md`. Until a rule has moved, the repo's own copy is the one that counts.
- A skill comes in here when it would otherwise have more than one home (two repos, or a repo and the claude.ai
  account). Its other copies are deleted in the same change, or listed for Zack to remove when only he can (the
  claude.ai account).
- Git author is Zack (`git config user.name "Zack Carns"`, `user.email zcarns03@gmail.com`); credit the agent in a
  `Co-Authored-By` trailer. Never rewrite history.
- Changes to this file, `AGENTS.md` and `.claude/` are Zack's to merge, as in every repo.
