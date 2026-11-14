# brain

The shared layer for every agent working in Zack's repos, and the Obsidian vault Zack edits it in.
Decided 2026-11-13 (Zartust/common-table-platform#60).

- **One master for what agents share:** rules that apply in every repo (`CLAUDE.md`), skills used in more than one
  repo (`.claude/skills/`), the plugin list (`plugins.md`) and the map of the repos (`repos.md`).
- **The repos stay the memory for their own work.** Each repo's code, `CLAUDE.md`, `docs/` and decision records live
  in that repo. This repo links to them; it never copies them.

## How it loads

| Where | How |
|---|---|
| Cloud sessions | Select `brain` beside the other repos when starting a session. If it isn't selected, each repo's `CLAUDE.md` tells the session to attach it first (`add_repo`, clone, `register_repo_root`; Zack, 2026-11-14), and it loads mid-session. Every attached repo's `CLAUDE.md` and `.claude/skills/` load, so nothing is copied or linked. |
| Nightly runner | Adds `brain` with the other repos (`common-table-platform/docs/agent-runner.md` §1). |
| Zack's PC | `~/.claude/CLAUDE.md` links to `brain/CLAUDE.md`, and each `~/.claude/skills/<name>` to `brain/.claude/skills/<name>`. |
| Plugins | Installed by the cloud environment's setup script, not from here. `plugins.md` says which, why and at what cost. |

## Open it in Obsidian (PC)

1. Clone this repo and open the folder as a vault.
2. Clone the other repos into `repos/` inside it (`repos/` is git-ignored), so links from `repos.md` open their files.

## Status

Starter version. Still to do, in order (checklist on Zartust/common-table-platform#60): the loading test, how to turn
an on-demand plugin on, an ADR, then moving shared skills and rules in here and deleting the copies.
