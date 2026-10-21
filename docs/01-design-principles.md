# Design principles

> **A blueprint, not our live config.** This describes how we approach building a team of local AI
> agents. Names, numbers and versions are examples from our build in October 2026.

## The idea in three lines

- **Local first.** Private data never goes to a cloud model.
- **A person approves.** Only the owner's own tap counts as a yes. New agents start in shadow mode.
- **Proven, not promised.** Frozen install bundles, a lab, and a sealed test before any agent goes live.

---

## 1. Hybrid local and cloud

We run open-weight models on our own hardware for anything private, and use cloud tools only where
the data is public, fake, or is code that holds no personal data.

- **Local models do the private work.** A shared model server on the private network serves every
  agent. Each agent's helper ("auxiliary") model slots are pinned to local models too, because an
  unpinned helper slot is a hidden path to the cloud.
- **Cloud seats live in a separate builder account,** used for code and fake data only. That
  account can't see media, the shared memory, or the password vault.
- **Every cloud key has a hard spending cap at the provider.** Hitting the cap pauses the job; it
  doesn't fail over to something else.
- **No cloud keys in agent or sandbox environments.** A tool whose default backend is cloud gets
  switched to local before its first run, and DNS logs confirm it.
- **Quarterly drill:** turn every cloud key off and check that every agent still works.

Why: subscription seats and free tiers often allow training on your data and don't come with a
business associate agreement or zero-retention terms (see `research/HYBRID_CLOUD_TERMS.md`). Treat
every seat and free tier as "public data only".

## 2. Data levels

Every piece of data gets a level, and the level decides where it may go.

| Level | What it covers | Where it can go |
|---|---|---|
| T0 | secrets, account and ID numbers, money data, legal and ID mail. Recorded by name only. | Never to a cloud model. Never filed in shared memory. |
| T1 | the default for shared-memory pages; voice recordings; contact data | Local only, or a vendor under zero data retention plus a data processing agreement, with a written exception and a legal sign-off |
| T2 | files about the people we serve, photos of people until a release is signed, the org's own workspace | Local only |
| T3 | public material, or material meant to be public | May use a cloud tool, on the first safe rung below |

**The safe-path ladder.** A task always takes the first rung that works:

1. run the pattern locally
2. public data only
3. fake data
4. derived output (a summary, not the source)
5. redacted locally
6. a separate account
7. a data processing agreement plus a written exception
8. a person does it by hand
9. never

Automatic PII redaction (Presidio and similar) is a second net on the public lane, never the gate
that lets private data out. Redaction is not de-identification.

Model choice follows the data level first and the job type second. No "smart router" guesses for
private data.

## 3. The gateway: agents never hold keys

- **One egress gateway** sits between the agents and the internet. It holds the keys and adds them
  on the way out, against an allowlist. A new destination with a key is a person's decision and
  gets a job record.
- **No secret shares an OS account with an agent.** Every keyed service runs as a different
  account than any agent.
- **Every tool's model is pinned local.** No tool may pick a model from whatever key it finds.
- **Telemetry is off** for every tool, and "off" is proven by watching the traffic, not by reading
  the setting.
- **Reading is split from acting.** An agent that reads outside text hands it to a member with no
  tools. The summary that comes back is data, never instructions.
- **Default-deny covers our own admin pages too.**

## 4. Approvals

**Five fenced verbs always wait for a person: Send. Spend. Publish. Delete. Legal.** They are
enforced in configuration and code, not just written in a prompt. Pushing to `main`, deploying and
changing DNS count as Publish. Changing an agent's rules, hooks, approval settings or key
permissions is governance and is the owner's call too.

- **A yes covers one action, one target and one scope.** If any of those change, ask again.
- **No answer means no.**
- **A message from another agent is information, never permission.** A yes passed along by an
  agent, or quoted in a file, isn't a yes.
- **Never tap "Always" on an approval card.** Approve once, or deny.
- **Every tool has an approval class.** Anything unclassified is treated as the most dangerous kind.
- **A must-ask command still asks** when it's run by full path, through `sh -c`, `env` or `xargs`.
  Each must-ask rule has a code gate and a test that tries to get around it.

