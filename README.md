---
name: no-slop
description: "Detects and eliminates AI slop in text, code, and design before it ships. Flags writing tells, fake facts, code smells, and design cliches in a structured scorecard. Every finding needs an exact quote and the rule it breaks. Two modes - detect only or detect and clean."
license: MIT
metadata:
  author: amit123amit
---


# no-slop

> The quality gate for AI output. Run it on anything before it ships.

Detects and eliminates AI slop across text, code, and design — in a structured
scorecard where every finding needs an exact quote and the rule it breaks.
No vibes. No invented problems. A clean pass is a normal, correct result.

---

## What is AI slop?

Content that reveals its machine origin through predictable patterns. Not because
AI wrote it — because no human reviewed it.

Three kinds:

- **Writing slop** — "delve into", "it's important to note that", significance
  inflation, soulless prose that is technically correct but dead on arrival
- **Code slop** — `data`, `result`, `temp`, obvious comments, unnecessary
  abstraction, reinventing standard library functions
- **Design slop** — purple/pink/cyan mesh gradients, "Transform your workflow"
  headlines, floating 3D shapes, generic diverse workplace photos

---

## Two modes

| Mode | When to use | What it does |
|------|-------------|--------------|
| **DETECT** (default) | Before shipping anything | Scorecard only. Never edits the output. |
| **DETECT + CLEAN** | When you want fixes applied | Scorecard first, then a rewrite. |

---

## What it checks

### Universal (runs on everything)

| Check | What it catches |
|-------|----------------|
| AI writing tells | 24 patterns: significance inflation, AI vocabulary cluster, em dashes, chatbot artifacts, sycophancy, dead-but-clean prose, and more |
| Factual accuracy | Claims verified against world-truth. Rating scale: TRUE / MOSTLY TRUE / MIXED / MOSTLY FALSE / FALSE / UNVERIFIABLE |
| Consistency | Self-contradictions, numbers that don't add up, terminology drift |
| Artifacts | Placeholders, model residue, broken markdown, truncated endings, dead links |
| Readability | Run-ons, buried lede, jargon stacks, wall of text, ambiguous pronouns |
| Pulse | Clean-but-soulless detection — no opinions, no rhythm, press-release energy |

### Domain checks (fire based on content type)

| Check | What it catches |
|-------|----------------|
| Code quality | Generic variable names, obvious comments, unnecessary abstraction, reinvented stdlib, magic numbers |
| Design slop | Generic gradients, layout antipatterns, "empower your business" headlines, stock photo aesthetics |
| Voice | Brand voice compliance (template — fill in your own brand rules) |
| Completeness | Did the output actually do what was asked? |

---

## Scorecard format

Every run produces one table. Greens included. No improved version, no apply
prompt, no invented problems.


<img width="820" height="529" alt="Screenshot 2026-08-03 at 3 02 25 AM" src="https://github.com/user-attachments/assets/965f9a7c-c48c-4d18-b9fd-6ce194236e01" />




---

## How to trigger it

Say any of these to Claude:

- `no slop`
- `de-slop this`
- `slop check`
- `is this ready to send`
- `does this sound like AI`
- `review before I send`
- `clean this up`
- `remove AI patterns`

Or just paste any output and ask if it's good to go.

---

## Skill structure

no-slop/
├── SKILL.md # Main skill — workflow, checks, output format
└── references/
└── checks/
├── ai-writing.md # 24 AI writing tell patterns + pulse check
├── factual-accuracy.md # Fact verification process and rating scale
├── consistency.md # Internal contradiction detection
├── artifacts.md # Placeholders, model residue, broken structure
├── readability.md # Clarity at sentence, structure, and word level
├── code-quality.md # Naming, comments, structure, over-engineering
├── design-slop.md # Visual, layout, and copy design patterns
└── voice.md # Brand voice template (fill in your own)

---

## Install

### In Claude.ai

1. Download `no-slop.skill`
2. Go to **Settings → Skills**
3. Click **Add skill** and upload the file

### Manual (Claude Code / any MCP setup)

Clone this repo and point your skill path at the `no-slop/` directory.

```bash
git clone https://github.com/<your-username>/no-slop.git
```

---

## Design principles

**Every finding needs a quote.** "Feels off" is not a finding. If you cannot
produce the exact span from the output and the rule it breaks, drop it.

**A clean pass is correct.** The skill does not invent problems to look
thorough. `✅ Good to go` is a real, common, normal result.

**Detect only by default.** The skill never silently edits output. DETECT mode
produces a scorecard. DETECT + CLEAN produces a scorecard and then a rewrite,
in that order.

**Context beats rules.** Academic writing needs hedging. Legal docs need
precision. The check flags overuse, not existence.

---

## What this combines

Built by merging the best of two community skills:

- **anti-slop** — comprehensive pattern catalogs for text, code, and design;
  Python detection and cleanup scripts
- **de-slop** — 24-pattern AI writing tell system; structured grading with
  Aligned / Drift / Misaligned; scorecard output format; factual accuracy and
  artifact checks

The combined skill adds: the pulse check as a first-class finding, code quality
and design slop checks with the same rigorous grading structure, and explicit
DETECT vs DETECT + CLEAN mode separation.

---

## Contributing

PRs welcome for:
- New check reference files (voice templates, domain-specific contexts)
- Additional pattern catalogs (AI image tells, slide deck slop, etc.)
- Translations of the pattern catalogs
- Improvements to the scoring criteria

---

## License

MIT

---
