# Skill sources

Outside skill repos and ideas we've looked at, what we took from each, and where it lives now. This is the place to
come back to before adding a suite, or to re-review one when it changes. Plugins we actually install are in
`plugins.md`.

**Bar for taking something:** a distinct idea we don't already have, that fits our rules (student data stays in one
local database; GitHub issues are the only task list; Impeccable leads UI). Every suite costs context in every
session and adds competing triggers, so most answers are "no" or "take one idea".

Reviewed 2026-11-14 from shallow clones (Zartust/common-table-platform#60). Re-review a row when you're about to
adopt it, or when its "Recheck" date passes.

| Source | What it is | Verdict | What we took, and where | Recheck |
|---|---|---|---|---|
| [obra/superpowers](https://github.com/obra/superpowers) (MIT) | 14 process skills: brainstorm, plan, execute, TDD, debugging, review, worktrees | Take one idea | "Evidence before claims" → `.claude/skills/verify-before-done`. The rest duplicates mattpocock-skills (`tdd`, `grilling`, `diagnosing-bugs`, `code-review`) | — |
| [chiefgyk3d/department-of-internal-arguments](https://github.com/chiefgyk3d/department-of-internal-arguments) (Apache-2.0) | Reusable agent roles plus domain "packs" (TikTok video, 2026-11-14) | Take one role | Adversarial reviewer → `.claude/agents/adversarial-reviewer.md`. Skipped: local-LLM routing (no GPU in cloud sessions) | 2027-02 (its other roles: reviewer, researcher) |
| [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core) | A full plan → execute → verify framework, `.planning/` state files, ~40 subagents | Take rules, not the framework | "Each check says how it fails", self-check of claimed files and commits, stop after 3 attempts, no unasked installs → `verify-before-done` and `common-table-platform/docs/agent-runner.md` §4–5. Its state files would be a second task list. Unused idea: the assumptions analyzer (each assumption with "if wrong" and a confidence, for Zack to confirm), a lighter cousin of `grilling` | — |
| [shareAI-lab/learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) | 17-step tutorial building an agent harness | Reading only | Lessons: a separate judge decides "done" (s17); compaction keeps a path back to everything removed (s08) | — |
| [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) | Tips gathered from Anthropic staff posts | Take two ideas | Not yet applied: path-scoped `.claude/rules/*.md` (`paths:` frontmatter), and a Gotchas section in every skill (both on #60). Don't copy its `settings.json` (`Bash(*)`) | — |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | Awesome-list, ~260 entries | Maybe later | Candidates, unvetted: [agnix](https://github.com/agent-sh/agnix) or [Ctxlint](https://github.com/ctxlint/Ctxlint) (stale paths in `CLAUDE.md`/`AGENTS.md`), [Safety Net](https://github.com/kenryu42/claude-code-safety-net) (blocks destructive git and shell commands), [ccusage](https://github.com/ccusage/ccusage) (token cost). Being listed isn't vetting: read a hook's source before installing | 2027-02 |
| [HKUDS/LightRAG](https://github.com/HKUDS/LightRAG) | Graph-plus-vector RAG library and server | No | Not a skill; heavy for what grep and `docs/` do. One idea from its agent files: `CLAUDE.md` is just `@AGENTS.md` (as `homelab` does; Zack to decide, #60) | — |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | Hooks record every tool call into SQLite and vectors, summarised by a second model | **No: privacy** | Nothing installed. It would copy any student data read in `dndpartyapp` into a second database and send it to an observer model. Our memory is git, which is reviewable | — |
| [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | CSV catalogue of styles, palettes and fonts, searched by a script | No | Its trigger claims all UI work, the same as Impeccable; its catalogue includes styles our anti-slop rules reject | — |
| [kylezantos/design-motion-principles](https://github.com/kylezantos/design-motion-principles) (MIT) | Motion create/audit skill through three designers' lenses | No skill; one idea | The website already passes its AI-slop checklist. Idea for the site's style guide: the frequency gate (the more often something is triggered, the less it animates; keyboard actions never) (#60) | — |
| "9 Claude Skills that write your prompt for you" (TikTok graphic) | prompt-master, grill-me, how-to, opus/fable optimizer, personal-voice, anti-AI, write-a-skill, handoff | Already have six | `grilling`, `brand-voice-builder`, Impeccable / anti-slop, `writing-for-agents` / `skill-creator`, `scrpg-handoff`. Prompt optimizers skipped: in Claude Code the repo and `CLAUDE.md` are the prompt. Gap: a handoff skill for repos other than the website (#60) | — |
| Matt Pocock, "tactical tornado" (TikTok video) | Agents get today's task working and leave a mess; tell them what to care about | Idea | Each repo's `CLAUDE.md` is rules only. A short "what good looks like here" section per repo would tell agents what to protect | — |

## Licences
Material adapted from these sources keeps a credit comment at the top of the file. Apache-2.0 sources (the
adversarial reviewer) also need that comment to say the file was changed. Don't copy text verbatim from a repo
whose licence you haven't checked.