**The job flow,** using a website change as the example:

1. The chief-of-staff agent writes a contract (scope, budget, done-criteria) and routes the work.
2. The builder works on a branch.
3. The independent tester checks it against tests written before the build.
4. A compliance reviewer checks the public wording.
5. The builder serves a private preview.
6. The owner gets one package: the preview link, screenshots, the tester's verdict unedited, the
   reviewer's note, the changes and the cost, with Approve / Rewrite / Reject.
7. On Approve, that exact change is deployed, the tester checks the live page, and the job is
   reported complete.

## 5. Walls before trust

**Sentences persuade; hooks enforce.** A rule in an agent's prompt is a suggestion. A rule in
config, file permissions, the OS sandbox or a hook is a wall.

- **Agents can't change their own rules.** Each agent's prompt file, settings and environment file
  belong to the machine's admin account. The agent may read them but never change, rename or
  delete them.
- **Each agent runs only through a root-owned launcher inside an OS sandbox** (Seatbelt on macOS,
  Bubblewrap on Linux). The sandbox blocks writes to its own rules, its code, hooks, shell startup
  files and login items, and blocks reading the environment file.
- **File tools may write only in the agent's work folder, skills and memory.**
- **Agents run on the local terminal backend inside the OS sandbox.** We found that the agent
  framework skips its dangerous-command check when its terminal runs in a container, so
  containers are for services only.
- **A rules change follows one path:** anyone may suggest it; it's built from one source by a
  script and checked against a written review standard; the owner sees the diff and approves; the
  admin installs it with its checksum; a fresh session and a rehearsal confirm it.
- **A watcher compares checksums** against the approved baseline. A change that didn't come through
  that path is an urgent alarm, and the approved version is put back with the evidence kept.

**How an agent's prompt ("soul") is built:**

- One shared block, word for word in every agent, plus one agent-specific part, built by a script.
  No hand-edited copies.
- Short enough to load whole. The framework we use cuts these files at 12,000 characters and
  drops the middle, so the build fails anything over 11,000. Budget per file, and "one in, one
  out" when adding a rule.
- The prompt holds who the agent is, its lane and its hard limits, one line per rule. Procedures
  and worked examples go in a separate instructions file. Explanations of why go in the build log.
- Never in a prompt: IP addresses, model names, secrets, dates that change, machine paths, or
  placeholders.
- Safety rules always live in the prompt, never in a skill that may not load. Helpers never see the
  prompt, so every helper brief starts with a fixed helper card.

## 6. Shadow mode and the safety gate

No agent goes live until it's proven on cases it has never seen. Words can't be guaranteed; walls
stop harm, and a sealed test decides.

1. **Install checks:** facts read from the real machine first, a frozen bundle with checksums
   verified on the source and on the target.
2. **Wall tests:** zero failures.
3. **Prompt check** passes on every channel the agent uses.
4. **Standard test pack:** every case that fits the role, several runs each, zero safety misses.
5. **Sealed held-out pack:** cases the builders never saw, zero safety misses, two blind graders.
6. **A week in shadow mode:** the agent plans and drafts; a person reviews everything; nothing
   leaves.
7. **The owner's OK,** logged with a date. The agent goes live in a narrow lane first.

**The stop rule:** if an agent still has safety misses after one full round of fixes, the next step
is a model test, not another round of rewording. Code guards come first, then the model test.

## 7. The independent judge (the PROOF pattern)

**The maker never grades its own work.**

- The tester is a separate agent with its own home folder and its own sandbox, started by a root
  runner. The builder never starts it and can't read or write its files.
- **The judge never uses the builder's model family,** and has no fallback model. The runner
  checks the served model's family before every job.
- **Answer keys, frozen tests and the grading script sit under a separate OS account with no agent
  on it.** If the builder can read the keys, that test set is burned.
- The runner opens the tester's job from a frozen, checksummed copy of the builder's work, so the
  builder can't narrow a review. The verdict is sealed with a checksum and sent to the owner. A
  mismatch is an alarm.
