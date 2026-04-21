---
name: gpt-pro
description: "Package files + structured prompts for manual copy-paste into GPT-5.4 Pro for deep reasoning review. Use when user says 'send to GPT Pro', 'GPT Pro review', 'ChatGPT Pro review', 'prepare for GPT', 'package for GPT Pro', 'get GPT to review', 'GPT Pro analysis', 'upload to ChatGPT', 'gpt-pro', '/gpt-pro', 'pack for GPT', 'bundle for ChatGPT', 'have ChatGPT check this', 'send this to GPT', 'GPT deep review'. Creates structured prompt + file bundle as tar.gz in ~/Documents/GPT Pro Analysis/ and copies prompt to clipboard for manual paste into ChatGPT Pro."
---

# GPT Pro Analysis — Deep Reasoning via GPT-5.4 Pro (Manual)

Prepare any problem + relevant files into a structured package for manual upload to ChatGPT Pro. Domain-agnostic — works for code debugging, architecture review, data analysis, writing feedback, reverse engineering, research synthesis, or anything that benefits from deep reasoning over multiple files.

## Workflow

```
UNDERSTAND → SCOPE → COLLECT → PROMPT → SANITIZE → PACKAGE → REPORT
```

Everything runs locally. The final step is a tar.gz on disk + prompt on clipboard, ready for you to paste into chatgpt.com.

## Why a Structured Package Beats a Chat Dump

ChatGPT Pro excels at deep, long-context reasoning — but quality in = quality out. A structured PROMPT.md that frames the problem, sets the role, defines the output format, and indexes the attached files consistently produces 10x better results than pasting raw code into the chat. This skill encodes the prompt patterns that work.

## Step 1: Understand the Task

Ask or infer from conversation context:
- **What's the question?** — "Why is this broken?", "Review this architecture", "Analyze this data"
- **What expertise should GPT Pro adopt?** — Match to the domain (senior engineer, editor, data scientist, etc.)
- **What output format?** — Code fixes with file:line? A report? A redesigned component? A comparison matrix?

## Step 2: Collect the Files (Be Generous)

**Philosophy: more is more.** GPT-5.4 Pro has a huge context window and is excellent at triaging — it will decide which files to read deeply and which to skim or ignore. Don't pre-filter aggressively; you'll strip out the one file that turns out to hold the clue. When in doubt, include it. Missing context is a bigger failure mode than "too much context".

**Always include:**
- The file(s) directly implicated in the problem
- Adjacent / dependent code (callers, callees, shared utilities, types)
- Config / env that affects behavior (sanitized)
- Sample data showing the issue (logs, errors, outputs, examples)
- A **file tree** of the repo (`tree -L 4 -I 'node_modules|target|.git|__pycache__' > tree.txt`) so GPT Pro sees the full universe even for files not attached
- A **recent git log** (`git log --oneline -n 50 > git-log.txt`) — recent commits often explain what broke
- Any `README.md`, `CLAUDE.md`, or architecture docs in the repo

**Include generously when even tangentially related:**
- Tests (they document expected behavior)
- Related modules in the same feature area
- Prior version of the file (via `git show HEAD~5:path/to/file > prior-version.txt`)
- Reference / comparison material — competitor code, working example, spec documents
- Prior analysis / decision logs / ADRs
- Package manifests (`package.json`, `Cargo.toml`, `pyproject.toml`) so versions are unambiguous

**Only exclude:**
- Secrets (see Step 5 — mandatory sanitization)
- Build artifacts: `node_modules/`, `target/`, `dist/`, `build/`, `__pycache__/`, `.next/`
- Binary blobs (images, fonts, compiled libs) unless directly relevant
- Machine-generated lockfiles >10MB (ok to include smaller ones)
- Vendored third-party code unless it's the subject of the review

## Step 3: Build the Package Directory

```
<descriptive-name>/
├── PROMPT.md           # THE key file — structured task + questions
├── tree.txt            # Full repo tree (tree -L 4 -I '...' > tree.txt)
├── git-log.txt         # Recent commits (git log --oneline -n 50)
├── source/             # Relevant source files (preserve original names)
│   ├── module_a.py
│   ├── module_b.rs
│   └── Component.tsx
├── adjacent/           # Callers, callees, shared utils, types
│   └── ...
├── tests/              # Related tests — document expected behavior
│   └── ...
├── data/               # Supporting evidence — logs, outputs, examples
│   ├── error_log.txt
│   ├── sample_output.json
│   └── expected_vs_actual.md
├── docs/               # README, architecture notes, ADRs, CLAUDE.md
│   └── ...
├── reference/          # Comparison material, prior analysis, competitor code
│   └── prior_findings.md
└── config/             # Sanitized config, package manifests
    ├── env_sanitized.txt
    ├── package.json
    └── Cargo.toml
```

