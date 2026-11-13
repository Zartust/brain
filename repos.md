# The repos

Each repo's own `CLAUDE.md` is its master; this map says what each is for and where to look. In the Obsidian vault the
clones sit in `repos/<name>/` (git-ignored), so the paths below open there.

| Repo | What it is | Default branch, and how changes land | Rules and memory |
|---|---|---|---|
| [steelcityrpg_website](https://github.com/Zartust/steelcityrpg_website) | steelcityrpg.com, the public site (Astro, data from Notion and Meetup) | `main`. Feature branch from `preview`, merged into `preview`; `preview` reaches `main` only when Zack says so | `CLAUDE.md`, `docs/README.md` (index), `docs/project/decisions.md`, `docs/handoffs/` |
| [common-table-platform](https://github.com/Zartust/common-table-platform) | Planning home for the Common Table Platform; no application code | `main`, through pull requests | `CLAUDE.md`, `docs/plan.md`, `CONTEXT.md`, `docs/adr/`, `docs/issue-rules.md`, `docs/ci-rules.md`, `docs/agent-runner.md`, `docs/state-map.md` |
| [SCRPG_scribe_bot](https://github.com/Zartust/SCRPG_scribe_bot) | Steel City RPG's Discord bot (GM Roster sync, Player Interest Board, wiki and Grimoire sync) | `master`. Zack merges every code and workflow change: a merge runs against live Discord and Notion | `CLAUDE.md`, `README.md`, `docs/` runbooks |
| [dndpartyapp](https://github.com/Zartust/dndpartyapp) | Local app for Zack's classroom D&D party; runs only on his machine. Will move into `gm-tools` | `main`, through pull requests | `CLAUDE.md` (the student-data rule comes first) |
| [gm-tools](https://github.com/Zartust/gm-tools) | The Greyhawk field guide (shhguide.steelcityrpg.com) and the private GM curriculum | `main`, fast-forwarded from a feature branch once CI is green | `CLAUDE.md`, `README.md`, `sources/` (accuracy only), `curriculum/` (private). Its `obsidian-vault/` is campaign content and stays separate from this vault |
| [homelab](https://github.com/Zartust/homelab) | Self-hosted stacks as code (n8n, Grimoire) | `main`. Branch `<type>/<issue#>-<slug>`, pull request with `Closes #<issue>` | `AGENTS.md`, `README.md` |
| brain (this repo) | Shared rules, skills and the plugin list for every agent | `main` | `CLAUDE.md`, `plugins.md`, this file |

**Tasks** for every repo are GitHub issues; the index across repos is Zartust/common-table-platform#38.
