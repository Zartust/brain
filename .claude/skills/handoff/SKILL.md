---
name: handoff
description: Close out a branch so another session can pick the work up cold — the repo's checks, a handoff written for someone who never saw this conversation, one "Resume:" issue with the open items as a checklist, and landing the branch the repo's way if the checks pass. Use when a session is ending with unmerged work on its branch, when a 2-hour idle check-in fires, when Zack says "wrap up", "hand off" or "close the branch", or when picking up a "Resume:" or "Idle branch:" issue. Works in every repo (replaces the website's scrpg-handoff).
---

# Handoff

Nothing lives only on a branch or only in a chat. When a session stops with unfinished work, the next session, or the
nightly runner, must be able to start from GitHub alone.

## Per repo
The repo's own `CLAUDE.md` wins where it differs. Its checks are in `common-table-platform/docs/agent-runner.md` §5.

| Repo | Base branch | Where the handoff goes | If the checks pass |
|---|---|---|---|
| `steelcityrpg_website` | `preview` | a file: `docs/handoffs/TEMPLATE.md` → `docs/handoffs/<branch, / → ->.md`, committed on the branch (see "Website extras") | merge into `preview` (merge commit) |
| `gm-tools` | `main` | the issue body | fast-forward `main` once CI is green, as its `CLAUDE.md` says |
| `common-table-platform` | `main` | the issue body | push, pull request (self-merge only when its `CLAUDE.md` allows) |
| `SCRPG_scribe_bot`, `dndpartyapp`, `brain` | default branch | the issue body | push, pull request for Zack |
| `homelab` | `main` | the issue body | push, pull request with `Closes #<issue>` |

## When you stop with work on a branch
Push it. In the website (its rule), also schedule the idle check: `send_later` with `delay_minutes: 120`, "Idle check
for <repo>/<branch>: if nothing happened since <time>, close it out with the handoff skill." When it fires, do nothing
if the branch's last commit is under 2 hours old. In the other repos, close out before the session ends.

## Closing out
1. **Bring the base in:** `git fetch origin <base> && git merge origin/<base>`. Never rebase a pushed branch.
2. **Run the repo's checks** and keep the result lines (`verify-before-done`). A check you couldn't run is written
   down as not run, and why.
3. **Write the handoff** for a session that has never seen this conversation. Website: fill its `TEMPLATE.md`.
   Elsewhere, put this in the issue body:
   ```
   Branch: <repo>/<branch> · base <base>@<sha> · closed out <YYYY-MM-DD> (session ended / idle 2 h)
   Status: landed / not landed (why)
   ## Goal: what was asked, and the "Done when"
   ## Done: what works now, and where (files, commits)
   ## Left: the checklist (- [ ] …, waits on Zack: #n)
   ## How to resume: exact commands (fetch, checkout, merge the base, install, checks), then the first file to open
   ## Decisions: made (and where recorded) / waiting on Zack (#n)
   ## Verification: what ran and its result; what didn't, and why
   ## Gotchas: what the next session would otherwise rediscover the hard way
   ```
   Never put secrets, `.env` values or student data in it.
4. **One issue** (search first, and reuse an existing one):
   - "Resume: <what the branch is for>", labelled per the repo's rules (`agent`/`zack`, one `priority:`, one `type:`,
     a parent epic). If an "Idle branch: <name>" issue already exists for the branch, use it and retitle nothing.
   - **The open items are a checklist on that issue**, each small enough for one session; not sub-issues (issue
     rules: at most three new issues a session). Open a sub-issue only for an item with a different owner, or one that
     waits on something the others don't. An agent item that waits on Zack names the `zack` issue, and Zack's actions
     go on `zack` issues.
5. **Commit and push** (author Zack Carns, the agent in `Co-Authored-By`).
6. **Land it** as the table says if the checks passed. Otherwise leave it, and say why in the handoff and the issue.
7. **Tell Zack** in one short message: what landed, what's open (issue links), what he must do.

## Website extras (`steelcityrpg_website`)
- **Gate:** `npm test`, `npm run check`, `npm run build:sample`, `npm run smoke`. In a cloud session, use
  `CHROMIUM_PATH=/opt/pw-browsers/chromium` if the pinned Playwright can't find its browser. Resolve `decisions.md`
  conflicts with entries in date order, newest first.
- **After merging into `preview`:** CI doesn't run there on its own (round 129). If the branch changed a page that
  depends on Notion, run Actions › Real-data check by hand on `preview` (round 133).
- **Claims on #79** (round 135): when merged, edit your claim to start with `RELEASED` and name the merge commit. When
  not merged, write `HANDED OFF` and link the issue.
- **"Idle branch" issues** come from the sweep (`scripts/stale-branches.sh`). Never close one as *completed* while its
  branch has unmerged work, or the sweep opens it again. Close it as **not planned** to have the sweep leave the
  branch alone.
- Process and the branch model: `docs/handoffs/README.md`.

## Picking up
Open the "Resume:" or "Idle branch:" issue (and, in the website, the handoff file; claim its areas on #79). Check out
the branch, merge the base in, work the checklist, run the checks, land it. Close the issue, and in the website delete
the handoff file, in the same change.

## Never
- Delete a branch with unmerged work, or force-push one.
- Land anywhere the table doesn't name. No `main` in the website without Zack's go.
- Leave an open item only in a handoff: each one is a checkbox on the issue.

## Gotchas
- A shallow clone can refuse the merge in step 1 ("refusing to merge unrelated histories"):
  `git fetch --depth=200 origin <base>` first.
- This skill lives in the brain. A session without `brain` attached doesn't have it, which is why each repo's
  `CLAUDE.md` says to attach the brain first.