Adapt the subdirectory names to the domain. For a book review it might be `chapters/`, `research/`, and `feedback/`. For data analysis it might be `datasets/`, `schemas/`, and `notebooks/`. Use whatever makes sense — and lean toward more folders / more files rather than fewer.

## Step 4: Write PROMPT.md

This is the most important file. GPT-5.4 Pro starts with **ZERO project knowledge** — it cannot infer your stack, build tooling, conventions, or file layout without you telling it. Even though you're attaching many files, the PROMPT.md is what tells GPT Pro *why those files are there*, *what to look at first*, and *what you actually want back*. Without a strong PROMPT.md, a big bundle becomes noise.

### The "Exhaustive Prompt" Pattern

Short, vague prompts on large bundles yield generic answers. Lean into depth — a strong prompt is typically 500-2000 words and follows this structure:

1. **Top:** Project briefing — stack, build/test commands, platform constraints, key directories
2. **Middle:** Exact question + what you already tried + verbatim errors
3. **Bottom:** Constraints + desired output format + file index

The more files you attach, the more the PROMPT.md needs to act as a *map*: point GPT Pro at the 2-3 files where the investigation should start, while making clear the rest are available as context to pull on.

### PROMPT.md Template

```markdown
# <Title — What You Want Analyzed>

## Your Role

You are a senior <domain expert> conducting <type of analysis>. You have deep expertise in <relevant technologies/fields>.

**Rules:**
1. Base every conclusion on evidence from the files provided. Flag speculation as `[HYPOTHESIS]`.
2. Don't tell me what's working. Tell me what's wrong/missing/improvable and how to fix it.
3. For each finding: root cause, evidence (file:line or specific quote), fix, expected impact.

## Project Briefing

<Stack, frameworks, language versions, platform targets. GPT Pro can't guess this.>
- **Stack:** <e.g., Next.js 15, TypeScript 5.8, Supabase, Vercel>
- **Build:** <e.g., `pnpm build`, `cargo test`, `npm run dev`>
- **Platform:** <e.g., macOS/iOS, Linux server, browser SPA>
- **Key directories:** <e.g., `src/` = app code, `lib/` = shared utils, `supabase/` = migrations>

## Context

<2-4 paragraphs explaining the system/project — what it does, how it works, current state. Include enough for GPT Pro to understand the architecture without needing to infer it from code alone.>

## Current Situation

<Quantified state — metrics, error rates, performance numbers, word counts, whatever is measurable. Concrete data, not vibes.>

## What We Already Tried

<Critical section. List what was attempted and why it didn't work. Prevents GPT Pro from suggesting things you already ruled out.>

1. <Approach 1> — <why it failed or was insufficient>
2. <Approach 2> — <result>

## Questions

### Q1: <Specific, answerable question>
**Data:** <Point to specific files in the package>
**Hypotheses:** <2-3 possible explanations to investigate>

### Q2: <Next question>
**Data:** <File pointers>

### Q3-N: <As many as needed, usually 3-7>

## Where to Start

<The bundle is large. Tell GPT Pro where to begin so it doesn't burn thinking budget on irrelevant files.>

**Start with:** `source/<primary-file>` and `data/<key-evidence>`. These are where the problem manifests.
**Then check:** `adjacent/<caller>` and `tests/<failing-test>` to verify the control flow.
**Everything else** (docs, reference, tree, git-log) is context — pull on it when the primary files raise questions.

## Files Included

<The bundle may contain dozens of files — this table is the navigation index. List every file with a one-line description of what it is and why it's attached. Group by folder.>

**Core (primary investigation)**
| File | Role |
|---|---|
| `source/auth.ts` | OAuth2 implementation — the code under review |
| `source/middleware.ts` | Request pipeline — calls auth.ts |
| `data/error_log.txt` | Production errors from last 24h |

**Adjacent (likely relevant)**
| File | Role |
|---|---|
| `adjacent/session.ts` | Session store — auth.ts writes to it |
| `adjacent/types.ts` | Shared type defs |
| `tests/auth.test.ts` | Existing tests — the failing one is `describe('refresh')` |

**Context (pull on if needed)**
| File | Role |
|---|---|
| `tree.txt` | Full repo layout |
| `git-log.txt` | Last 50 commits — the regression likely entered in the last 10 |
| `docs/ARCHITECTURE.md` | How the auth subsystem fits the whole app |
| `config/package.json` | Dep versions (jsonwebtoken 9.0.2, next-auth 5.0.0-beta) |
| `reference/prior-auth-review.md` | Last GPT Pro review from 2 months ago |

## Desired Output

<Be specific about what form you want the answer in. This dramatically improves quality.>

Options:
- "Return a patch plan with exact file:line changes + test cases"
- "List the 3 riskiest assumptions and how to validate each"
- "Give 3 architectural options with tradeoffs table"
- "Root cause analysis with evidence chain"
- "Code review with severity-ranked findings"

## Constraints

<What can't change. What's locked in. Scope boundaries.>

- Don't suggest replacing <framework/library> — it's locked in
- Must maintain backward compatibility with <API/interface>
- Performance budget: <specific numbers>
- <Other hard constraints>
```

