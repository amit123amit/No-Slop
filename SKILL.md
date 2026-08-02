---
name: no-slop
description: >-
  Quality gate for AI output across text, code, and design. Use when the user
  says no slop, de-slop, slop check, clean this up, is this ready to send,
  does this sound like AI, or review before I send. Also use when any output
  feels generic, hollow, or AI-washed. Runs a structured scorecard flagging
  writing tells, filler phrases, buzzwords, fake facts, code smells, design
  cliches, placeholders, and broken artifacts. Every finding needs an exact
  quote and the rule it breaks. Two modes - detect only produces a scorecard,
  detect and clean produces a scorecard then rewrites. Default is detect only.
license: MIT
compatibility: Works with Claude, Claude Code, and any agent that supports SKILL.md
metadata:
  author: amit123amit
  version: "1.0"
---

# No-Slop

The unified quality gate for AI output. Run it on anything before it ships.

Two jobs in one skill:
1. **Detect** — scorecard of every slop signal found, with exact quotes and suggestions. Never edits the output itself.
2. **Clean** — if the user asks to fix or clean, rewrite after the scorecard. Not before.

Default mode is DETECT. Switch to DETECT + CLEAN when the user says "fix it", "clean it", "rewrite it", "apply the fixes."

---

## What is AI Slop?

Content that reveals its machine origin through predictable patterns — not because AI wrote it, but because no human reviewed it. Three kinds:

- **Writing slop**: Phrases, hedges, buzzwords, and structures that flood AI output because they statistically fit many contexts. Specificity is the antidote.
- **Code slop**: Generic variable names, obvious comments, unnecessary abstraction. Code that compiles but communicates nothing.
- **Design slop**: Cookie-cutter layouts, purple/pink/cyan gradients, "empower your business" headlines. Visual and copy patterns that come from templates, not decisions.

And the failure mode that none of the patterns catch: **clean but soulless**. Technically correct, no opinions, no rhythm, no pulse. Still slop.

---

## Step A — Classify the input

Before checking, determine:

- **Type**: text/copy, code, design, or mixed.
- **Audience**: external (about to ship to a customer or the public), internal (team-facing), or personal.
- **Context**: What was the output trying to do? If a clear request is visible in the conversation, note it — it is the Completeness check benchmark.

If the input is ambiguous, infer and print your read at the top so a wrong assumption is visible. Ask only what you genuinely cannot infer.

---

## Step B — Which checks fire

Run all universal checks on every input. Run domain checks based on what the input contains.

### Universal checks (always run)

| Check | Fires when | Reference |
|---|---|---|
| AI writing tells | any text present | `references/checks/ai-writing.md` |
| Factual accuracy | any factual claim present | `references/checks/factual-accuracy.md` |
| Consistency | always | `references/checks/consistency.md` |
| Artifacts | always | `references/checks/artifacts.md` |
| Readability | any text or doc | `references/checks/readability.md` |
| Pulse | any text present | inline — see pulse section below |

### Domain checks (fire based on content type)

| Check | Fires when | Reference |
|---|---|---|
| Code quality | code present | `references/checks/code-quality.md` |
| Design slop | visual/design content present | `references/checks/design-slop.md` |
| Voice | copy present AND a brand/voice context is available | `references/checks/voice.md` |
| Completeness | a clear ask is visible in context | grade against the ask itself, no file |

Load a check's reference file only when that check fires. Do not preload all files.

---

## Step C — Grade each check

Read each fired check's reference file. Grade independently — never let one check color another.

For each finding you must have:
- **Quote**: the exact span from the output (or the visual element)
- **Breaks**: the rule it breaks, named specifically
- **Suggestion**: see tiers below
- **Severity**: blocker, should-fix, or polish

**Suggestion tiers:**
- Mechanical checks (AI writing tells, Artifacts, Readability, Code quality): quote the exact offending span so the user sees precisely what to touch.
- Judgment checks (Factual accuracy, Consistency, Voice, Completeness, Design slop): give awareness-level advice on the kind of problem, not a line-edit.

If you cannot produce Quote + Breaks + Suggestion, it is not a finding. Drop it. Never invent findings to look thorough.

---

## Step D — The pulse check (run after all other checks)

A separate pass over the text, independent of pattern detection.

Signs of a dead-but-clean output:
- Every sentence the same length and structure
- No opinions, only neutral reporting
- No acknowledgment of uncertainty or complexity
- No first person where it would fit
- No humor, edge, or surprise
- Reads like a press release or legal brief when it shouldn't

What gives writing a pulse: real opinions, varied rhythm (short punch, then a longer sentence that takes its time), acknowledged complexity ("impressive but also kind of unsettling"), first person where honest, a little mess (an aside, a tangent), specific feelings instead of generic ones.

