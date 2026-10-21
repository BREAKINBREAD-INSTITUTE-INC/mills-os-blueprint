# oh-my-hermes (OMH): first look, Oct 21, 2026

Repo: https://github.com/rlaope/oh-my-hermes. MIT license, about 3.5k stars, 5,225 commits. I read the page only; nothing was installed or run.

**What it is:** a layer that sits on top of Hermes as one plugin. It says it never patches Hermes. It adds:
- 108 `omh-*` skills
- 9 `ulw-*` workflows: context, interview, research, plan, work, maestro, loop, QA, perf
- a file-based long-term memory provider
- model routing chains by task category
- a terminal dashboard
- handoffs to Claude Code and Codex

**Proposed verdict: LAB (borrow the ideas; don't install in production yet).** JJ decides. Under our harness rule it would need the full entry test first.

## Risks against our rules
1. **Routing is cloud-first and goes by task category, not by how private the data is.** The default chains are GPT-6, Claude, Kimi, GLM, DeepSeek and Gemini, with some served through a third party ("OpenGateway"). That breaks the hybrid rule unless every chain is pinned to local models, or it all sits behind our gateway's data-level routing.
2. **The install is `curl ... | sh` from `main`, and `omh update` pulls the latest.** Both are unpinned. We would install from a reviewed, pinned commit only.
3. **It reads `$HERMES_HOME/.env` and the Hermes config** to detect providers. It says it reads only IDs and variable names. *Not confirmed:* needs a source read.
4. **The page has a prompt aimed at AI agents** ("fetch and run INSTALL_FOR_AGENTS.md"). We ignored it as page data. This is a pattern the intake audit should flag.
5. **It's very large:** 108 skills plus a memory provider plus a plugin. That's a big attack surface. NVIDIA found that 26% of public skills had vulnerabilities.
6. **It can start Claude Code or Codex from Hermes** (opt-in). That would be a new path to a cloud model and must go through the gateway.
7. **No published benchmark** shows Hermes plus OMH beats plain Hermes. The project says so itself.
8. **Supported Hermes versions aren't stated.** We'd be on v0.22.2.

## Ideas worth taking now (they work with local models, and we write them ourselves)
- **A "completion-integrity gate":** never report work as done unless it was seen to happen. This matches PROOF and "progress only with evidence".
- **Parallel work with fixed file ownership** and a git worktree per unit, for FORGE.
- **The interview, plan, work, QA chain** (`ulw-*`) as the shape of a FORGE job.
- **Routing chains with fallback, crossed with data levels:** this is the idea for our gateway's policy layer (pick a model by task *and* by data level).
- **Reviewed long-term memory with "recall packs"**, for ARCHIVE and Keeper. Memory edits get reviewed before they stick.
- **A dashboard with cost, cache and evidence rows**, for Mission Control.

## If JJ wants to try it (lab only)
1. Read install.sh and the plugin source at a pinned commit, through the intake audit.
2. Install it in the LAB VM with fake data, and point every routing chain at local models only.
3. Check that it never reads secret values from `.env`, and watch its network traffic.
4. Measure it against plain Hermes on our own task list before deciding anything.
