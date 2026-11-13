# Plugins and skill suites

What every session gets, why, and what it costs. Decided 2026-11-13 (Zartust/common-table-platform#60): keep ECC and
Ponytail, add other suites as needed, and keep large suites off until a task needs them.

| Plugin | What it is for | Pinned | Contents | Always-on context | Default |
|---|---|---|---|---|---|
| `mattpocock-skills@claude-plugins-official` | Engineering method skills (code review, TDD, diagnosing bugs, writing for agents) | marketplace default | 27 skills | ~1,650 tokens | on |
| `ponytail@ponytail` | "Simplest thing that works" | `v5.3.3` | 6 skills, 3 hooks | ~640 tokens | on |
| `ecc@ecc` | Large general suite: 387 skills, 68 agents | `v2.2.3` | 387 skills, 68 agents, 7 hooks, 1 MCP server | ~44,800 tokens | **off, on demand** |

Costs are from `claude plugin details <plugin>` (2026-11-13), the context a plugin adds to every session while it is on.

## Where they are installed
- **Cloud sessions:** the cloud environment's setup script (claude.ai › Code › environment `ZClaudeCloude`). A repo's
  `.claude/settings.json` plugin lines do **not** install in cloud sessions
  ([docs](https://code.claude.com/docs/en/cloud-environments.md)). The script's plugin lines:
  ```
  claude plugin marketplace add anthropics/claude-plugins-official || true
  claude plugin install mattpocock-skills@claude-plugins-official || true
  claude plugin marketplace add https://github.com/DietrichGebert/ponytail.git#v5.3.3 || true
  claude plugin install ponytail@ponytail || true
  claude plugin marketplace add https://github.com/affaan-m/ECC.git#v2.2.3 || true
  claude plugin install ecc@ecc || true
  claude plugin disable ecc@ecc || true
  ```
  When this table changes, the setup script changes in the same step, and the other way round.
- **Zack's PC:** the same `claude plugin` commands, once.

## Turning ECC on for a task
Not yet tested in a cloud session (Zartust/common-table-platform#60). The candidate is `claude plugin enable ecc@ecc`;
what is unknown is whether the session must restart before ECC's skills appear. Turn it off again afterwards.

## Adding a plugin
1. Measure it: `claude plugin details <plugin>`, and note the always-on tokens.
2. Add a row here: what it is for, the pinned tag, the cost, on or on demand. A suite over about 5,000 always-on tokens
   is on demand.
3. Its hooks start disabled until Zack turns them on.
4. Add its lines to the setup script.
