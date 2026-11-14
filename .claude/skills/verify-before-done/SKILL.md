---
name: verify-before-done
description: Use right before saying work is done, fixed or passing — before a commit message, a merge, a pull request, an issue comment or a closing reply, and after any subagent reports success. Proves each claim with fresh command output instead of memory or another agent's word.
---

<!-- Ideas from obra/superpowers (verification-before-completion, MIT) and open-gsd/gsd-core
     (failing-direction.md, gsd-executor self-check), rewritten for these repos. See skill-sources.md. -->

# Verify before done

A claim about the state of the work needs evidence from **this** session, after the last edit. "It passed earlier",
"it should pass" and "the subagent said so" are not evidence.

## The gate
For each thing you're about to claim:
1. **Name the command that proves it**, and what failure looks like (non-zero exit, `0 passed`, `0 collected`, an
   empty grep). A check that can't fail proves nothing: a test run that collected no tests exits 0.
2. **Run it now**, in full. Not a subset, not a related check (a linter is not a type check; a type check is not a build).
3. **Read the result**: exit code, pass and fail counts, the last lines.
4. **Say the claim with the evidence** (`npm test: 412 passed`), or say what's actually true.

## Claims and what proves them
| Claim | Proof | Not proof |
|---|---|---|
| Tests pass | the repo's test command, this session, with counts | an earlier run; one file |
| Build passes | the build command, exit 0 | the type check or linter |
| Bug fixed | the original symptom, re-run, gone | "the code changed" |
| New test guards the fix | it fails with the fix reverted, passes with it | it passes once |
| Files or commits exist | `test -f <path>`; `git merge-base --is-ancestor <sha> HEAD` | your own summary |
| A subagent finished | `git diff` / `git status` show its change | its report |
| Issue done | each "Done when" line ticked against evidence | tests green |

Each repo's own checks are listed in `common-table-platform/docs/agent-runner.md` §5.

## When you can't verify
Say so plainly and say why ("no `NOTION_TOKEN`, so I ran `build:sample`, not `build`"). An unverified step is reported
as unverified, never rounded up to done.

## Gotchas
- Words like "should", "probably", "looks fixed" in your draft reply mean the gate hasn't run.
- A check skipped, quarantined or weakened to get green is not a pass (each repo's CI rules forbid it).
- Three failed fix attempts on one problem: stop and report what you tried, instead of a fourth guess.
