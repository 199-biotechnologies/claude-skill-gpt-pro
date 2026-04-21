<div align="center">

# Claude Skill: GPT Pro Review

**Get a second opinion from ChatGPT Pro -- one command packages everything.**

<br />

[![Star this repo](https://img.shields.io/github/stars/199-biotechnologies/claude-skill-gpt-pro?style=for-the-badge&logo=github&label=%E2%AD%90%20Star%20this%20repo&color=yellow)](https://github.com/199-biotechnologies/claude-skill-gpt-pro/stargazers)
&nbsp;&nbsp;
[![Follow @longevityboris](https://img.shields.io/badge/Follow_%40longevityboris-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/longevityboris)

<br />

[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blueviolet?style=for-the-badge&logo=anthropic)](https://docs.anthropic.com/en/docs/claude-code/skills)
&nbsp;
[![ChatGPT Pro](https://img.shields.io/badge/ChatGPT-Pro-10a37f?style=for-the-badge&logo=openai&logoColor=white)](https://chat.openai.com)
&nbsp;
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
&nbsp;
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](https://github.com/199-biotechnologies/claude-skill-gpt-pro/pulls)

---

You work in Claude Code. Sometimes you want a second set of eyes from a different model. This skill packages your problem -- files, context, and an engineered prompt -- into a tar.gz bundle ready to upload to ChatGPT Pro. One command. No copy-pasting. No lost context.

[Why This Exists](#why-this-exists) | [Install](#install) | [How It Works](#how-it-works) | [Use Cases](#use-cases) | [Contributing](#contributing)

</div>

---

## Why This Exists

Every model has blind spots. Claude might miss something GPT catches, and vice versa. Cross-model review is the cheapest way to find bugs, validate architecture decisions, and catch reasoning errors before they ship.

But getting a good answer from ChatGPT Pro requires a good prompt. Raw code dumps produce vague responses. Structured prompts with expert roles, specific questions, and evidence pointers produce actionable findings.

This skill encodes the prompt engineering patterns that consistently work. You tell Claude what you want reviewed. It packages the right files, writes a structured prompt, strips secrets, and puts everything in your clipboard. You paste, upload, send.

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

```bash
git clone https://github.com/199-biotechnologies/claude-skill-gpt-pro.git \
  ~/.claude/skills/gpt-pro
```

Claude Code auto-discovers skills in `~/.claude/skills/`. No config needed.

### Verify

In Claude Code, say:

```
Send this to GPT Pro for review
```

Claude activates the skill and walks you through scoping, collecting files, and packaging.

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
                                    +-- PROMPT.md     (copied to clipboard)
                                    +-- project.tar.gz
```

### Then you:

1. Open [chat.openai.com](https://chat.openai.com)
2. **Cmd+V** -- the prompt is already in your clipboard
3. Upload the `.tar.gz`
4. Send

That's the entire workflow. Claude does the hard part (structuring the prompt, choosing the right files, sanitizing secrets). You do the easy part (paste and upload).

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
+-- reference/          # Comparison material, prior analysis
|   +-- prior_findings.md
+-- config/             # Sanitized config
    +-- env_sanitized.txt
```

Subdirectories adapt to the domain. A writing review might use `chapters/` and `feedback/`. Data analysis might use `datasets/` and `schemas/`.

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

The template adapts per domain. The constant pattern is: **Role + Context + Questions + Evidence Pointers + Output Format**.

---

## Use Cases

This skill is domain-agnostic. It works for anything that benefits from deep reasoning over multiple files.

| Domain | Example |
|--------|---------|
| **Code review** | "Why does this crash under load?" with relevant source + error logs |
| **Architecture review** | System design files + trade-off questions |
| **Security audit** | Source code + threat model + compliance requirements |
| **Data analysis** | Datasets + schemas + analysis goals |
| **Writing feedback** | Chapters + review criteria (structure, pacing, clarity) |
| **Research synthesis** | Papers + comparison matrix + synthesis goals |
| **Reverse engineering** | Decompiled code + reference implementations |
| **AI prompt review** | Prompt files + expected vs actual outputs |

---

## Security

The skill auto-sanitizes before packaging. It strips:

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
2. The skill references prior findings in the new `PROMPT.md`
3. Round 1 outputs go into the new package under `reference/`

Tell Claude "follow up on the last GPT Pro analysis" and it handles this automatically.

---

## Trigger Phrases

Say any of these in Claude Code to activate:

> "Send to GPT Pro" | "GPT Pro review" | "ChatGPT Pro review" | "Package for GPT" | "Have ChatGPT check this" | "GPT deep review" | `/gpt-pro`

---

## Contributing

Found a better prompt pattern? Have a domain-specific template? PRs are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

[MIT](LICENSE) -- use it, fork it, adapt it.

---

<div align="center">

Built by [Boris Djordjevic](https://github.com/longevityboris) at [199 Biotechnologies](https://github.com/199-biotechnologies) | [Paperfoot AI](https://paperfoot.ai)

<br />

**If this is useful to you:**

[![Star this repo](https://img.shields.io/github/stars/199-biotechnologies/claude-skill-gpt-pro?style=for-the-badge&logo=github&label=%E2%AD%90%20Star%20this%20repo&color=yellow)](https://github.com/199-biotechnologies/claude-skill-gpt-pro/stargazers)
&nbsp;&nbsp;
[![Follow @longevityboris](https://img.shields.io/badge/Follow_%40longevityboris-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/longevityboris)

</div>