### Adapting the Template

The template above is a skeleton. Adapt the sections to fit the domain:

- **Code debugging**: Add "System Architecture" section, include error messages, stack traces
- **Architecture review**: Add "Current Architecture" diagram/description, "Proposed Changes", "Trade-offs to Evaluate"
- **Writing/book review**: Replace "Questions" with "Review Criteria" (structure, pacing, clarity, argument strength)
- **Data analysis**: Add "Dataset Description" (schema, size, collection method), "Analysis Goals"
- **Design review**: Add "Design Requirements", "User Personas", "Existing Patterns"
- **Reverse engineering**: Add "What We Know", "What We Don't Know", "Reference Implementations"

The pattern is always: **Role + Context + Specific Questions + Evidence Pointers + Output Format**.

## Step 5: Sanitize

Before packaging, strip ALL secrets. This is non-negotiable.

```bash
# Create sanitized env copy
grep -v -E '(PRIVATE_KEY|MNEMONIC|API_KEY|PASSWORD|SECRET|AUTH_TOKEN|CREDENTIAL|ACCESS_KEY)' .env > config/env_sanitized.txt
```

**Strip from all files:**
- Private keys, mnemonics, seed phrases
- API keys, auth tokens, session cookies
- Passwords, database connection strings
- Internal IP addresses, hostnames (replace with `<REDACTED>`)
- Cloud credentials (AWS, GCP, Azure)

**Quick check before packaging:**
```bash
grep -rni "password\|secret\|api_key\|private_key\|mnemonic\|token.*=" <pack-dir>/ | head -20
```

## Step 6: Package for Manual Upload

All packages go under `~/Documents/GPT Pro Analysis/`, with each run in its own subfolder:

```bash
BASEDIR=~/Documents/GPT\ Pro\ Analysis
OUTDIR="$BASEDIR/<project>-<type>-$(date +%Y-%m-%d)"
mkdir -p "$OUTDIR"
cp /tmp/<pack-dir>/PROMPT.md "$OUTDIR/PROMPT.md"
tar czf "$OUTDIR/<project>-<type>-$(date +%Y-%m-%d).tar.gz" -C /tmp <pack-dir>/
```

Then copy the prompt to clipboard for immediate paste:

```bash
cat "$OUTDIR/PROMPT.md" | pbcopy
```

## Step 7: Report to User

1. **Package location** — the path under `~/Documents/GPT Pro Analysis/`
2. **What's inside** — file count, key files listed
3. **Clipboard** — confirm the prompt has been copied
4. **How to use it:**
   - Open ChatGPT Pro in browser (chatgpt.com)
   - Start a new chat, select GPT-5.4 Pro model
   - Paste (Cmd+V) — the prompt is already in your clipboard
   - Attach the `.tar.gz` (or individual files from the pack directory if preferred)
   - Send
   - Wait — deep thinking runs can take 10 min to 1 hour. Don't click "Answer now"; it skips deep thinking.

## PASTE_THIS.txt Alternative

For smaller packages where file upload is overkill, create a single `PASTE_THIS.txt` that concatenates everything:

```
# <TITLE>
# Paste this into ChatGPT Pro
# Date: <DATE>

## Task
<prompt content>

================================================================
FILE: <filename>
================================================================
<file contents>

================================================================
FILE: <next filename>
================================================================
<file contents>
```

This works well when the total content is under ~50K characters. Copy the whole thing to clipboard:

```bash
cat "$OUTDIR/PASTE_THIS.txt" | pbcopy
```

Then paste directly into ChatGPT — no file upload needed.

## Multi-Round Workflow

When GPT Pro returns results and you need a follow-up:

1. Save the GPT Pro response locally with clear attribution:
   - `<dir>/gpt-pro-analysis-<date>.md` with header: "Source: GPT-5.4 Pro, <date>"
2. For Round 2+, reference prior findings in the new PROMPT.md:
   ```markdown
   ## Prior Analysis (Round 1)
   <Summary of what GPT Pro found last time>

   ## What Needs Follow-Up
   <Specific gaps, validations needed, deeper dives>
   ```
3. Include Round 1 outputs in the new package under `reference/`
4. In ChatGPT, you can either continue the same thread (paste the new prompt as a follow-up message) or start fresh with the Round 2 package — starting fresh is usually cleaner when the follow-up is substantial.
