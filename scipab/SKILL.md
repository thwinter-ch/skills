---
name: scipab
description: Structure a data dump, situation report, or problem description into the SCIPAB framework (Situation, Complication, Implication, Position, Action, Benefit, Risk). Triggers on "scipab", "/scipab", "frame this as scipab", "structure this in scipab", or when the user asks for a decision brief, executive summary, or issue report.
---

# /scipab

Takes a messy data dump — logs, git state, bug reports, findings, debates — and rewrites it as a clean **SCIPAB** brief so the reader can jump straight to the decision.

SCIPAB is Mandel Communications' executive messaging framework (see https://www.mandel.com/scipab-messaging-tool). This variant adds **Risk** as a 7th section to make the downside of the proposed action explicit.

## When to use

- Surfacing a problem, bug, or unexpected state to the user
- Asking for a green light on a risky action
- Briefing on a decision that has trade-offs
- Turning investigation findings into a go/no-go report
- Any moment where a reader would otherwise have to parse prose to extract "what do you want me to do"

Skip SCIPAB for:
- Simple status updates ("done, here's the link")
- Direct answers to direct questions
- Routine confirmations

## Input

The user provides one of:
1. A raw data dump (tool output, log excerpt, git status, findings)
2. A free-form problem description
3. A previous analysis they want restructured

The skill reads that input, fills in the 7 sections, and drops any section that genuinely doesn't apply (marked `N/A — <one-line reason>`).

## The seven sections

| Section | Answers | Length |
|---|---|---|
| **Situation** | What's the baseline the reader already knows? | 1-2 sentences |
| **Complication** | What changed / what's broken / what's new? | 1-3 sentences |
| **Implication** | Why does it matter? What's the cost of doing nothing? | 1-3 sentences |
| **Position** | What's your recommended stance in one line? | 1 sentence |
| **Action** | Concrete next step(s) — commands, file paths, names | 1-5 bullets |
| **Benefit** | What the reader gets from approving the action | 1-2 sentences |
| **Risk** | What could go wrong if the action is taken; blast radius, reversibility | 1-3 bullets |

## Output format

Use Markdown with H2 or H3 section headings. Keep it tight — the whole brief should fit on one screen when possible. Lead with a one-line title that names the issue.

```markdown
## 🚨 {One-line issue title}

**Situation.** {baseline fact the reader already knows}

**Complication.** {what changed or broke}

**Implication.** {why it matters, what it costs to ignore}

**Position.** {recommended stance in one line}

**Action.**
- {concrete step 1 with file paths/commands}
- {concrete step 2}

**Benefit.** {what we get by doing this}

**Risk.**
- {downside 1 — reversibility, blast radius}
- {downside 2}

---

Green light?
```

## Rules

1. **No section should be longer than 3 sentences.** If it is, cut it.
2. **Position is one line.** Not a paragraph. If you can't compress it, you don't have a position yet.
3. **Action must be executable.** Include file paths, commands, IDs — not "investigate X" or "figure out Y". If the next step is investigation, the whole brief is premature.
4. **Risk is not a hedge.** It's the honest downside of the *proposed* action, not a list of reasons to do nothing. Focus on: reversibility, blast radius, who else is affected, what breaks if wrong.
5. **If a section truly doesn't apply**, write `N/A — <one-line reason>` rather than padding with filler.
6. **End with a clear ask** — "Green light?" / "Proceed?" / "Confirm A or B?" — so the reader knows what response you need.
7. **Artifacts over prose.** Tables, bullet lists, file paths beat narrative sentences.

## Examples

### Example 1 — Surfacing a rogue script outcome

```markdown
## 🚨 tw-brain/raw/ has 407 untracked files

**Situation.** Git-tracked baseline of `tw-brain/raw/` is 1 file (the MARP video). Everything else lives in `inbox/` waiting for `/ingest`.

**Complication.** A batch canonicalization script ran despite rejection and wrote 407 untracked files to `raw/`, including nonsense year folders (`1778/`, `2027/`). The inbox originals were restored from git, so every file now exists twice.

**Implication.** Future `/lint` will catalog duplicates and real `/ingest` subagents will re-process files already sitting in `raw/`, compounding the mess. The rogue files aren't real `/ingest` output — just frontmatter shuffles with no wiki extraction.

**Position.** The rogue files are worthless and untracked; delete them.

**Action.**
- `git clean -fd tw-brain/raw/` to remove untracked files and folders
- Verify: `git status tw-brain/raw/` shows clean
- Dispatch Sonnet subagent pilot on 10 inbox files for real `/ingest`

**Benefit.** `raw/` matches git-tracked state again. Inbox is single source of truth. Subagents produce real wiki extraction, not frontmatter shuffles.

**Risk.**
- `git clean -fd` is destructive for untracked work — but here the untracked files are the bug, not work to preserve
- Nonzero chance the script also touched tracked files (verified: only `.gitkeep` and MARP are tracked in `raw/`)

Green light to run `git clean -fd tw-brain/raw/`?
```

### Example 2 — Go/no-go on a decision with trade-offs

```markdown
## Should we merge the experimental branch before the release freeze?

**Situation.** Release freeze starts Thursday 2026-03-05. Experimental branch `feature/new-ingest` has 23 commits, passes CI, peer-reviewed.

**Complication.** Two of the 23 commits touch the payment reconciliation path, which is outside the original scope of the feature. No dedicated QA on that path.

**Implication.** Shipping an unreviewed reconciliation change into a freeze-locked release means any bug sits in production for 3 weeks with no hot-fix window. Reverting after freeze requires exec approval.

**Position.** Split the branch — ship the in-scope commits now, hold the reconciliation commits for the next release.

**Action.**
- `git cherry-pick` the 21 in-scope commits onto `release/2026-03`
- Open a new branch `feature/reconciliation-cleanup` with the 2 held commits
- File a ticket to QA the held commits after freeze

**Benefit.** Feature ships on time, freeze integrity preserved, unreviewed code stays out of production.

**Risk.**
- Cherry-pick may surface subtle conflicts the original merge wouldn't have
- Splitting the branch adds 30-60 min to the release prep window

Confirm split, or ship whole branch and accept the freeze risk?
```

## Priority order when drafting

When content is long or noisy, draft sections in this order: Position → Action → Situation → Complication → Implication → Risk → Benefit.

Why: if you can't state the Position in one line, the rest is guesswork. If you can't write a concrete Action, the brief is premature. Get those right first, then frame them with context.
