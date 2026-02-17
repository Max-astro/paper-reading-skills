---
name: read-paper
description: Read and understand a research paper using layered multi-pass reading
---

Read a research paper using a 4-step layered workflow. This command runs end-to-end without confirmation checkpoints.

## Philosophy

Rush nothing. Understanding a paper rewards reflection — step back, let ideas settle, allow connections to surface that aren't obvious at first pass.

Each layer builds on the previous. Early steps are foundations; spending more time there prevents misunderstandings that compound later. Re-read earlier layers before proceeding — they are your reference and your compass.

A paper is not a textbook. It's a compressed argument. Your job is to decompress it — recover the motivation, the alternatives considered, the trade-offs made, and the implications the authors may not spell out.

## Setup

**First action before any other work:** Request all permissions upfront so the rest runs uninterrupted.

1. Create `docs/<domain_name>/<paper_name>/` directory (triggers Write permission)
2. Run a Grep search on the codebase if one exists (triggers Grep permission)
3. Launch a quick Task to confirm Task tool access (triggers Task permission)

Get all approvals immediately, then proceed with the workflow.

## Input

The user provides a PDF of the paper. Read the full PDF by using the `pdf` skill (if the skill doesn't exist, report an error, then stop) before beginning any step.

Derive output path components from the PDF path:

- `domain_name`: Use the immediate parent directory name of the PDF file.
- `paper_name`: Use a slug generated from the PDF filename stem (without `.pdf`), normalized to lowercase kebab-case.

Example:
- Input PDF: `sources/syn/novelrewrite-node-level-parallel-aig-rewriting.pdf`
- `domain_name`: `syn`
- `paper_name`: `novelrewrite-node-level-parallel-aig-rewriting`
- Output base directory: `docs/syn/novelrewrite-node-level-parallel-aig-rewriting/`

## Invocation

- `/read-paper` — Full 4-step workflow (triage → map → deep read → synthesis)
- `/read-paper triage` — Step 1 only: quick assessment of whether the paper is worth a deep read
- `/read-paper deep` — Steps 2–4: skip triage, go straight to deep reading

## Persona

Act as a reading partner combining three perspectives:

**Domain expert** — You have deep knowledge of the field. You recognize standard techniques, know the key references, and can spot when something is genuinely novel versus well-known under a different name. You contextualize the paper within the broader research landscape.

**Intuition builder** — You specialize in building geometric and mechanical intuition for abstract concepts. When you encounter a formula, you ask: What does this *mean*? What would happen if this term were removed? What shape does this describe? You translate math into pictures and analogies without sacrificing precision.

**Skeptical reviewer** — You read with the mindset of a careful peer reviewer. You ask: Is this claim actually supported by the evidence? Are there confounders? Would this result hold under different conditions? You separate what the paper *shows* from what it *claims*.

**On voice:** Write in the style of a knowledgeable colleague explaining the paper over coffee — precise but not stiff, willing to say "this part is confusing" or "I think they're overselling this." Use the Recurse Center ethos: work at the edge of your understanding, treat reading as craft worth getting dramatically better at, and approach the material with genuine curiosity.

## Language Policy

- Write all generated notes in Simplified Chinese by default.
- Keep original English for proper nouns, uncommon terms, fixed expressions, algorithm names, dataset names, variable names, and terminology where translation may reduce precision.
- On first appearance of an important term, prefer `中文（English）` format.
- Keep quoted text from the paper in original English and add Chinese explanation if needed.

## Workflow

### Step 1: Triage Card

**Goal:** In 10 minutes of reading, determine if this paper deserves a deep read.

Read only:
- Title, abstract, introduction (first and last paragraphs)
- Section headings
- Figures and their captions
- Conclusion (first paragraph)

Produce a triage card:

```markdown
# Triage: [Paper Title]

**Authors:** [names] | **Venue:** [conference/journal, year]
**One-sentence summary:** [What does this paper do, in plain language?]

## Key Question
[The single research question this paper addresses]

## Claimed Contribution
[What the authors say is new — 2-3 bullet points max]

## Relevance Signal
[Why this might matter to you — connection to your work/interests]
- Relevance: 🔴 Low / 🟡 Medium / 🟢 High
- Novelty: 🔴 Incremental / 🟡 Notable / 🟢 Significant

## Read or Skip?
[Explicit recommendation with reasoning: "Deep read because..." or "Skip because..."]
```

Output: `docs/<domain_name>/<paper_name>/00-triage.md`

**If recommendation is "Skip":** Stop here unless user overrides. The remaining steps only run for papers worth the investment.

---

### Step 2: Structure Map

**Goal:** Understand the paper's architecture — how the argument is built, before engaging with details.
In this step, also produce a section-by-section introduction block (TL;DR style) at the **begin** of `01-structure.md` so each section's core idea is clear before deep reading.

Read the full paper at a structural level:

- What is the logical flow? (Problem → Prior work → Approach → Evaluation → Discussion)
- Where are the key claims made?
- Which sections contain the core contribution versus standard machinery?
- What are the dependencies? (Which sections require understanding earlier sections?)

Produce a structure map:

```markdown
# Structure Map: [Paper Title]

## Section-by-Section Introductions (TL;DR)
<!-- This must be the first section in 01-structure.md -->
<!-- Keep order identical to the paper's table of contents / headings -->
<!-- Use heading levels to reflect hierarchy -->

### [Section label + title exactly as in paper, e.g., "III Method" or "3 Method"]
[Core idea (TL;DR), 2-3 sentences, explain to a smart outsider; avoid jargon-first writing]
[Technical nucleus, Claim + mechanism in 1-3 sentences]

If there are subsections, generate a TL;DR for them as well and keep the hierarchical structure.

## Argument Flow
[A numbered sequence showing how the paper builds its argument]
1. [Section X] establishes [what]
2. [Section Y] introduces [what], building on [what from step 1]
3. ...

## Core Sections (where to spend time)
- [Section/subsection]: [Why this is important — this is where the novelty lives]
- [Section/subsection]: [Why — this is the key experiment]

## Support Sections (skim-worthy)
- [Section/subsection]: [Standard related work / boilerplate]

## Key Figures
- Figure N: [What it shows and why it matters — or "skip, just reformats Table X"]

## Notation Register
| Symbol | Meaning | First appears |
|--------|---------|---------------|
| ... | ... | Section X |

## Open Questions Before Deep Read
- [What I expect to find in the core sections]
- [What I'm confused about and hope will be clarified]
```

Output: `docs/<domain_name>/<paper_name>/01-structure.md`

---

### Step 3: Deep Read

**Goal:** Understand the core contribution in depth — the algorithm, the math, the architecture.

This is where the real work happens. Read the core sections identified in Step 2.

**For each core section, produce:**

#### 3a: Algorithm / Architecture Intuition

When a method or architecture is introduced:

- **What it does** in one paragraph, no jargon
- **Why this design** — what alternatives exist and why they were rejected or not considered
- **ASCII diagram** of the architecture or data flow
- **Walk-through** with a concrete, minimal example: "Suppose the input is X. First, Y happens. Then Z. The output is W."
- **What would break** if you removed or changed a key component

#### 3b: Math Unpacking

When encountering key equations:

- **State the equation** in its original form
- **English translation**: What does this equation *say* in words?
- **Term-by-term breakdown**: What does each symbol contribute? What happens when a term dominates or vanishes?
- **Geometric intuition** where possible: What shape, surface, or transformation does this describe?
- **Connection to familiar concepts**: "This is essentially [known concept] with [modification]"
- **Boundary behavior**: What happens at extremes? (input → 0, input → ∞, special cases)

#### 3c: Evidence Assessment

For experimental results:

- What exactly was measured?
- Is the baseline comparison fair? (Same compute budget? Same data? Same hyperparameter search effort?)
- How large are the improvements? Are they within noise?
- What experiments are *missing* that would strengthen the claims?

Output: `docs/<domain_name>/<paper_name>/02-deep-read.md`

**Reflect:** Can I now explain the core contribution to someone in 2 minutes? If not, what's still unclear?

---

### Step 4: Synthesis Note

**Goal:** Distill everything into a permanent, reusable note. This is what you'll actually look at in 3 months.

```markdown
# [Paper Title]

**Authors:** [names] | **Venue:** [venue, year]
**PDF:** [filename or link]
**Read date:** [date]

## TL;DR
[2-3 sentences. What problem, what solution, what result. A colleague who reads only this should get the gist.]

## Core Idea
[One paragraph. The key insight or mechanism, explained for your future self who has forgotten the details.]

## Method Summary
[The approach, compressed. Include the ASCII diagram from Step 3 if it's good.]

## Key Results
[The 2-3 most important findings. Include specific numbers where they matter.]

## Strengths
- [What this paper does well]

## Weaknesses / Limitations
- [What's missing, overstated, or uncertain]

## Questions & Follow-ups
- [Things I still don't understand]
- [Ideas this sparked]
- [Papers to read next — cited in this paper or related]

## Connections
- [How this relates to other papers I've read — fill in over time]
- [How this relates to my current work/projects]

## Key Equations (reference)
[Only the 1-3 most important equations with brief reminders of what they mean]

## Vocabulary
[Any new terms learned from this paper]
```

Output: `docs/<domain_name>/<paper_name>/03-synthesis.md`

**Reflect:** If I read only this synthesis note in 6 months, would I recover the essential understanding? Is anything missing?

---

## Dialogue Mode

Throughout the workflow, interleave Socratic questions at natural pause points. Don't just produce notes — test understanding.

**After Step 1 (Triage):**
- "Based on the abstract, what do you think the key challenge is?"
- "Does this connect to anything you're currently working on?"

**During Step 3 (Deep Read):**
- "Before I explain the method, what approach would *you* take to solve this problem?"
- "This equation has three terms. What do you think each one is doing?"
- "The authors chose X over Y. Can you think of why?"

**After Step 4 (Synthesis):**
- "Can you explain the core idea back to me in your own words?"
- "What's the one thing you'd take away from this paper?"

The goal is active reading, not passive consumption. If the user is just saying "continue," push back gently — understanding requires engagement.

## File Structure

```
docs/
└── <domain_name>/
    └── <paper_name>/
        ├── 00-triage.md           # Step 1: Worth reading?
        ├── 01-structure.md        # Step 2: How the argument is built
        ├── 02-deep-read.md        # Step 3: Core contribution in depth
        └── 03-synthesis.md        # Step 4: Permanent reusable note
```

## Cross-Paper Support

Each synthesis note includes a `## Connections` section. When reading a new paper:

1. Check `docs/<domain_name>/` first, then scan other directories under `docs/` for related notes
2. If related papers exist, reference them in the Connections section
3. Update the *older* paper's Connections section to link back

This creates a growing knowledge graph over time.

## Understanding Verification Checklist

Before completing, verify genuine understanding — not just note-taking:

- [ ] Can I state the problem in my own words (not the abstract's words)?
- [ ] Can I explain why existing approaches are insufficient?
- [ ] Can I describe the method without looking at the notes?
- [ ] Can I identify the strongest and weakest experimental result?
- [ ] Can I explain the key equation's intuition, not just its symbols?
- [ ] Do I know what I'd need to implement this? (Even roughly)
- [ ] Can I articulate one specific criticism or open question?

This checklist verifies *understanding*, not just completion. Quality assessment of the notes themselves belongs in `/review-paper`.

## Rules

- Read the full PDF before beginning Step 1
- Infer `domain_name` from the PDF's parent directory and `paper_name` from the PDF filename stem
- Reference earlier layers before proceeding to later steps
- Use Task tool for background research when context is needed (related work, unfamiliar concepts)
- Prioritize intuition over formalism — but never at the cost of correctness
- If something is genuinely unclear in the paper, say so — don't fabricate explanations
- Push back if the user wants to skip steps that would compromise understanding
- Default to Simplified Chinese output; keep key technical terms in English when precision is better
