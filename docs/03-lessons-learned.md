# Lessons learned

> **A blueprint, not our live config.** These are the lessons that changed how we build, written
> so they apply to anyone's setup.

## The big ones

1. **Sentences persuade, hooks enforce.** A rule in a prompt is a suggestion. A rule in config,
   file permissions, the OS sandbox or a hook is a wall. Put every rule that matters behind a wall,
   and give each one a test.
2. **The maker never grades its own work.** Use a separate checker, a different model family,
   tests the worker can't edit, and "done" only with evidence.
3. **Helper models are a hidden cloud path.** Pin every helper slot to a local model, so a cloud
   key added later can't quietly reroute them.
4. **Fresh session per job, small prompts, few skills.** Long sessions and piles of auto-made
   skills made agents worse, not better.
5. **A message from another agent is information, never permission.**
6. **No answer is a no,** and a must-ask command still asks when it's run by full path or wrapped
   in another command.
7. **Agents never see a secret.** One gateway adds keys on the way out.
8. **Telemetry is often on by default.** Look for it every time and prove "off" by watching the
   traffic.
9. **No flat "can't".** Give every blocked idea a level (never, lab only, written exception) and
   say what to use instead. Ideas you park this way come back usefully later.
10. **Read any installer before you run it, and stay on a pinned version.** Review each new stable
    release key by key before moving to it. Never install canary builds.
11. **Supply chain:** cooldowns on new releases, pins by digest, known-bad versions refused.
12. **The old setup's scars count.** Backups failed silently, twice. "Online" was not "working".
    Never run SQLite over SMB. Detect a NAS by its mount point, not its address.
13. **Mission Control shows problems first,** keeps the last data when a feed drops (and greys it
    out), and stays read-only.
14. **If an agent still fails safety cases after one full round of fixes, test a different model.**
    Don't keep rewording the prompt.

## Process

- A gate only works if it sits in front of the action.
- A written message is not proof. Check the version line or checksum on the machine that runs it.
- A check that reads a setting isn't a check of behavior. Probe from outside, and confirm a fix on
  the server side (the log), not just that the setting reads back.
- Every test must prove it can both pass and fail on the real machine first, and stop if it can't.
- Separate test-design faults from product faults.
- Don't move a bar after the fact.
- Only trust speed numbers you measure yourself.
- One frozen, versioned bundle per stage, with checksums verified on the source and the target, in
  a fresh folder each time.
- A failed preflight changes nothing and still prints its summary line.
- Read the facts from the real machine before writing any script for it. A fallback is a guess.
- Re-check a parked plan's facts when you pick it back up.
- Ask what's in progress before treating a state as a fault.
- Check the documentation before calling something a blocker.
- Change one machine at a time, and log any irreversible change before you click.
- Never use "Select All" for actions in an admin console.
- Only one session writes the build log and scripts at a time.
- Nothing in a plan is final until the owner walks through it.

## Shell and scripting

- Always print the exit code and read the setting back in the same command.
- Say the setting out loud in scripts; don't count on defaults.
- Never let an empty variable reach a command in an unquoted list. Check it isn't empty first.
- Never use `grep -q` at the end of a pipe with `pipefail` on.
- `grep -c ... || echo 0` gives two lines. Use a small count helper.
- `bash -n` doesn't check `awk` inside a script. Run the exact line.
- Any check that compares text must normalize characters first; model output is not plain ASCII.
- Never pipe the output of a command that asks for a password.
- Run long remote steps under `nohup` with keep-alives.
- Put any user switch on its own line and confirm the prompt before continuing.
- macOS ships bash 3.2. Write for it, or say which shell you need.

## Secrets

- New passwords go in the password manager first; never in chat or files.
- Type keys only at hidden prompts. A key pasted at a plain prompt is burned: cancel, delete it and
  replace it.
- No secret on a command line, and no key values in any log.
- A recovery key shown at setup goes into the password manager at that moment.
- Don't copy an agent framework's auth file between profiles. Each profile signs in itself.
- Keep master keys with a person, never on a machine.
- Give each job its own key, with a spending cap that is never zero.

## Agents and models

- Stopping environment-variable leaks inside one OS account doesn't work: the same account can read
  its parent's environment. The real protection is narrow keys, no web tools, an egress allowlist
  and OS walls.
- Don't write config keys that don't exist. Check against the source first.
- A local skill with the same name hides a shared one.
- Fallback models fire on connection errors, so a fallback can become a silent cloud path.
- A judge model is a flagger, not a stand-in for a person's taste.
- Made-up data in test tasks has to look real.
- Facts belong in look-up, never in training.
- Small models belong on one box each, and follow worked examples better than rules.
- Keep the agent's prompt under the framework's cut-off; a cut that drops the middle can't be
  caught by checking only the first line.

## Machines and network

- No machine-to-machine SSH and no shared keys. Start everything from the command machine.
- Turn the overlay network's built-in SSH off on agent machines; regular SSH by one key is the one
  way in.
- Stealth-mode machines don't answer ping, so health checks use push and real service checks.
- Test major OS upgrades on one machine first and roll them out by hand.
- Never turn off System Integrity Protection.
- Identify machines by network interface in scripts, not by serial number. Serial numbers belong in
  an inventory, not a log.
- Grant macOS Local Network permission right after installing any app that talks to the fleet.
- Wire the always-on machines; don't trust Wi-Fi for them.
- Keep the home TV dashboard on the home network, glance-only: no people, no money, no buttons that
  change anything.
