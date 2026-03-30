<div align="center">

# GPT Pro Skill for Claude Code

**One command. Structured prompt + file bundle. Ready to upload to ChatGPT Pro.**

<br />

[![Star this repo](https://img.shields.io/github/stars/199-biotechnologies/claude-skill-gpt-pro?style=for-the-badge&logo=github&label=%E2%AD%90%20Star%20this%20repo&color=yellow)](https://github.com/199-biotechnologies/claude-skill-gpt-pro/stargazers)
&nbsp;&nbsp;
[![Follow @longevityboris](https://img.shields.io/badge/Follow_%40longevityboris-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/longevityboris)

<br />

[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blueviolet?style=flat-square&logo=anthropic)](https://code.claude.com/docs/en/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=flat-square)](https://github.com/199-biotechnologies/claude-skill-gpt-pro/pulls)

---

Stop copy-pasting code into ChatGPT. This skill teaches Claude Code to **package any problem** into a structured tar.gz with an engineered prompt, then **copies it to your clipboard** -- ready to paste and upload in seconds.

[Install](#install) | [How It Works](#how-it-works) | [Use Cases](#use-cases) | [Template](#prompt-template) | [Contributing](#contributing)

</div>

---

## Why This Exists

ChatGPT Pro excels at deep, long-context reasoning -- but **quality in = quality out**. A structured `PROMPT.md` that frames the problem, assigns an expert role, defines the output format, and points to the right files produces **10x better results** than dumping raw code into the chat.

This skill encodes the prompt engineering patterns that consistently work.

### Before vs After

| Without this skill | With this skill |
|---|---|
| Copy-paste files one by one | One command bundles everything |
| "Here's my code, what's wrong?" | Expert role + specific questions + evidence pointers |
| Unstructured, meandering responses | Finding > Evidence > Fix > Impact format |
| Secrets accidentally included | Auto-sanitization of keys, tokens, credentials |
| Files scattered on Desktop | Organized in `~/Documents/GPT Pro Analysis/` |

---

## Install

**One command:**

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/199-biotechnologies/claude-skill-gpt-pro.git \
  ~/.claude/skills/gpt-pro
```

That's it. Claude Code auto-discovers skills in `~/.claude/skills/`.

### Verify it works

In Claude Code, just say:

```
Send this to GPT Pro for review
```

Claude will activate the skill and walk you through scoping, collecting files, and packaging.

---

## How It Works

```
You: "Send this to GPT Pro"
         |
         v
   +-----------+     +-----------+     +-----------+
   | UNDERSTAND|---->|   SCOPE   |---->|  COLLECT  |
   | What's the|     | Which     |     | Copy files|
   | question? |     | files?    |     | to staging|
   +-----------+     +-----------+     +-----------+
                                            |
         +----------------------------------+
         v
   +-----------+     +-----------+     +-----------+
   |  PROMPT   |---->| SANITIZE  |---->|  PACKAGE  |
   | Write     |     | Strip all |     | tar.gz +  |
   | PROMPT.md |     | secrets   |     | clipboard |
   +-----------+     +-----------+     +-----------+
                                            |
                                            v
                                    ~/Documents/
                                    GPT Pro Analysis/
                                    project-review-2026-03-30/
                                    +-- PROMPT.md     (also in clipboard)
                                    +-- project.tar.gz
```

### Then you:

1. Open [chat.openai.com](https://chat.openai.com)
2. **Cmd+V** -- the prompt is already in your clipboard
3. Upload the `.tar.gz`
4. Send

---

## Use Cases

This skill is **domain-agnostic**. It works for anything that benefits from deep reasoning over multiple files:

| Domain | Example |
|--------|---------|
| **Code debugging** | "Why does this crash under load?" with relevant source + error logs |
| **Architecture review** | System design files + trade-off questions |
| **Data analysis** | Datasets + schemas + analysis goals |
| **Writing feedback** | Chapters + review criteria (structure, pacing, clarity) |
| **Reverse engineering** | Decompiled code + reference implementations |
| **Security audit** | Source code + threat model + compliance requirements |
| **Research synthesis** | Papers + comparison matrix + synthesis goals |

---

## What Gets Packaged

```
your-project-review-2026-03-30/
+-- PROMPT.md           # Engineered prompt with role, context, questions
+-- source/             # Relevant source files (original names preserved)
|   +-- module_a.py
|   +-- module_b.rs
|   +-- Component.tsx
+-- data/               # Supporting evidence -- logs, outputs, examples
|   +-- error_log.txt
|   +-- sample_output.json
+-- reference/          # Optional -- comparison material, prior analysis
|   +-- prior_findings.md
+-- config/             # Optional -- sanitized config
    +-- env_sanitized.txt
```

Subdirectories adapt to the domain. A book review might use `chapters/` and `feedback/`. Data analysis might use `datasets/` and `schemas/`.

---

## Prompt Template

Every `PROMPT.md` follows a proven structure:

```markdown
# <Title -- What You Want Analyzed>

## Your Role
You are a senior <domain expert> conducting <type of analysis>.

**Rules:**
1. Base conclusions on evidence from files. Flag speculation as [HYPOTHESIS].
2. Tell me what's wrong/missing/improvable and how to fix it.
3. For each finding: root cause, evidence (file:line), fix, impact.

## Context
<2-4 paragraphs: what the system does, how it works, current state>

## Current Situation
<Quantified state: metrics, error rates, performance numbers>

## Questions
### Q1: <Specific, answerable question>
**Data:** <Point to specific files>
**Hypotheses:** <2-3 possible explanations>

## Files Included
<Each file with a one-line description>

## Output Format
1. **Finding** -- what's the issue?
2. **Evidence** -- cite file:line or quote
3. **Fix** -- concrete, actionable
4. **Impact** -- what changes if fixed?
```

The template adapts per domain. The pattern is always: **Role + Context + Questions + Evidence Pointers + Output Format**.

---

## Security

This skill auto-sanitizes before packaging. It strips:

- Private keys, mnemonics, seed phrases
- API keys, auth tokens, session cookies
- Passwords, database connection strings
- Internal IPs and hostnames (replaced with `<REDACTED>`)
- Cloud credentials (AWS, GCP, Azure)

A grep check runs before every package to catch anything missed.

---

## Multi-Round Workflows

When GPT Pro returns results and you need a follow-up:

1. Save Round 1 outputs with clear attribution
2. Reference prior findings in the new `PROMPT.md`
3. Include Round 1 outputs in the new package under `reference/`

The skill handles this automatically when you say "follow up on the last GPT Pro analysis."

---

## Trigger Phrases

Say any of these in Claude Code to activate:

> "Send to GPT Pro" | "GPT Pro review" | "ChatGPT Pro review" | "Package for GPT" | "Have ChatGPT check this" | "GPT deep review" | "Browser pack" | `/gpt-pro`

---

## Contributing

Found a better prompt pattern? Have a domain-specific template that works well? PRs are welcome.

1. Fork the repo
2. Edit `SKILL.md` or add templates
3. Open a PR with before/after examples

---

## License

MIT -- use it, fork it, adapt it.

---

<div align="center">

Built by [Boris Djordjevic](https://github.com/longevityboris) at [199 Biotechnologies](https://github.com/199-biotechnologies)

A Claude Code skill for cross-model collaboration.

<br />

**If this skill saves you time:**

[![Star this repo](https://img.shields.io/github/stars/199-biotechnologies/claude-skill-gpt-pro?style=for-the-badge&logo=github&label=%E2%AD%90%20Star%20this%20repo&color=yellow)](https://github.com/199-biotechnologies/claude-skill-gpt-pro/stargazers)
&nbsp;&nbsp;
[![Follow @longevityboris](https://img.shields.io/badge/Follow_%40longevityboris-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/longevityboris)

</div>
