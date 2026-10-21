# Mills OS blueprint

> **A blueprint, not our live config.**
> This is how we approached building Mills OS, a team of AI agents that runs on our own hardware
> for a small nonprofit. It is not a copy of our setup. Addresses, account names, keys, policies,
> install bundles and build status are left out on purpose, and nothing here will configure a
> machine for you.

## The idea

- **Local first.** Private data never goes to a cloud model. Cloud tools get public data, fake
  data, or code.
- **A person approves.** Send, Spend, Publish, Delete and Legal always wait for a human tap. New
  agents start in shadow mode.
- **Proven, not promised.** Walls in the OS, an independent judge that never shares the builder's
  model family, and a sealed test before any agent goes live.

## What's in here

| Path | What it is |
|---|---|
| [`docs/01-design-principles.md`](docs/01-design-principles.md) | Hybrid local and cloud, data levels, the egress gateway, approvals, walls, shadow mode and the safety gate, the independent judge (PROOF), loop patterns, delete safety, holds and nevers, rules of engagement |
| [`docs/02-roster.md`](docs/02-roster.md) | The leads, their members and helpers, how they talk, who approves what, and machines by role |
| [`docs/03-lessons-learned.md`](docs/03-lessons-learned.md) | What changed how we build, from process to shell scripting to secrets |
| [`research/HYBRID_CLOUD_TERMS.md`](research/HYBRID_CLOUD_TERMS.md) | Cloud AI terms compared for training, retention, agent use and regulated data |
| [`research/REVERSE_ENGINEERING_LEGAL_FRAME.md`](research/REVERSE_ENGINEERING_LEGAL_FRAME.md) | US legal and ethical frame for reverse engineering, and our draft rules of engagement |
| [`research/REVIEW_oh-my-hermes.md`](research/REVIEW_oh-my-hermes.md) | A first-look review of a large plugin layer, against the rules above |
| [`reference/SKILLS_AND_TOOLS_REFERENCE.md`](reference/SKILLS_AND_TOOLS_REFERENCE.md) | About 700 repos, tools, models, sites and channels, each with a verdict, licence and risks |
| [`reference/skills_and_tools.tsv`](reference/skills_and_tools.tsv) | The same, as a spreadsheet |
| [`reference/video_lessons.tsv`](reference/video_lessons.tsv) | About 1,000 videos summarized from their transcripts: main points, tools named, claims to verify |

## How to read it

Start with the principles, then the roster. The lessons file is the fastest way to avoid the
mistakes we made. The reference files are a reading list, not endorsements: every verdict is a
proposal for our setup, and prices, versions and licences drift. Check before you rely on them.

None of this is legal advice. The research notes cite their sources; read the originals.

## Related repos

- `mills-tv`: the TV launcher we use as a glance screen, a fork of
  [tv-vibehack](https://github.com/Gathin23/tv-vibehack) by Gathin
- `mills-home-bridges`: the read-only home bridges and dashboard that feed it

---

By JJ Mills, for BREAKINBREAD INSTITUTE INC. Written with help from Claude.

## Licence

Writing is [CC BY 4.0](LICENSE-docs): share and adapt it with credit to BREAKINBREAD INSTITUTE INC. Code and snippets are [MIT](LICENSE-code). See [LICENSE](LICENSE).
