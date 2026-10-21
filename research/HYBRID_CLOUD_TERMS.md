# Hybrid cloud terms check for Mills OS (local = private, cloud = no personal data)

Research date: 2026-10-20. Read-only web research. No sign-ins, sign-ups or downloads by hand (two public PDFs were cached automatically by the fetch tool and read locally as text). "Not confirmed" means no primary source said it plainly.

Access notes: openai.com, help.openai.com, hhs.gov and americanbar.org returned HTTP 403 to direct fetches. For those pages, the claims below come from search-result excerpts of the official URL listed. That is weaker evidence than a full read and is marked **(search excerpt)**.

---

## Bottom line

| Cloud source | Cost / free limit | Trains on your data? | Retention | Agent / automated use | BAA / regulated data | Fit for Mills OS |
|---|---|---|---|---|---|---|
| Nous Portal (incl. free `:free` models, Hermes free tier / guest onboarding) | Free tier: free models only, limits not published | **Yes by default**; "Privacy Mode" opt-out, applies going forward only | No period stated | Allowed in Hermes; terms limit "automatic" requests | No BAA, no HIPAA mention | Public / non-personal tasks only |
| ChatGPT Plus/Pro | $20 / $100-500 per month | Plus/Pro: yes unless you turn off "Improve the model for everyone" | Not confirmed for plan-usage in third-party apps | Codex CLI: yes. Hermes is a listed "ChatGPT plan usage" partner | Not BAA-eligible | OK for coding/building on non-personal data |
| ChatGPT Business | $20-25 per user per month | No by default | Chats kept until deleted | Codex: yes; "plan usage in other apps" is listed only for Go/Plus/Pro | **Not BAA-eligible** | OK for non-personal work; not regulated data |
| Claude Pro/Max (Claude Code) | Subscription | User setting; on = de-identified data kept up to 5 years | 30-day deletion after you delete; longer if flagged | Claude Code: yes. Third-party harness: allowed for now, conditions apply (see 3) | No (consumer plans not ZDR or BAA) | Good as the builder on code, never on client data |
| NVIDIA build.nvidia.com (hosted trial) | Free trial, per-model rate limits, not published | **Terms allow use of inputs/outputs to improve products "including AI models"** | Session only, but logged for security/abuse | Trial/testing only, **no production** | **Terms forbid PHI, card data, personal data** | Testing only, never real data |
| OpenAI API | Pay per token | No by default | Abuse logs up to 30 days; ZDR/MAM by approval | Yes | Yes: self-serve BAA for eligible orgs, Modified Retention required | Candidate for regulated work under a BAA |
| Anthropic API | Pay per token | No by default | 30 days (privacy center) / not retained by default except Covered Models (docs); ZDR via sales | Yes | Yes: self-serve BAA + "HIPAA readiness" in Console for eligible orgs | Candidate for regulated work under a BAA |

The pattern: only the **paid APIs with a signed BAA and the right retention settings** come close to handling regulated data. Every subscription seat and every free tier should be treated as "public data only".

---

## 1. Nous Research: Nous Portal and free models in Hermes

**Sources**
- Nous Portal Privacy Policy, "Last Updated: September 28, 2026": https://portal.nousresearch.com/privacy
- Nous Portal Terms of Service, "Last update: September 28, 2026": https://portal.nousresearch.com/terms
- Hermes Agent releases (v0.21.2 to v0.22.2): https://github.com/NousResearch/hermes-agent/releases
- Hermes provider docs: https://hermes-agent.nousresearch.com/docs/integrations/providers
- Hermes Nous Portal guide: https://hermes-agent.nousresearch.com/docs/guides/run-hermes-with-nous-portal
- Pricing page https://portal.nousresearch.com/pricing returned HTTP 429, so it was not read.

