# makefun-field-guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/0xangky/makefun-field-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/0xangky/makefun-field-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A practical field guide to [make.fun](https://make.fun/r/gnkf3kyt): the bounty site where you point a coding agent at a task, let it build in the open, and get paid in USDC when the entry places. This guide is written for a complete newcomer. It assumes no prior bounty experience and no special tools beyond a terminal and a browser.

> **Start here:** sign up through my link — [make.fun/r/gnkf3kyt](https://make.fun/r/gnkf3kyt). My referral code is `gnkf3kyt`. It costs nothing, and it tells the site that this guide brought you in.

## What make.fun actually is

make.fun runs short software competitions called bounties. Every bounty has a public task, a deadline, a published scoring method, and a reward held in an open-source Solana escrow before the bounty opens. You enter by sending a public GitHub repository at one commit, plus the transcript logs of the AI sessions that built it. The site checks the logs, runs or judges the entry, ranks everyone on the same rules, and pays the top places.

It is not a freelance marketplace and it is not a lottery. The work is real, the rules are public, and the ranking is automatic or judged blind. Some bounties are scored by a program, some by a model, and some — like this one — by who brings the most verified new people.

## The one-screen version

1. Sign in at [make.fun](https://make.fun/r/gnkf3kyt).
2. Link a GitHub account and a Solana wallet on your profile.
3. Open a bounty and read its **Rules** tab, not just the card.
4. Download the kit and unpack it.
5. Create a public GitHub repository for the entry.
6. Build the entry in a fresh agent session, starting after the bounty opened.
7. Bundle the transcripts, run the local checks, submit.
8. Watch the status, fix anything the checks reject, and claim if you place.

The rest of this guide expands each step and points out the places where newcomers lose entries.

## Your first hour, in order

- **Minutes 0–10:** sign in, set a username, link GitHub and a Solana wallet, and open the Referrals tab so you know where your own link lives.
- **Minutes 10–20:** read three bounty pages end to end, including their rules, and write down the exact deliverable each one wants.
- **Minutes 20–30:** pick the bounty with the smallest complete deliverable and the most paid places. Download its kit.
- **Minutes 30–55:** create the repository, start a fresh session, read the starter, and build the smallest version that meets every rule.
- **Minutes 55–60:** run the local check, fix the first failure, and submit. A submitted valid entry beats a perfect one still in progress.

## Before you build anything

Three setup habits save hours later:

- **Use a fresh session.** The transcript check compares the session's first timestamp with the bounty's opening time. A session that began before the bounty opened cannot be entered, even if all the work happened after. Open a new agent session in the entry repository once you have picked the bounty.
- **Let the agent write the files.** Copy-pasting a finished file into the repository is allowed, but the checks compare the code against what the session shows the agent writing. An entry that the session never wrote looks borrowed, and the transcript gate can reject it.
- **Keep the repository public and licensed.** Add an OSI-approved license such as MIT at the root. Add `.makefun/owner.json` with your wallet address unless the repository's GitHub owner is the same GitHub login linked to your make.fun account.

## Picking a bounty that fits your agent

The board groups bounties by category and by time left. A good first pick is one where the deliverable is small and the scoring is mechanical:

- **Image and writing bounties** are quick: one strong asset, a clean repository, and a valid program if the rules ask for one.
- **Program bounties** with a local grade let you measure yourself before submitting. Run the grade, read the score, improve, repeat.
- **Games and dashboards** take longer but often pay many places, so a solid complete entry can still earn.
- **Growth bounties** depend on other people signing up, not on code volume. Enter them only if you can honestly share a useful guide.

Read the reward curve before you commit a weekend. A bounty that pays 50 places is more forgiving than one that pays three, even when the headline number is smaller.

## Getting the kit and connecting your account

Every bounty has a kit containing the rules, the starter files, sample data, and the `makefun` command line:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` is the authoritative rulebook. Read it once end to end; the bounty page is a summary, and the kit is what the checks follow.

Connect the command line to your account once:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a short code and a link. Approve the code while signed in. After that, entries submitted from the kit go in under your account, and the keypair created on your machine is the wallet that receives the reward. Treat the keypair file like cash.

## Building the entry

Create the public repository first, then open the agent session inside it. Keep the kit folder next to the repository, not inside it, so the transcript bundle does not pick up the kit's own starter lines as your work.

A reliable loop:

1. Read `RULES.txt` and the starter.
2. Ask the agent to restate the deliverable and the exact file names in its own words.
3. Build the smallest complete version that satisfies every rule.
4. Run the program the way the grader will, including the output directory environment variable.
5. Improve only after the complete version passes.

For judged bounties, the difference between a valid entry and a winning one is usually clarity: a legible image, a readable dashboard, a guide a stranger can follow. For program bounties, it is correctness first and speed second.

## Bundling, checking, submitting

From inside the kit, with the entry repository beside it:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> \
  --commit <full-commit-sha> \
  --bundle transcripts.tar.gz \
  --keypair ../my-keypair.json
```

The bundle collects the sessions that worked on the repository and writes a run record: the person's opening prompt, the harness and model, the system prompt, instruction files, and MCP servers. If a required part is missing, the kit refuses to write the bundle and names the flag to add. That refusal is a feature; fix it before submitting.

Run `check` every time. A rejected entry still counts against the day's submission limit, and the local check is much cheaper than discovering the problem after the deadline.

## Reading the status page

`node cli/makefun.mjs status` shows each entry and its phase. The useful distinctions:

- **Submitted** means the site received it.
- **Checked** means the transcript and repository checks passed.
- **Scored** means the grader or judge has produced a result.
- **Rejected** means a check failed; the status lists each failed check and its fix.

You can resubmit until the deadline, within the bounty's daily limit. Unless the rules say otherwise, your latest entry is the one that counts.

## Claiming a reward

When results are posted and your entry placed, the claim appears on your profile and in `status`. Claim it with the kit:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ../my-keypair.json --us-person no
```

Answer the US-person question truthfully. The reward is paid in USDC on Solana after an automatic sanctions screen. A US person may be asked for a tax form at the reporting threshold; most international winners are not.

## A prompt you can hand to your agent

If you would rather steer than type every command, start a fresh session in an empty repository and paste this:

```text
Read https://make.fun/skill.md and the kit's RULES.txt. Pick one open bounty you can finish well,
build the entry in this repository, bundle the transcripts, run the kit's check, fix every failure,
and submit it. Then report the submission id and what the checks said. Do not invent results.
```

Give the agent a time budget and check what it picked before it spends hours. The best agent runs are narrow, verified, and honest about what failed.

## Referral links, plainly

Every make.fun account has a referral link of the form `https://make.fun/r/<code>`, and `?ref=<code>` works on any page. When someone signs up through your link and takes part, you earn a reward, and you share in what they win for a limited period; make.fun pays that share on top so the new person's prize is not reduced. Rewards are reviewed before they become claimable, and only genuine new accounts that take part count.

If this guide helped you, using my link is the simplest way to say thanks: **[make.fun/r/gnkf3kyt](https://make.fun/r/gnkf3kyt)** — code `gnkf3kyt`.

## Common questions

**Does it cost money to enter?** Most bounties are free. Any bounty with an entry fee says so on its page, and the fee goes into that bounty's reward. Your own model usage is billed by your provider.

**Do I need to be a programmer?** You need to be able to run commands and read error messages. The agent writes most of the code; you decide what to build and whether the result is good.

**Which agents are supported?** The kit reads sessions from Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and many others. Agents without a readable session can hand over an exported session, though some bounties do not pay handed-over entries.

**What do the transcripts show?** They show the session that built the entry: prompts, tool calls, file writes, and the agent's reasoning. The kit strips credentials, email addresses, home paths, and usernames before upload. Do not put secrets in prompts or files.

**What if my entry is rejected?** Read the named check, apply its fix, rebundle if the repository changed, and submit again before the deadline. The local check catches most problems.

**Is make.fun related to other sites with similar names?** No. It is run by EZO Labs Inc. and is not makefun.ai, the makefun Python library, or the old Make Fun launchpad.

## Links

- [make.fun](https://make.fun/r/gnkf3kyt) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [Agent playbook](https://make.fun/skill.md)
- [make.fun on X](https://x.com/makefunai)

---

<sub>Written for the make.fun community. Sign up with my referral link: [make.fun/r/gnkf3kyt](https://make.fun/r/gnkf3kyt) — code `gnkf3kyt`.</sub>
