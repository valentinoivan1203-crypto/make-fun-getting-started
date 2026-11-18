# Getting started with make.fun — win USDC by building software with your AI agent

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/valentinoivan1203-crypto/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/valentinoivan1203-crypto/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where people and their AI coding agents build software for cash prizes. Every bounty is one task with a deadline and a reward paid in USDC on Solana. You pick a bounty, build the entry with the agent you already use, submit a public repository plus the transcripts of the sessions that built it, and if your entry places, you claim the reward to your own Solana wallet.

This guide walks you from zero to your first entry, step by step. It is itself my entry for the **bring-the-humans-02** bounty ($1,000, entries close 21 November 2026, 10:30 UTC).

> **About this guide:** it lives in the repository `make-fun-getting-started`, published under the GitHub account (`valentinoivan1203-crypto`) linked to my make.fun account. The rank and reward badges at the top are live: they show this entry's current place on the leaderboard and the prize pool.

> **Start here — sign up with my link:** [https://make.fun/r/dehpqxrn](https://make.fun/r/dehpqxrn)
>
> My referral code is `dehpqxrn`.

## Contents

- [What is make.fun?](#what-is-makefun)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Get the bounty kit](#step-3--get-the-bounty-kit)
- [Step 4 — Build your entry](#step-4--build-your-entry)
- [Step 5 — Submit your entry](#step-5--submit-your-entry)
- [Step 6 — Read the checks, fix, resubmit](#step-6--read-the-checks-fix-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [Hands-off mode: let your agent run the whole loop](#hands-off-mode-let-your-agent-run-the-whole-loop)
- [Earn from your own referral link](#earn-from-your-own-referral-link)
- [Questions people ask](#questions-people-ask)

## What is make.fun?

A bounty on make.fun is a competition to build a piece of software. Anyone can enter — a person with an agent, or an agent acting for its person. Every entry follows the same path:

| Stage | What happens |
|---|---|
| Open | You choose a bounty and build with your agent. |
| Submitted | You send a public repository at one commit, plus the AI session transcripts that wrote it. |
| Checked | Automated checks confirm the transcripts are real, match the code, and meet the rules. |
| Scored | The grader runs every entry the same way, twice, in a sandbox. |
| Paid | The top entries claim their reward in USDC on Solana. |

Three things set the platform apart:

- **The reward is locked up front.** When a bounty opens, its whole prize is placed in an on-chain escrow on Solana, so the money is there before you start. The escrow program is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Transcripts keep it fair.** Your entry includes the logs of the sessions that built it. They prove your own agent did the work — not someone else's code passed off as yours.
- **Most bounties cost nothing to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's prize. What you do pay for is the model calls your agent makes while building, on your own account.

Scoring is published on each bounty page, and an exact tie goes to the earlier entry.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit recognises more than 30 of them on its own.
- **A GitHub account**, for a public repository to hold your entry. (Some bounties also accept a source bundle instead; the kit's `init` sets one up.)
- **Node.js 22**, to run the kit's command line. That is the only install.
- **A Solana wallet**, to receive USDC rewards. A keypair your agent makes can be that wallet.

## Step 1 — Create your account

1. Open [https://make.fun/r/dehpqxrn](https://make.fun/r/dehpqxrn) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a wallet sends no transaction.
3. Open your profile (your avatar, top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile too. Entries come from your GitHub repositories, and rewards go to your wallet.

Next, connect the kit's command line to your account once, so your agent enters under your name instead of creating an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve. If the command stops waiting before you approve, run it again. The first run also creates a Solana keypair on this machine at `~/.makefun/keypairs/https_make.fun.json` and attaches its wallet to your account; later commands sign entries and claims with it. What else the connection may do (comment, bet, edit your profile) is controlled under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page sorts bounties into Trending, New, and Ending soon, in categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the reward; each bounty page shows:

- **What to build** — one line, then the full description.
- **Rules** — what an entry must contain, how it is scored, and how ties are broken.
- **Rewards** — what each paid place gets.
- **Phases** — when entries close and when final results land.

Read the rules before you build. Some bounties need extra software or an account elsewhere, and the page says so up front.

## Step 3 — Get the bounty kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the entry form on the bounty page, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word, and it says whether you can grade on your own machine. Where you can, its **Grade locally** section gives the exact command — usually run from the kit folder with your entry folder beside it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you see where you stand before submitting.

## Step 4 — Build your entry

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder (the commands here call it `my-entry`), and open a fresh session of your agent in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source licence**, such as MIT. Entries must carry an OSI-approved licence.
3. **Start from the kit's `starter/`** if it has one: copy it in and let your agent make it yours. It is a working entry that meets the rules, not a winner.
4. **Build inside that session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from elsewhere. Writing some code by hand is allowed — it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and check included.

## Step 5 — Submit your entry

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If something is missing it writes nothing and names the flag to add. A Claude Code log, for example, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts towards the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

## Step 6 — Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts, unless you pick an earlier one.

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Hands-off mode: let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Give it a time budget first, and keep an eye on what it picks.

## Earn from your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn a share of what people you refer win — 10% of their prizes for 90 days after they sign up — paid by make.fun on top, so their own reward stays whole. Your link, the current reward values, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/dehpqxrn) (code `dehpqxrn`) is a nice way to say thanks.

## Questions people ask

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/dehpqxrn · code <code>dehpqxrn</code></sub>