**Cost / free limits**
- Hermes v0.21.2 (Sept 11, 2026) added "Nous free tier and guided first launch", with the guided launch behind `HERMES_GUEST_ONBOARDING=1` ([releases](https://github.com/NousResearch/hermes-agent/releases)).
- Hermes v0.22.0 (Oct 13, 2026) says a fresh desktop install opens a setup chat on a free trial model (`nous/welcome`, once per OS user) that needs no key ([releases](https://github.com/NousResearch/hermes-agent/releases)).
- **`nous.guest` as a name: not confirmed.** It does not appear in the public docs or release notes I could read, or in the local Hermes checkout at `~/.hermes/hermes-agent` (commit dated Sept 1, 2026). The closest public items are the v0.21.2 guest-onboarding flag and v0.22.0's `nous/welcome` trial model.
- Free-tier limits: **not published** (not confirmed). Terms §5.6 say free trials are not guaranteed, limited to one per person or entity, and may need identity or payment checks ([terms](https://portal.nousresearch.com/terms)).

**Training / opt-out**
- The Privacy Policy says prompts, inputs, outputs, telemetry and trace data may be used to train models, and may be shared with affiliates, research collaborators and model providers to "train, fine-tune, evaluate" ([privacy](https://portal.nousresearch.com/privacy)).
- Opt-out: "Privacy Mode" (Account Settings > Privacy Settings). With it on, Nous says "we will not store your inference payloads", with security, legal and fraud exceptions. It "may apply only on a prospective basis" and does not change what third-party providers do under their own terms ([privacy](https://portal.nousresearch.com/privacy)). Terms §12.3(a) and (d) say the same ([terms](https://portal.nousresearch.com/terms)).

**Retention**
- No retention period is stated ([privacy](https://portal.nousresearch.com/privacy)).

**Automated / agent use**
- Use through Hermes is the intended path. The terms ban creating accounts by automated means, restrict use "by means of programs that send them automatic enquiries or requests", and ban directing agents toward malicious or spam behavior ([terms](https://portal.nousresearch.com/terms)). How "automatic enquiries" applies to Hermes agents on the Portal: **not confirmed**.

**BAA / regulated data**
- No mention of HIPAA, BAAs or sensitive-data handling in the policy or terms ([privacy](https://portal.nousresearch.com/privacy), [terms](https://portal.nousresearch.com/terms)).

**Bad-fit flags**
- Training by default. No retention period. Prompts may pass to third-party model providers. No BAA. Service is "AS IS" (terms §14). **Use only for public or synthetic data.**

---

## 2. ChatGPT seats (Plus / Pro / Business / Team) with Codex CLI or Hermes

**Sources**
- Codex auth docs (developers.openai.com/codex/auth redirects to): https://learn.chatgpt.com/docs/auth
- Codex pricing (developers.openai.com/codex/pricing redirects to): https://learn.chatgpt.com/docs/pricing
- Sign in with ChatGPT (consumer page): https://learn.chatgpt.com/docs/sign-in-with-chatgpt
- Sign in with ChatGPT developer docs: https://developers.openai.com/siwc and https://developers.openai.com/siwc/token-sharing-open-source
- OpenAI HIPAA Implementation and Configuration Guide (PDF metadata July 9, 2026): https://cdn.openai.com/osa/baa-hipaa-guide.pdf
- Help center (search excerpts; direct fetch 403): https://help.openai.com/en/articles/8798634-managing-data-sharing-and-privacy-in-chatgpt-business ; https://help.openai.com/en/articles/7730893-data-controls-faq ; https://help.openai.com/en/articles/20001542-using-your-chatgpt-plan-in-other-apps-and-sites ; https://help.openai.com/en/articles/20001069-hipaa-eligible-products-and-functionality
- Hermes provider docs: https://hermes-agent.nousresearch.com/docs/integrations/providers

**Cost**
- Plus $20/month; Pro $100, $200 or $500/month; Business $20 per user per month (annual, 2+ users) or $25 monthly; Enterprise/Edu: contact sales ([pricing](https://learn.chatgpt.com/docs/pricing)).
- Plus and Business Standard have 5-hour and weekly Codex limits. Pro currently has no 5-hour limit. API-key use is billed separately at API rates ([pricing](https://learn.chatgpt.com/docs/pricing)).

**Codex CLI with a ChatGPT seat**
- Codex supports "Sign in with ChatGPT for subscription access" or an API key ([auth](https://learn.chatgpt.com/docs/auth)).
- For CI/CD and programmatic use, OpenAI recommends API keys. Codex access tokens for "trusted scripts, schedulers, and private CI runners" are a Business/Enterprise feature ([auth](https://learn.chatgpt.com/docs/auth), [pricing](https://learn.chatgpt.com/docs/pricing)). Treat `~/.codex/auth.json` like a password ([auth](https://learn.chatgpt.com/docs/auth)).

**ChatGPT seat inside Hermes**
- OpenAI's own page lists **Hermes Agent** under "Apps with ChatGPT plan usage" ([sign-in-with-chatgpt](https://learn.chatgpt.com/docs/sign-in-with-chatgpt)).
- That page names "eligible ChatGPT Plus and Pro subscribers". The help center excerpt says plan usage in commercial apps is for Go, Plus and Pro **(search excerpt)** ([help 20001542](https://help.openai.com/en/articles/20001542-using-your-chatgpt-plan-in-other-apps-and-sites)). **Business/Team: not listed, so not confirmed.**
- Per-app caps of 10-100% of weekly plan usage are set in Settings > Usage. Connecting an app does not add usage, and the app does not get your ChatGPT chats or memories ([sign-in-with-chatgpt](https://learn.chatgpt.com/docs/sign-in-with-chatgpt)).
- Developer docs cover plan usage for "open-source and locally hosted apps". Paid or remotely hosted apps need an interest form ([siwc open-source](https://developers.openai.com/siwc/token-sharing-open-source)).
- Hermes' own docs say its ChatGPT/Codex provider uses device-code OAuth, stores tokens in `~/.hermes/auth.json`, and that how usage counts against plan limits is "not currently documented" ([Hermes providers](https://hermes-agent.nousresearch.com/docs/integrations/providers)). Whether Hermes v0.21.7 uses the official Sign in with ChatGPT flow or the older Codex OAuth client: **not confirmed**.
- OpenAI's general Terms of Use ban "automatically or programmatically" extracting data or Output **(search excerpt)** ([terms](https://openai.com/policies/terms-of-use/)). How that applies to a listed partner app: not confirmed.

**Training / retention**
- Plus/Pro: training is on unless you turn off "Improve the model for everyone". On a personal plan that setting also covers Codex tasks **(search excerpt)** ([data controls FAQ](https://help.openai.com/en/articles/7730893-data-controls-faq)).
- Business: workspace data is excluded from training by default, including Codex, under the Business Terms and Enterprise Privacy commitments **(search excerpt)** ([help 8798634](https://help.openai.com/en/articles/8798634-managing-data-sharing-and-privacy-in-chatgpt-business)).
- With ChatGPT sign-in, Codex follows workspace retention settings. With an API key, it follows the API org's settings ([auth](https://learn.chatgpt.com/docs/auth)).
- Retention for plan-usage requests made from third-party apps: **not confirmed**. The developer example uses `store: false`, but no policy text was found ([siwc open-source](https://developers.openai.com/siwc/token-sharing-open-source)).

**BAA**
- The HIPAA Guide says "ChatGPT services for consumers (e.g., Free, Plus, Pro) and ChatGPT Business are not Eligible Services" ([HIPAA guide](https://cdn.openai.com/osa/baa-hipaa-guide.pdf)).
- Codex Local is covered only through an API key under a BAA, or through a ChatGPT eligible workspace where an Account Director enables HIPAA for Codex Local ([HIPAA guide](https://cdn.openai.com/osa/baa-hipaa-guide.pdf)).

**Bad-fit flags**
- No BAA on any of these seats. Plus/Pro train by default. Business is not confirmed for third-party plan usage. **Fine for building and non-personal tasks. Never for client data.**

---

## 3. Claude seats (Pro / Max / Team) with Claude Code and Hermes

**Sources**
- Consumer Terms (effective Oct 8, 2025): https://www.anthropic.com/legal/consumer-terms
- Claude Code legal and compliance: https://code.claude.com/docs/en/legal-and-compliance
- "Use the Claude Agent SDK with your Claude plan" (notices dated June 15 and Oct 7, 2026): https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan
- Consumer retention (updated July 1, 2026): https://privacy.claude.com/en/articles/10023548-how-long-do-you-store-my-data
- Commercial training default: https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training
- Hermes provider docs: https://hermes-agent.nousresearch.com/docs/integrations/providers

**Claude Code as the builder: allowed**
- Claude Code on Free/Pro/Max falls under the Consumer Terms, and on Team/Enterprise under the Commercial Terms ([legal](https://code.claude.com/docs/en/legal-and-compliance)).
- OAuth sign-in is for subscribers and "ordinary use of Claude Code and other native Anthropic applications". Pro/Max limits assume "ordinary, individual usage of Claude Code and the Agent SDK" ([legal](https://code.claude.com/docs/en/legal-and-compliance)).

**Subscription sign-in inside a third-party harness like Hermes: allowed for now, with conditions**
- The Consumer Terms ban access "through automated or non-human means" "except when you are accessing our Services via an Anthropic API Key or where we otherwise explicitly permit it" ([consumer terms](https://www.anthropic.com/legal/consumer-terms)).
- The Claude Code docs say developers building products, including on the Agent SDK, "should use API key authentication". Anthropic does not let third-party developers offer Claude.ai login, "route requests through Free, Pro, or Max plan credentials on behalf of their users", or "collect, store, or intermediate Claude.ai credentials or session tokens". Anthropic "may" enforce this "without prior notice" ([legal](https://code.claude.com/docs/en/legal-and-compliance)).
- Anthropic's support page (Oct 7, 2026 notice) says you "can still use ... third-party apps with your subscription limits". The changes announced for the Agent SDK were paused on June 15, 2026 ([support 15036540](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)).
- Hermes' docs say its Anthropic subscription OAuth path "only works if you're on a Claude Max plan and have purchased extra usage credits" ([Hermes providers](https://hermes-agent.nousresearch.com/docs/integrations/providers)). This does not fully match Anthropic's Oct 7 wording, so the current billing behavior is **not confirmed**.
- **Practical reading:** an always-on, multi-agent Hermes setup is not "ordinary, individual usage", and Hermes stores the token itself in `~/.hermes/auth.json`. **Use an Anthropic API key for Hermes.** Keep the Pro/Max sign-in for Claude Code itself.

**Training / retention (consumer Pro/Max)**
- Training is a user setting. When it is on, de-identified data may be kept "for up to 5 years". Deleted chats are removed from back-end storage within 30 days. Flagged chats are kept up to 2 years, and safety scores up to 7 years ([consumer retention](https://privacy.claude.com/en/articles/10023548-how-long-do-you-store-my-data)).
- Team (Claude for Work) is not used for training by default, except feedback or an opt-in program ([training article](https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training)).

**BAA**
- Consumer plans are not ZDR-eligible ([API retention](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)). The BAA covers the API, Microsoft Foundry and Enterprise. Team is not named ([BAA article](https://privacy.claude.com/en/articles/8114513-business-associate-agreements-baa-for-commercial-customers)).

**Bad-fit flags**
- No BAA on Pro/Max/Team, and Pro/Max may train if the setting is on. **Use Claude Code to build. Never point it at real client data.**

---

## 4. NVIDIA hosted API (build.nvidia.com / NIM API catalog)

**Sources**
- NVIDIA API Trial Terms of Service, "v. September 19, 2025" (linked from every model page): https://assets.ngc.nvidia.com/products/api-catalog/legal/NVIDIA%20API%20Trial%20Terms%20of%20Service.pdf
- NVIDIA staff on credits, rate limits and production (forum, Sept 10 and Sept 17, 2025): https://forums.developer.nvidia.com/t/request-more-4-000-credits-option-on-build-nvidia-com/344567
- Model page (example): https://build.nvidia.com/nvidia/llama-3_3-nemotron-super-49b-v1_5
- DGX Spark NVIDIA AI Enterprise quick start: https://docs.nvidia.com/dgx/dgx-spark/nvaie-quickstart.html
- Developer Program NIM access: https://developer.nvidia.com/blog/access-to-nvidia-nim-now-available-free-to-developer-program-members/ and https://docs.api.nvidia.com/nim/docs/product

**Cost / limits**
- NVIDIA staff (Sept 10, 2025): "We no longer use a credit-based system for build.nvidia.com." Rate limits vary per model, are not published, and are shown in the top right of the site ([forum](https://forums.developer.nvidia.com/t/request-more-4-000-credits-option-on-build-nvidia-com/344567)). The "40 RPM" figure appears only in user posts, not confirmed.
- The terms still describe credits (§1.4) ([trial ToS](https://assets.ngc.nvidia.com/products/api-catalog/legal/NVIDIA%20API%20Trial%20Terms%20of%20Service.pdf)).

**Production use: not allowed on the trial**
- §1.2: access is "for limited trial purposes only and without use of the API Service or Generated Content in production". §1.4: production needs a separate Subscription ([trial ToS](https://assets.ngc.nvidia.com/products/api-catalog/legal/NVIDIA%20API%20Trial%20Terms%20of%20Service.pdf)).
- NVIDIA staff: production requires an NVIDIA AI Enterprise license ([forum](https://forums.developer.nvidia.com/t/request-more-4-000-credits-option-on-build-nvidia-com/344567)).

**Logging / training**
- §2.3: NVIDIA "will not store or use User Content or Generated Content at the end of each API Service session" unless expressly disclosed.
- But §3.3 says NVIDIA will collect "User Content and Generated Content to improve NVIDIA products and services, including AI models", and that use "will be logged for security, fraud or abuse monitoring and shared with third party service providers".
- §2.4: fine-tuning content is kept 30 days (inputs) and 90 days (outputs).
- These two sections pull in different directions, so **assume prompts can be used to improve models**. No opt-out was found ([trial ToS](https://assets.ngc.nvidia.com/products/api-catalog/legal/NVIDIA%20API%20Trial%20Terms%20of%20Service.pdf)).

**Regulated data: explicitly prohibited**
- §2.6(a): no "confidential information, controlled or sensitive data, including protected health information, personal data ..., payment card industry information".
- §4.3: no personal, "financial, health or governmental information". NVIDIA "specifically disclaims" that its servers are fit for PCI DSS or HIPAA data.
- §2.6(h) also bans "any robot, spider, data scrapping or extraction tool". Liability is capped at US$10 (§13.2) ([trial ToS](https://assets.ngc.nvidia.com/products/api-catalog/legal/NVIDIA%20API%20Trial%20Terms%20of%20Service.pdf)).

**DGX Spark owners**
- NVIDIA AI Enterprise for DGX Spark is "available only if you purchased it", requested an evaluation, or received an entitlement certificate. A 90-day trial is offered ([Spark NVAIE](https://docs.nvidia.com/dgx/dgx-spark/nvaie-quickstart.html)).
- NIM containers can be pulled through the free Developer Program or NVIDIA AI Enterprise ([Spark NVAIE](https://docs.nvidia.com/dgx/dgx-spark/nvaie-quickstart.html)). Developer Program self-hosted NIM is for research, development and testing on up to 16 GPUs ([NIM FAQ](https://docs.api.nvidia.com/nim/docs/product)).
- **No extra hosted-API credits for Spark owners** were found (not confirmed). The real Spark benefit is running NIM **locally**, which keeps private data on the box. Production use of NIM containers still points to an NVIDIA AI Enterprise license.

**Bad-fit flags**
- Testing only, no production, no personal data, possible training use. **Never send client data to build.nvidia.com.**

---

## 5. Comparison: OpenAI API and Anthropic API

### OpenAI API
Sources: https://developers.openai.com/api/docs/guides/your-data ; https://cdn.openai.com/osa/baa-hipaa-guide.pdf ; https://help.openai.com/en/articles/8660679 (search excerpt)

- **Training:** "data sent to the OpenAI API is not used to train or improve OpenAI models" unless you opt in ([your-data](https://developers.openai.com/api/docs/guides/your-data)).
- **Retention:** abuse-monitoring logs are kept "for up to 30 days". Responses and Chat Completions are stored at least 30 days by default, or when `store` is true ([your-data](https://developers.openai.com/api/docs/guides/your-data)).
- **ZDR / Modified Abuse Monitoring:** both need prior OpenAI approval. Under ZDR, `store` is forced to false. Assistants, Threads, Vector Stores, Conversations, Files, Fine-tuning, Evals and Batches are **not** ZDR-eligible ([your-data](https://developers.openai.com/api/docs/guides/your-data)).
- **BAA:** self-serve in the API Platform for orgs with an established API usage history, or by email for custom terms. Once on, it cannot be turned off **(search excerpt)** ([help 8660679](https://help.openai.com/en/articles/8660679)).
- Under the BAA, API use "currently requires" a Modified Retention feature, used with HIPAA input and output only ([HIPAA guide](https://cdn.openai.com/osa/baa-hipaa-guide.pdf)). Web Search with live internet "is not HIPAA eligible" ([your-data](https://developers.openai.com/api/docs/guides/your-data)).

### Anthropic API
Sources: https://platform.claude.com/docs/en/manage-claude/api-and-data-retention ; https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data (updated July 1, 2026) ; https://privacy.claude.com/en/articles/8114513-business-associate-agreements-baa-for-commercial-customers ; https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training

- **Training:** commercial and API data is not used for training by default ([training](https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training)). "Retained data is never used for model training without your express permission" ([API retention](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)).
- **Retention: the two sources differ.**
  - The privacy center says inputs and outputs are deleted "within 30 days". Flagged content is kept up to 2 years, and scores up to 7 years ([commercial retention](https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data)).
  - The platform docs say conversation content "is not retained by default", except Covered Models (Claude Fable 5/5.1, Mythos 5/5.1), which require 30-day retention ([API retention](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)).
  - Treat 30 days as the working figure.
- **ZDR:** available through sales, enabled per organization. It does not cover the Console, Managed Agents, consumer plans, Team/Enterprise interfaces or Covered Models ([API retention](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)).
- **BAA / HIPAA readiness:** eligible orgs can sign the standard BAA and turn on HIPAA readiness in the Console. Once on, it is permanent. Non-eligible features return a 400 error. HIPAA readiness and general use need separate orgs. Claude Code is "not covered under HIPAA readiness" on the API path ([API retention](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention)).
- **BAA exclusions:** Batch, Files, Skills, Code Execution, Computer Use, Web Fetch, and MCP/connectors ([BAA article](https://privacy.claude.com/en/articles/8114513-business-associate-agreements-baa-for-commercial-customers)).

---

## 6. What "certified" actually means

### HIPAA
- **There is no official HIPAA certification.** HHS "does not endorse or otherwise recognize" private "certifications" for the Security Rule, and they do not excuse a covered entity. HHS/OCR do not certify any person or product as "HIPAA compliant" **(search excerpt)** ([HHS FAQ](https://www.hhs.gov/hipaa/for-professionals/faq/2003/are-we-required-to-certify-our-organizations-compliance-with-the-standards/index.html); [HHS misleading-claims notice](https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/be-aware-misleading-marketing-claims/index.html)).
- Self-certification or third-party certification does not replace the business associate contract **(search excerpt)** ([HHS FAQ 237](https://www.hhs.gov/hipaa/for-professionals/faq/237/can-business-associates-self-certify/index.html)).
- **Common third-party attestations:**
  - HITRUST e1, i1 and r2 are nested assessments. HITRUST itself says even an r2 should not simply be submitted to prove HIPAA Security Rule compliance ([HITRUST advisories](https://hitrustalliance.net/advisories/haa-2026-005-csf-version-11.9.0-release); [HITRUST and HIPAA](https://hitrustalliance.net/hubfs/Thought%20Leadership/HITRUST-and-HIPAA.pdf)).
  - SOC 2 is an AICPA CPA examination of controls for security, availability, processing integrity, confidentiality and privacy ([AICPA SOC 2](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2/)). The Type 1 vs Type 2 definitions were not confirmed from the AICPA page.
- **BAA requirement:** any vendor that creates, receives, maintains or transmits PHI for you is a business associate. That includes a cloud provider holding only encrypted PHI without the key **(search excerpt)** ([HHS cloud guidance](https://www.hhs.gov/hipaa/for-professionals/special-topics/health-information-technology/cloud-computing/index.html)).
- Under 45 CFR 164.504(e)(2), the contract must set permitted uses and disclosures; ban other uses; require Subpart C safeguards; require reporting of improper uses and breaches; pass the same terms to subcontractors; support access, amendment and accounting; open books to HHS; and return or destroy PHI at the end ([eCFR 164.504](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-E/section-164.504)).
- **For Mills OS:** a Mills OS that touches a hospital's PHI is itself a business associate. That means a BAA with the hospital, and a sub-BAA with any cloud model that sees PHI.

### PCI DSS
- Tokenization "does not replace" PCI DSS compliance, but can cut the number of systems in scope. The token system and anything that can turn a token back into a card number stay in scope ([PCI tokenization supplement](https://listings.pcisecuritystandards.org/documents/Tokenization_Guidelines_Info_Supplement.pdf)).
- The smallest scope is SAQ A: card-not-present only, all account-data functions outsourced to a PCI DSS-compliant provider, no electronic storage, processing or transmission of card data on your systems. v4.0.1 adds a script-attack criterion for pages with embedded payment forms ([SAQ A v4.0.1](https://docs-prv.pcisecuritystandards.org/SAQ%20(Assessment)/SAQ/PCI-DSS-v4_0_1-SAQ-A-r1.pdf); [PCI FAQ 1588](https://www.pcisecuritystandards.org/faqs/1588/)). SAQ A does not apply to service providers.
- **For Mills OS:** never let any model, local or cloud, see a full card number. Use processor-hosted fields or links, and keep only processor tokens and last-4.

### FERPA (schools)
- A vendor can get education records without consent only as a "school official". It must do work the school would otherwise do, stay under the school's "direct control", and follow FERPA's limits on reuse and redisclosure (34 CFR 99.31(a)(1)(i)(B)) ([eCFR 99.31](https://www.ecfr.gov/current/title-34/subtitle-A/part-99/subpart-D/section-99.31)).
- The Department of Education warns that click-wrap terms the vendor can change without notice make "direct control" hard to show ([PTAC cloud FAQ](https://studentprivacy.ed.gov/sites/default/files/resource_document/file/FAQ_Cloud_Computing_0.pdf); [PTAC online services guidance](https://studentprivacy.ed.gov/sites/default/files/resource_document/file/Student%20Privacy%20and%20Online%20Educational%20Services%20(February%202014)_0.pdf)).
- There is **no FERPA certification**. What matters is a signed contract.

### FCRA and GLBA (credit reports)
- **FCRA §604 (15 U.S.C. 1681b):** a consumer reporting agency may furnish a report "under the following circumstances and no other", and users need a permissible purpose ([FTC FCRA text, March 2026](https://www.ftc.gov/system/files/ftc_gov/pdf/fcra-march-2026.pdf); [CFPB FCRA manual](https://files.consumerfinance.gov/f/documents/102012_cfpb_fair-credit-reporting-act-fcra_procedures.pdf)). Each report pulled and processed must tie back to the client's permissible purpose.
- **GLBA Safeguards Rule (16 CFR 314.4):** requires a written security program. §314.4(f) requires choosing service providers able to keep safeguards, binding them by contract, and assessing them periodically ([eCFR 314](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-314)). The specific (c)(3) encryption and (j) FTC-notice subsections were not re-read this session (eCFR redirected); not confirmed here.

### Law firms: ABA Formal Opinion 512 (July 29, 2024)
- Sources: [ABA news release](https://www.americanbar.org/news/abanews/aba-news-archives/2024/07/aba-issues-first-ethics-guidance-ai-tools/); [Opinion PDF](https://www.americanbar.org/content/dam/aba/administrative/professional_responsibility/ethics-opinions/aba-formal-opinion-512.pdf); [Business Law Today summary](https://www.americanbar.org/groups/business_law/resources/business-law-today/2024-october/aba-ethics-opinion-generative-ai-offers-useful-framework/). All are **(search excerpt)**; the PDF returned 403.
- For self-learning AI tools, lawyers should get the client's **informed consent** before putting information about the representation into prompts, even inside a firm's "closed system".
- Boilerplate in an engagement letter is **not** enough.
- Lawyers must understand the tool's terms and data handling (competence and confidentiality, Rules 1.1 and 1.6). The opinion is advisory, not binding; state bar rules govern.
- **For Mills OS:** client matter content stays local unless the firm gets specific informed consent and the vendor terms rule out training and retention.

---

## 7. Limits of automatic PII redaction (Presidio, NeMo Guardrails) as the only gate

- **Presidio says so itself:** "there is no guarantee that Presidio will find all sensitive information", so "additional systems and protections should be employed". It works by NER, regex, rules, checksums and context ([Presidio README](https://github.com/microsoft/presidio)). The README says the project "has moved to a new home" under a data-privacy-stack organization.
- **NeMo Guardrails' sensitive-data rails use Presidio:**
  - Default `score_threshold` is 0.2. Raising it reduces false positives, which trades toward more misses.
  - The setup needs the English spaCy model `en_core_web_lg`. Other languages are not documented.
  - Retrieval rails work only on LLMRails.
  - "Detect" refuses to answer; "mask" replaces the PII and carries on ([NeMo Presidio page](https://docs.nvidia.com/nemo/guardrails/configure-guardrails/guardrail-catalog/third-party/presidio)).
  - Missed entities are referred to the Presidio FAQ ([same page](https://docs.nvidia.com/nemo/guardrails/configure-guardrails/guardrail-catalog/third-party/presidio)).
- **Redaction is not de-identification.**
  - HIPAA de-identification means either Safe Harbor (remove 18 identifier types and have no actual knowledge the rest can identify someone) or Expert Determination (re-identification risk "very small") **(search excerpt)** ([HHS de-identification guidance](https://www.hhs.gov/hipaa/for-professionals/special-topics/de-identification/index.html)).
  - A regex/NER pass gives neither by itself. Dates, ZIP codes, rare diagnoses, case facts and free-text context can still identify a person.
- **Contract rules still apply after redaction:**
  - NVIDIA trial terms ban "confidential information" generally, not just PII ([trial ToS](https://assets.ngc.nvidia.com/products/api-catalog/legal/NVIDIA%20API%20Trial%20Terms%20of%20Service.pdf)).
  - ABA 512 is about any information "related to the representation", not just names (search excerpt, see 6).
- **Recommendation:**
  - Use a default-deny **routing** rule: data tagged hospital, law, school or credit never leaves local models.
  - Use Presidio/NeMo only as a second net on the "public" lane, never as the gate that lets regulated data go out.
  - Log every cloud call for audit.

---

## Open items (not confirmed)
1. What exactly `nous.guest` is in Hermes v0.21.7, and the Nous free-tier rate limits.
2. Whether ChatGPT **Business** seats can use "plan usage" in Hermes, and how long OpenAI keeps plan-usage requests from third-party apps.
3. Whether Hermes' Anthropic OAuth path bills subscription limits or extra usage today (Hermes docs and Anthropic's Oct 7, 2026 notice differ).
4. Anthropic API default retention: "within 30 days" (privacy center) vs "not retained by default" (platform docs).
5. NVIDIA trial terms §2.3 vs §3.3 on whether inputs are kept to improve models.
6. Full text of ABA Opinion 512, HHS FAQs and OpenAI help articles, which were read only as search excerpts because direct fetches were blocked (403).
