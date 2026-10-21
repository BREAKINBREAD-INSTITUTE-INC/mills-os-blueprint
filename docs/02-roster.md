# The roster: leads, members and helpers

> **A blueprint, not our live config.** Machine addresses, account names and build status are
> left out on purpose.

## Four layers

1. **The owner.** Talks mainly to the chief of staff, by text or voice note, and may also talk to
   any lead directly. Approves everything that leaves the building.
2. **The chief of staff (FRIDAY).** Plans the work, sends it to the right lead, tracks it and
   reports back. Does no specialist work itself.
3. **Team leads.** Each lead has its own name and prompt, its own memory, its own agent profile and
   chat line, and a home machine. A lead owns its results and reports to the chief of staff.
4. **Members and helpers.**
   - **Members** are saved role briefs with a short tool list. They have no memory or chat line of
     their own and run only when their lead calls them. A member that needs its own tools, model
     or schedule gets its own small profile.
   - **Helpers** are temporary sub-agents for one task, at most three per lead, plus a fleet-wide
     cap. They run on the local model, can't send or write memory, and are retired when the task is
     done.

**ARCHIVE** is a service, not a chat agent: the shared memory, kept separate for each entity, with
one writer.

## The leads

| Lead | Owns | Members (on-demand roles) |
|---|---|---|
| FRIDAY | Chief of staff: requests, planning, briefings, follow-ups, routing | none |
| SCOUT | Research and fact-checking. The only web-facing lead: no private data, no send rights. | researcher, librarian, LAB (the always-learning member) |
| GRACE | Grant writing and the org's sensitive records. Local models only, no cloud keys. | writer, records, intake (read-only mail) |
| LEX | Legal review, contracts, filings | COMPLY (also checks other leads' public output), contracts, filings, policies |
| FORGE | Code, websites, the dashboard, the installer | architect, coder, tester (PROOF: nothing ships until it passes), security, IT setup |
| HARVEST | Everything made from recordings and teaching: podcast, courses, clips | producer, media tech, editor, designer, writer, curriculum builder |
| HERALD | Email, texts, calls, social posts, follow-ups | drafter, follow-ups, RELAY (the only role anywhere with send rights, and only after the owner approves) |
| SENTINEL | Watches the fleet: health, power, backups, updates, drift, alerts. Watches only: never logs into other machines, never fixes them. | backup keeper, key and date keeper, security watch |
| JARVIS | The owner's personal lead: study, growth, planning. Talks to FRIDAY only for scheduling. | librarian, scholar, coach, planner, Keeper (home network) |

**Content pipeline (HARVEST), as a background queue:** a recording lands in the storage inbox,
HARVEST works on it overnight, COMPLY checks facts and claims, the owner gets a morning approval
list, and RELAY publishes only what the owner approved.

## How leads talk

- Every lead can reach every other lead through the chief of staff, who logs each handoff.
- A short list of approved direct pairs, for example: GRACE and LEX/COMPLY; any lead to ARCHIVE
  (filing); SENTINEL to FRIDAY (status); FORGE and PROOF (through a job board and a root runner);
  JARVIS and FRIDAY (scheduling only).
- Members and helpers never get a chat line.
- Work passes between leads over the private network only.

## Who approves what

- **The leads** do plans, branch code, drafts, tests and previews inside the chief of staff's
  contract. No approval needed.
- **"Ready"** means PROOF or COMPLY passed it and the chief of staff packaged it.
- **The owner only:** Send, Spend, Publish, Delete and Legal; push to `main`, deploys and DNS; and
  any change to an agent's rules.
- The chief of staff can say "go" on work, but never says yes on the owner's behalf.

## Machines, by role

Roles only; the hardware is what we happened to have.

| Role | Hardware class | Notes |
|---|---|---|
| Model server | Linux GPU box (DGX Spark class) | The shared brain for every lead and helper, and ARCHIVE's only writer. Bound to the private network. |
| Chief-of-staff machine | Mac Studio | FRIDAY; later HERALD under its own OS account. No web-reading agents on this box, so send rights stay away from outside material. |
| Builder machine | Mac Studio | FORGE plus PROOF in its own home and sandbox; later HARVEST and media work. |
| Research machine | Mac Studio | SCOUT and LAB. The only web-facing box. |
| Private-records machine | Mac Studio | GRACE and LEX/COMPLY. Local models only, no cloud keys at all. |
| Watch post | Mac mini | SENTINEL and its own small local model. |
| Command laptop | MacBook Pro | Runs every install bundle, holds the admin key, backup command and an offline copy of the chief of staff. Never wiped. |
| Shared storage | NAS | Backup target. Never the only copy of anything. |

Rules that came with this structure:

- Every lead on a shared machine gets its own profile folder, service label, and its own OS
  account where the data level needs it.
- Every agent machine gets the same baseline: disk encryption, device management, the AI network
  segment, the private overlay network with its built-in SSH turned off, and SSH by the command
  laptop's key only.
- Every lead gets: its prompt, an instructions file, a settings file, a command block-lock, a model
  that passed the lab and the sealed test for that role, OS walls, its own secrets account, a
  nightly prompt check, its own chat line, a watchdog, delete safety, and a place on Mission
  Control.