If the output is technically clean but has no pulse, grade it as Drift with a suggestion of where to inject an opinion, a varied sentence, or a specific reaction.

---

## Step E — Set the verdict

Severity definitions:
- **blocker**: false claim, hard guardrail break in outbound copy, output failing the job it was asked to do, placeholder shipping, or the output is fundamentally the wrong thing.
- **should-fix**: real, fixable, not fundamental.
- **polish**: cosmetic, optional.

Verdict (becomes the title in Step F):
- `# ✅ Good to go` — all fired checks aligned, or only polish findings.
- `# ✅⚠️ Good to go, with a few things to fix` — drift or should-fix findings, no blockers.
- `# ❌ Not ready` — any blocker.

---

## Step F — Render the scorecard

Render in this exact format. Nothing else — no improved version, no numbered changes list, no apply prompt.

```
# <verdict title>

<one short line summarizing the situation — skip on a fully clean run>

| Check | Status | What's off | Source | Suggestion |
|-------|--------|-----------|--------|-----------|
| <name> | <✅/⚠️/❌ + words> | <short, or — if green> | <specific standard> | <suggestion or —> |
```

Status labels: `✅ Good to go` / `⚠️ Might wanna fix` / `❌ Not ready`

Source column — name the real standard, not a generic tag:
- AI writing tells → `Signs of AI writing`
- Factual accuracy → `World-truth / fact-checker`
- Consistency → `Internal coherence`
- Artifacts → `Universal slop standard`
- Readability → `Universal readability standard`
- Pulse → `Pulse check`
- Code quality → `Code quality standard`
- Design slop → `Design slop standard`
- Voice → `voice.md` or `brand.md`
- Completeness → `The ask`

Show every fired check as its own row, greens included. Use `—` in What's off and Suggestion for green rows.

---

## Step G — Clean mode (only if requested)

If the user asks to fix, clean, rewrite, or apply the fixes: rewrite the output after the scorecard, addressing every finding. Label the rewritten version clearly. Do not silently apply fixes — always show the scorecard first.

---

## Pulse check (quick inline reference)

No separate file needed. Grade as part of Step D:

**Pulse killers to flag:**
- All sentences 15-25 words, identical rhythm throughout
- Zero opinions stated (everything hedged or neutral)
- No variation in paragraph length
- No specific feelings — only generic positivity or neutral description
- Closing with "exciting times ahead" / "a step in the right direction" / "the future looks bright"

**Pulse builders to suggest:**
- One short sentence after a long one
- One stated opinion (not "some might say" — an actual "I think" or "this is the part that matters")
- One specific detail that only someone who was there would know
- One acknowledged tension ("this works well, but the tradeoff is...")

---

## Common scenarios

**"De-slop this email before I send it"**
→ Classify as external copy. Run all universal checks + Voice (if brand context available) + Completeness (if original ask visible). DETECT mode. Show scorecard.

**"Fix the slop in this article"**
→ Classify as text/copy. Run universal checks. DETECT + CLEAN mode. Show scorecard first, then rewrite.

**"Does my code look AI-generated?"**
→ Classify as code. Run AI writing tells (if doc comments present) + Code quality + Artifacts. DETECT mode.

**"Review this landing page copy"**
→ Classify as external copy + design. Run all universal checks + Design slop. DETECT mode.

**"Is this ready to ship?"**
→ Run everything relevant. Set the verdict. The verdict is the answer.

---

## Hard rules

- Every finding rests on an exact quote and the rule it breaks. "Feels off" is not a finding.
- The skill detects and suggests in DETECT mode. It never edits or rewrites. The human owns the fix.
- In CLEAN mode, show the scorecard first, then the rewrite. Never silently fix.
- A clean `# ✅ Good to go` is normal and correct — do not invent findings.
- No em dashes anywhere you write.
- If a reference file will not load, mark that check Skipped with the reason. A silent pass is a lie.
- Context beats rules. Not all patterns are always slop. Academic writing needs hedging. Legal docs need precision. Judge overuse, not existence.

---

## Reference files

Load only what fires:

- `references/checks/ai-writing.md` — 24 AI writing tell patterns + the pulse check
- `references/checks/factual-accuracy.md` — Fact verification process and rating scale
- `references/checks/consistency.md` — Internal contradictions and number reconciliation
- `references/checks/artifacts.md` — Placeholders, model residue, broken structure
- `references/checks/readability.md` — Sentence, structure, and word-level clarity
- `references/checks/code-quality.md` — Naming, comments, structure, over-engineering
- `references/checks/design-slop.md` — Visual, layout, and copy patterns in design
- `references/checks/voice.md` — Brand voice (template — fill in with your brand)
