# Reverse engineering: US legal and ethical frame (research notes, Oct 20, 2026)

These are research notes, not legal advice. Sources were read on 2026-10-20. Check each case's exact wording before relying on it.

## The laws
- **DMCA §1201(a)(1)(A)** ([17 U.S.C. §1201](https://www.law.cornell.edu/uscode/text/17/1201)): you may not get around a technical measure that controls access to a copyrighted work, such as encryption, signature checks or license checks. This applies even when nothing is copied. §1201(a)(2) and (b) also ban making or sharing tools that get around these measures.
- **§1201(f), the interoperability exception:** someone who lawfully owns a copy of a program may get around these measures *only* to find what's needed to make their own, independently written program work with it. Three conditions apply: the information must not already be available, interoperability must be the only purpose, and nothing may be infringed. Other laws, such as contracts and the CFAA, still apply.
  - In *Davidson v. Jung* (Blizzard v. bnetd, 8th Cir. 2005), the developers lost: the court held §1201(f) didn't cover them and enforced the EULA's ban on reverse engineering.
- **Fair use:** two cases held that taking a program apart for interoperability can be fair use: *Sega v. Accolade* (9th Cir. 1992) and *Sony v. Connectix* (9th Cir. 2000, [opinion](https://law.resource.org/pub/us/case/reporter/F3/203/203.F3d.596.99-15852.html)). But fair use is **not** a defense against a §1201 claim.
- **Contracts can override these rights:** in *Bowers v. Baystate* (Fed. Cir. 2003, [opinion](https://law.resource.org/pub/us/case/reporter/F3/320/320.F3d.1317.01-1109.01-1108.html)), a no-reverse-engineering clause was upheld. Always read the EULA and terms first.
- **Trade secrets:** the Defend Trade Secrets Act, [18 U.S.C. §1839(6)(B)](https://www.law.cornell.edu/uscode/text/18/1839), says reverse engineering is not "improper means", but only when the copy was obtained lawfully and no contract was broken.
- **Copyright Office exemptions** ([37 CFR §201.40](https://www.law.cornell.edu/cfr/text/37/201.40), in force since 2024-10-28):
  - **(b)(18), good-faith security research:** covers devices you lawfully own, or systems whose owner has authorized it. The work must be done in an environment that avoids harm, and it must be mainly to improve security. It is not a defense under other laws, and it does not allow sharing circumvention tools.
  - **Repair and diagnosis:** (b)(13)-(17).
  - **The next rulemaking** is under way, docket COLC-2026-0100 ([copyright.gov/1201/2027](https://www.copyright.gov/1201/2027/)). The new rule is expected around October 2027, so re-check then.
- **CFAA:** *Van Buren* (2021, [decision](https://www.law.cornell.edu/supremecourt/text/19-783)) asks whether you were allowed into that part of the system at all. The 2022 DOJ policy says prosecutors should decline good-faith security research. That policy doesn't bind civil lawsuits or state laws. The justice.gov text is *not confirmed*: the page couldn't be loaded.

## Draft rules of engagement for Mills OS (to agree with JJ)
1. **Targets:** only what JJ owns or has lawfully licensed, his own code, practice targets built to be broken, or systems where the owner has given written permission. Never anyone else's servers or networks.
2. **Before any work, write down:**
   - where the copy came from
   - what the EULA or terms say
   - the purpose: interoperability, security or repair
   - whether the information is already documented
3. **Approval:** SENTINEL signs off on any task that involves getting around a protection, and names the legal basis for it.
4. **Where the work happens:** in an isolated sandbox with no outbound network.
5. **Sharing:** nothing is published (tools, patched files, keys) without JJ's review.
6. **Security findings** go to the vendor first.
7. **Off-limits:** cheats, piracy, getting around paid subscriptions, and stripping DRM from media.
8. **Recheck** the exemption wording after the 2027 rulemaking.

This file covers only the legal and ethical frame. Tools and a curriculum are not researched yet.
