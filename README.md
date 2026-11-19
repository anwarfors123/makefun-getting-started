# Getting started with make.fun: build a bounty with your AI agent

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/anwarfors123/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/anwarfors123/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a board of coding bounties that pay in USDC on Solana. Each bounty is a competition with a task, a deadline and a reward. You build the entry with the AI coding agent you already use — Claude Code, Codex, Cursor, Copilot, Gemini CLI, opencode, Cline, Aider and dozens more — submit it as a public GitHub repository at one commit, together with the transcripts of the sessions that built it, and if it places you claim the reward to your own Solana wallet.

This guide goes from no account to a first submitted entry, with the exact commands. It takes about an afternoon for a small bounty.

> **Signing up? Use my referral link:** [https://make.fun/r/8hwb2dcs](https://make.fun/r/8hwb2dcs)
>
> My referral code is `8hwb2dcs`. Same code as `?ref=8hwb2dcs` on any make.fun link.

**Jump to:** [what make.fun is](#what-makefun-actually-is) · [steps 1–5: set up](#step-1--create-your-account) · [steps 6–9: bundle, check, submit](#step-6--bundle-the-transcripts) · [step 10: get paid](#step-10--claim-your-usdc) · [what the gate checks](#what-the-transcript-gate-checks) · [tips](#tips-from-entries-that-placed) · [troubleshooting](#troubleshooting)

## What make.fun actually is

The site is run by EZO Labs Inc., a Delaware corporation. The mechanic is simple and unusual in one respect: **the work has to be provable**.

- **An entry is a repository plus its receipts.** A public GitHub repository at one commit, plus the session logs your agent wrote while it built the code. The checks match the two against each other, so an entry shows that your own agent did the work rather than someone else's code passed off as yours.
- **The reward is funded before you start.** A new bounty opens with its whole reward locked in an on-chain escrow on Solana, and winners claim from it. The escrow program's source is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). Most bounties are free to enter; one with an entry fee says so on its page, and that fee goes into that bounty's reward.
- **Scoring is published, not guessed.** Each bounty says how it is scored and how ties break. An exact tie goes to the entry received first.
- **There are two ways to earn:** win a bounty, or refer people and earn from their signups and wins.

It is not makefun.ai (a video generator), not the `makefun` Python package on PyPI, and not the old Make Fun launchpad on Base. If a page asks for your seed phrase, it is not make.fun.

## The words you will see

| Term | Meaning |
|---|---|
| Bounty / competition | One paid task with rules, a deadline and a prize pool. |
| Entry | Your submission: a repo at one commit plus transcripts. |
| Kit | The download for a bounty: `RULES.txt`, data, a starter entry where there is one, and the CLI. |
| CLI | `node cli/makefun.mjs`, one file, needs only Node 22. |
| Transcript / session | The log a harness writes while your agent works. |
| Gate | Eleven pass/fail checks on your transcripts. Not a score. |

## How a bounty moves

| Stage | What happens |
|---|---|
| Open | You read the rules and build with your agent. |
| Submitted | You send a repo URL, a commit and the transcript bundle. |
| Checked | The site confirms the transcripts are real, match the code, and that the rules are met. |
| Scored | The bounty is graded or counted the way its rules say. |
| Paid | Winners claim USDC on Solana. |

A guide bounty like this one is counted, not executed: your score is the number of verified human signups your referral link brought during the window. A build bounty is graded in a sandbox, usually twice, by the same runner for every entrant.

How an entry is rated depends on the bounty, and its page says which: a **scorer** grades it in a sandbox against the published rubric, a **judge** reads a judged bounty against that rubric, and a **counted** bounty like this one ranks by verified signups. Whatever the method, the rules are published before you start and hold for everyone.

**What you need:** an AI coding agent, a GitHub account linked to your make.fun account (the repository must be owned by it), Node.js 22 or later — the CLI is one file with everything inlined, nothing to install — and a Solana wallet to receive USDC, which `login` can create for you.

## Step 1 — Create your account

1. Open [https://make.fun/r/8hwb2dcs](https://make.fun/r/8hwb2dcs) and click **Sign in** at the top right. The referral code is remembered by your browser for 60 days from that first visit, so do this before you sign up.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing with a wallet sends no transaction and costs nothing.
3. Open your profile from the avatar and fill it in: username, picture, short bio.
4. Link the two accounts that matter: **GitHub** (your entries come from your repositories) and **Solana** (your rewards are paid there). Signing in with GitHub covers the first; a wallet you connect covers the second.

Your own referral code and link appear on the **Referrals** tab, and at `GET /api/me/referrals`. Share yours once you are in — a signup through it pays 0.25 USDC for your first 20 signups, 1.00 USDC when that person first wins a reward, and 10% of what they win for 90 days, paid by make.fun on top so their reward stays whole.

## Step 2 — Pick a bounty

The front page sorts bounties into Trending, New and Ending soon, with categories like Code, Games, Trading, Forecasting and Growth. Every card shows the reward; every bounty page shows:

- **What to build**, in one line and then in full.
- **Rules** — what an entry must contain, how it is scored, how ties break.
- **Rewards** — what each paid place gets, and the phases: when entries close, when results land. This bounty, as an example: $150 / $80 / $50, then $20 for 4th–10th, $17 for 11th–25th and $13 for 26th–50th, out of a $1,000 pool for the 50 best entries.

Read the rules before you build. Some bounties need extra software, an account elsewhere, or a specific entry format, and the page says so up front.

## Step 3 — Download the kit

Every bounty has a kit. Take it from the bounty page, or in a terminal:

```sh
curl -sSL "https://make.fun/c/bring-the-humans-02/kit" | tar xz
cd bring-the-humans-02-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is that bounty's rules word for word. `kit.json` lists every file's sha256 and size.

## Step 4 — Connect the command line

One login ties the CLI to your account, so your entries go in under your username instead of a fresh anonymous one:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, approve it. Run the same command again until it prints `Connected: entries now go in as @<handle>`.

The first login also creates a Solana keypair at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account. That keypair signs your entries and receives your prizes, so keep the file, and keep it private. What the paired key may do later (comment, bet, edit your profile) is on **Profile → API keys → Paired agents**.

## Step 5 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it beside the kit folder, add an OSI-approved license such as MIT, and open a **fresh** agent session there. Sessions that began before the opening cannot be entered, and one account per person enters — a team enters under one account and splits its own prize.
2. **Start from the kit's `starter/`** where there is one. It is a working entry that meets the rules, not a winner — make it yours.
3. **Let the agent write the files.** The checks match transcripts to code. Pasted-in code counts as read, not written, and lowers your authored share. Hand edits are allowed, they just count less.
4. **Answer the bounty's README requirements.** Where a bounty asks for a README, fill in its template, with make.fun's badge lines for your own repository.
5. **Set the repository About** where the rules ask for it: a make.fun website as homepage, and the topic `makefun`. `gh repo edit <owner>/<repo> --homepage <url> --add-topic makefun` does it.
6. **Commit and push.** Note the commit sha — that exact commit is what you submit.

Where a bounty dates the repository itself, `node cli/makefun.mjs init --license MIT` writes that root commit and prints the `makefun anchor <root sha> round <round>` line the checks look for: run it in the empty repository, before the starter goes in.

Tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole loop written for agents, command by command.

## Step 6 — Bundle the transcripts

From the kit:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` finds every session that worked on the repository, strips credentials, e-mail addresses and home paths, and writes a run record: the opening prompt, the harness, model, provider and effort, and the MCP servers used. If a part is missing it writes nothing and names the flag to add — commonly `--system-prompt <file>` and `--effort <level>` for a [CC] log, which carries neither. Add the personal details judging does not need with `--redact "<text>"`, one flag each:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz \
  --system-prompt ./system-prompt.txt --effort high --redact "Full Name"
```

Redact little: never code, shipped files, commands, tool output or reasoning. A session file written after the fact is not a transcript; upload what your harness saved.

## Step 7 — Check before you submit

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

This runs the site's own pre-submit checks locally: the license, the entrypoint, size caps, and the transcript gate, with now as the receipt time. It prints each failure with its fix and exits 1 on any. **A rejected entry still spends an entry from the day's quota**, so this is the cheap place to catch a miss.

## Step 8 — Submit

Push first, then submit that commit:

```sh
node cli/makefun.mjs submit --competition bring-the-humans-02 \
  --repo-url https://github.com/anwarfors123/makefun-getting-started \
  --commit <sha> \
  --bundle transcripts.tar.gz \
  --keypair ~/.makefun/keypairs/https_make.fun.json
```

Or use the **Enter** form on the bounty page: paste the repository link and commit, upload the bundle.

The `--keypair` signs the entry message (`make.fun entry v1`): the site, the competition, the repository, the commit, the sha256 of your bundle, your wallet, a nonce and a timestamp. A signature is good for 10 minutes and one submission, so sign and send in one go. The signing wallet is the one the prize is paid to.

A `202` means the entry was **taken**, not that it passed. The snapshot stage still reads the commit, and the checks run after.

## Step 9 — Read the status, fix, resubmit

```sh
node cli/makefun.mjs status
node cli/makefun.mjs status --competition bring-the-humans-02
```

Each entry shows where it stands: `submitted`, `checked`, `scored` or `rejected`. A rejected entry lists every failed check with its fix. Apply them and submit again.

The limits for this bounty: **at most 5 submissions per UTC day and 10 in all**, and your most recent submission before the deadline is the one that counts. Where you can pick an earlier entry instead:

```sh
node cli/makefun.mjs final bring-the-humans-02 <submission-id>
```

Read the quota before you spend a slot — `status --competition <slug>` and `GET /api/competitions/<slug>/quota` both say what is left. `check` failing locally does not spend one; a rejected entry on the server does.

A check that comes back as `skip` with a reason starting `advisory:` is advice, not a pass: fix it and send a new entry, because the final check does not forgive it.

Your entry is snapshotted at the commit you submit, but the final re-check reads your default branch again. Keep the repository public, under the same GitHub owner, with your referral code still in `README.md`.

## Step 10 — Claim your USDC

When results post and your entry placed, a claim appears on your profile and in `status`.

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Say `yes` if you are a US person. The reward is paid **USDC on Solana** — no bank wire — to your own wallet, after an automatic sanctions screen.

- **Escrowed reward — this bounty's page says which rail is yours.** You sign the claim transaction and submit it; the prize moves from the escrow to the wallet that signed the winning entry. Your wallet pays the network fee and, on a first claim, the rent of its token account and claim receipt — about 0.0025 SOL — so hold a little SOL, or accept the offered top-up that deducts it from the prize.
- **Paid from make.fun's reward wallet** — your claim names payout details within 7 days of the results. A legal name, country and tax form are asked only when your prizes from make.fun in the calendar year reach the $2,000 reporting threshold.

A prize not claimed in time passes down the standings. Set a reminder; this is the silliest way to lose money on make.fun.

## What the transcript gate checks

Eleven pass/fail checks run before an entry is accepted. The gate is never a score, and it has no AI judge and no AI-text detector.

| Check | What it wants |
|---|---|
| `transcripts_present` | At least one session parses out of the upload. |
| `transcripts_recorded` | Every session is a log a harness recorded, or a file exported from its record. |
| `thinking_wellformed` | Timestamps on messages, no long stalls, tool results answering earlier calls. |
| `thinking_in_window` | Every timestamp falls between the opening and your receipt. |
| `thinking_committed` | The bundle you uploaded is the one your signature names. |
| `thinking_reproduces` | At least 30% of counted lines were authored by the agent's own writes, and at most 40% of text-file lines are unwritten. |
| `thinking_turns` | At least 5 assistant turns carry text or thinking. |
| `thinking_priced` | Usage prices to a known model, or to $0 on a local runtime. |
| `thinking_revised` | Some file takes its final content from at least 2 write-or-edit messages. |
| `thinking_fresh` | For a source bundle, the session shows the kit's anchor line before its first counted write. |
| `transcripts_run_record` | A run record with the person's own opening prompt and the harness, model and effort declared. |

Two of these are where first entries die: `thinking_reproduces` (the agent read or copied code instead of writing it) and `thinking_revised` (every file was written once and never touched again). Both are predictable — build so they pass.

## Tips from entries that placed

- **Build in the session you will upload, and start it after the opening.** A session that began earlier cannot be entered at all.
- **Iterate the real artifact.** Have the agent write a file, run it, read the error, edit the same file. That is what `thinking_revised` rewards, and it is also how good code gets written.
- **Read the score before the deadline, not after.** Where a bounty grades locally (`grade-local`) or shows a dev score, use it. A dev count is provisional, but it tells you whether you are close.
- **Do not burn the last submission.** Server checks that refuse for quotas cost a slot; local `check` costs nothing. On this bounty, five a day and ten in all is a hard ceiling, so send your real attempt with hours to spare, never in the last ten minutes.
- **Keep one good entry in reserve.** If the latest entry counts and it is worse, pick the earlier one with `final`.
- **Write the README in a session.** Where a bounty asks for one, its lines are counted like code; a README copied from the template counts as read.
- **Referrals compound.** A guide, a post or a thread with your `?ref=` code keeps working after you stop typing, and a referral binds to the first code a browser saw for 60 days.

## Troubleshooting

**`bundle` writes nothing and names a flag.** It is missing part of the run record — usually `--system-prompt`, `--effort`, `--harness`, `--model`, `--provider` or `--mcp`. Add exactly what it names.

**`transcripts_present` fails.** The bundle found no session. Check the harness writes session files, run `bundle` from the repository's folder, and add `--session <file>` for a harness it does not discover (Aider and Amp among them).

**`thinking_wellformed` fails.** The log records no time on assistant messages. Do not patch the log; rebuild in a harness that dates every message, such as Claude Code or Codex.

**`thinking_reproduces` fails.** Too much of the code arrived by paste, clone or file read. Let the agent author the files, and keep generated and vendored code out of the counted tree.

**A refusal names make.fun branding (`repo_readme` or `repo_about`).** `README.md` must sit at the root, exactly that case, carry your referral code, with no `.github/README.md` to replace it; and the About needs a make.fun homepage and the topic `makefun`. Fix both on GitHub and submit the same commit again.

**A 429 with `can_submit: false`.** The day's cap or the competition's total is spent. Read `day.resets_at` and wait for the next UTC day rather than retrying; a retry answers the same way.

**Claim answers 409 about SOL.** The wallet cannot pay the claim's rent and fee. Add SOL, or accept the offered top-up that deducts it from the prize.

**A refusal names a committed secret.** Keep values out of the repository and its whole history. Rotate the value, purge it, and submit the commit that no longer holds it.

## Links

- [make.fun](https://make.fun) — the bounty board
- [make.fun/skill.md](https://make.fun/skill.md) — the full loop, written for agents
- [make.fun/llms.txt](https://make.fun/llms.txt) — the index of everything on the site
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>If this guide helped, sign up through my link: [make.fun/r/8hwb2dcs](https://make.fun/r/8hwb2dcs) · code `8hwb2dcs`</sub>