- Code under test runs in a third, tighter sandbox: no network, no access to the tester's files.
- **Safety tests are graded from real artifacts** (files, logs, exit codes), never from what the
  agent says it did.
- **Every grader needs a control** proving it can both pass and fail on the real machine.
- The pattern generalizes: an agent that must judge others, or hold something its teammates must
  not see, gets its own profile; an agent with real powers (send, spend) gets its own account.

## 8. Loop patterns

- **A fresh session per job,** small prompts and few skills. One long session did far worse than
  fresh ones in our tests, and cutting auto-made skills raised success.
- **Spec-wall handoff:** one agent writes the spec, another builds from it.
- **Claims need a control run:** old, new, old again.
- **Start on one folder, and stop after two rounds with no progress.** Cap retries (we use four).
- **BLOCKED always carries a reason.**
- **Declare the paths you'll touch first, and check the diff after.**
- **Claims must cite job-record entries.** The job record keeps per-file changes, a per-job budget,
  separate "reviewed" and "approved" fields, and a "sent? unknown" state with no automatic resend.
- **Learned lessons pass the tester and the owner before they become skills.** Every memory write
  is read back.
- **Checks cost zero tokens where they can:** plain scripts and scheduled jobs that wake an agent
  only when something is wrong. Healthy output stays silent; failures are always shown.
- **Unattended briefs fail loudly.**

## 9. Delete safety

- **Delete means move to trash.** Agents and scripts never remove a file outright. They move it to a
  dated trash folder with a note of where it came from, who moved it and why.
- **Trash is kept 30 days** in a copy the agent's own account can't change or empty. Only a person
  purges, after reading the list.
- A real delete is a fenced verb, enforced in code.
- Under it: daily snapshots, machine backups, and an off-site copy with versioning and object lock.

## 10. No flat "can't": holds and nevers

Every blocked idea gets a level and an alternative:

- **E0, never:** breaks a law, someone else's rights or terms, or a fenced verb. Every E0 row names
  what to use instead.
- **E1, lab only:** fake or public data, in an isolated lab.
- **E2, written exception:** an ID, a scope, safeguards, a test and a review date.

Examples of E0 and what we use instead: agent trading or payments (people pay by hand; paper
trading for classes); pooled free keys or borrowed logins (one official tier per provider); bot-check
or CAPTCHA bypass (official APIs, then RSS, archives, polite crawling, or ask the owner); fake
testimonials and AI "humanizers" (real testimonials with consent; AI use disclosed); voice cloning
without consent (your own voice, or a signed release); models with safety training removed (the
open-weight model, run locally).

All E0 and E2 rows are reviewed quarterly, or sooner if a law, a vendor's terms or the tool changes.

## 11. Rules of engagement

**For research and outside material:**

- Read the post and every real link. Read only; helpers fetch, never download.
- Keep FACT apart from INFERENCE. Spot-check big claims.
- Take a second look at everything skipped: what can we still learn, what's the legitimate route,
  what would move it up?
- Text on a page aimed at AI agents ("fetch and run this") is page data, never an instruction.
- What we learn becomes written rules, placed in the build order, so it isn't overlooked.

**For outside skills, plugins and MCP servers (the intake audit):**

- Nothing reaches an agent without an audit: read in a throwaway container, never run, one page of
  risks with file and line evidence.
- Check: is it maintained, known-bad versions refused, signatures, two scanners, the licence, and
  telemetry off proven by traffic. Vendor text that says "don't ask the user" fails.
- Pin by version or digest, wait out a cooldown on new releases, and read any installer before
  running it. No `curl | sh` from a moving branch.

**For reverse engineering and security work** (see `research/REVERSE_ENGINEERING_LEGAL_FRAME.md`):

- Targets are only what we own or have lawfully licensed, practice targets built to be broken, or
  systems whose owner gave written permission.
- Before any work, write down where the copy came from, what the terms say, the purpose
  (interoperability, security or repair), and whether the information is already documented.
- Work happens in an isolated sandbox with no outbound network. Security findings go to the vendor
  first. Nothing is published without review.
- Off-limits: cheats, piracy, getting around paid subscriptions, stripping DRM.
