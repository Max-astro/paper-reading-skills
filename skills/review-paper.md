---
description: Critically review paper understanding and reading notes quality
allowed-tools: Read, Edit, Write, Glob, Grep, Task, Bash(git status:*), Bash(git add:*), Bash(git commit:*)
---

Review paper reading notes using three sequential passes. This command runs end-to-end without confirmation checkpoints — all passes execute, fixes are applied, and a summary is presented at completion.

## Philosophy

Rush nothing. This is craft that rewards reflection — step back, let ideas settle, allow connections to surface that aren't obvious at first pass.

Each pass builds on the previous. Run them in order: comprehension → critique → integration. Apply fixes from each pass before proceeding to the next — comprehension gaps from pass 1 would invalidate critique work if done out of order.

## Setup

**First action before any other work:** Request all permissions upfront so the rest runs uninterrupted.

1. Read a file from `docs/papers/` (confirms notes exist)
2. Run a Grep search on the papers directory (triggers Grep permission)
3. Launch a quick Task to confirm Task tool access (triggers Task permission)
4. Run `git status` to confirm git access (triggers Bash permission)

Get all approvals immediately, then proceed with the workflow.

If `docs/papers/` is empty or doesn't exist, stop and inform the user — nothing to review.

## Invocation

- `/review-paper` — Run all three passes on most recent paper
- `/review-paper comprehension` — Comprehension pass only
- `/review-paper critique` — Critique pass only
- `/review-paper integration` — Integration pass only
- `/review-paper <slug>` — All passes on a specific paper

## The Three Passes

### Pass 1: Comprehension Audit

**Persona:** Experienced professor conducting a reading group. You care about whether the reader *actually understands* the paper — not just whether they took notes.

**On approach:** Channel the Recurse Center ethos — real understanding means you can explain it, rebuild it, and argue with it. Summaries that paraphrase the abstract are a red flag.

**Focus:** Does the reader understand the paper, or just *think* they understand it?

**Check for:**

*Understanding gaps:*
- Synthesis note TL;DR that's just the abstract reworded (no genuine compression happened)
- Method description that recites steps but doesn't explain *why each step is needed*
- Missing intuition — equations presented without interpretation
- "Core idea" that describes *what* but not *why it works*

*Explanation quality:*
- Could someone reconstruct the approach from the deep-read notes alone?
- Are the ASCII diagrams actually clarifying, or decorative?
- Do the walk-through examples use realistic values and cover edge cases?
- Is boundary behavior of key equations explored?

*Completeness:*
- All core sections from structure map covered in deep-read?
- Key equations identified and unpacked?
- Experimental results assessed, not just reported?

**Methodology:**
1. Read `00-triage.md` for initial expectations
2. Read `01-structure.md` for what was identified as core
3. Read `02-deep-read.md` — does it deliver on the structure map's promises?
4. Read `03-synthesis.md` — does it compress genuinely, or just paraphrase?
5. Cross-check: Does the understanding verification checklist actually pass?
6. Apply fixes directly — add missing intuition, deepen explanations, fill gaps

**Reflect before proceeding:** After fixing comprehension issues, are there claims in the notes that should now be questioned? Note areas for critique pass.

**Commit:** `papers: comprehension audit <slug> (pass 1/3)`

---

### Pass 2: Critical Assessment

**Persona:** Peer reviewer who has read hundreds of papers in this area. You assess the paper's actual contribution — not just what the notes say about it.

**On approach:** Channel Martin Kleppmann's rigor — trace the argument, verify the logic, check that trade-offs are acknowledged. Bring the skeptical reviewer mindset: separate what's shown from what's claimed.

**Focus:** Is the reader's assessment of the paper fair and rigorous?

**Check for:**

*Overclaiming in notes:*
- Strengths listed without evidence (e.g., "elegant approach" — why is it elegant?)
- Weaknesses that are too gentle or miss obvious issues
- Results accepted uncritically (did the notes actually check baselines, compute budgets, statistical significance?)

*Missing critical perspective:*
- Implicit assumptions the paper makes but doesn't acknowledge
- Failure modes or edge cases not discussed
- Comparison with alternative approaches that the paper ignores
- Scalability, generalizability, or reproducibility concerns

*Argument integrity:*
- Does the paper's evidence actually support its claims?
- Are there logical gaps in the argument chain?
- Do the notes flag these, or accept the paper's narrative at face value?

**Methodology:**
1. Re-read the original paper alongside the notes
2. For each claim in the synthesis: trace it back to evidence in the paper
3. Identify what the paper assumes but doesn't prove
4. Check: Are the "Questions & Follow-ups" genuinely probing, or surface-level?
5. Use Task tool to search for related work that the paper might have missed or the notes should reference
6. Apply fixes — strengthen critique sections, add missing questions, note unsupported claims

**Reflect before proceeding:** Has the critical assessment changed the overall evaluation of the paper? Should the TL;DR or "Core Idea" be revised?

**Commit:** `papers: critical assessment <slug> (pass 2/3)`

---

### Pass 3: Integration Review

**Persona:** Research advisor helping connect this paper to the reader's broader knowledge and current projects.

**On approach:** This pass is about *connections* — between papers, between theory and practice, between what you've read and what you're building. A paper read in isolation is quickly forgotten. A paper connected to your work becomes a tool.

**Focus:** Connections, applications, and the growing knowledge graph.

**Check for:**

*Cross-paper connections:*
- Does the Connections section reference related papers already in `docs/papers/`?
- Are those connections substantive ("both use attention mechanisms" is weak; "this paper's sparse attention solves the scalability problem noted in [other paper]" is strong)
- Should older paper notes be updated to reference this new paper?

*Project relevance:*
- Are connections to current work specific and actionable, or vague?
- Could any technique or insight from this paper be applied to current projects?
- If so, what would be the first concrete step?

*Knowledge gaps exposed:*
- Did this paper reveal background knowledge the reader is missing?
- Are there foundational papers that should be read to better understand this one?
- Are the "Papers to read next" recommendations well-chosen?

*Note durability:*
- Will the synthesis note make sense in 6 months? Are there implicit references that need to be made explicit?
- Is the vocabulary section complete?
- Would a table-of-contents or tag system help with future retrieval?

**Methodology:**
1. Read all existing paper notes in `docs/papers/`
2. Identify genuine connections (not forced ones)
3. Update the current paper's Connections section
4. Update related papers' Connections sections to cross-reference
5. Check if the Questions & Follow-ups suggest a clear next-reading path
6. Apply fixes — enrich connections, add reading-path suggestions, improve long-term note quality

**Commit:** `papers: integration review <slug> (pass 3/3)`

---

## Output Summary

After all passes complete, present:

```
## Review Summary: [Paper Title]

### Comprehension Audit
- [Understanding gaps found and filled]
- [Explanations deepened: N sections]
- [Missing intuition added for: equations/diagrams]

### Critical Assessment
- [Overclaiming corrected: N instances]
- [Critical questions added: N]
- [Unsupported claims flagged: N]

### Integration Review
- [Cross-paper connections made: N]
- [Older papers updated: list]
- [Suggested next readings: list]

### Understanding Score
Before review: [assessment]
After review: [assessment]
Key remaining gap: [the single most important thing still not fully understood]

### Areas for Reader Attention
- [Anything that requires the reader to go back to the original paper]
- [Concepts that need external resources to fully understand]
- [Decisions about interpretation that could go multiple ways]
```

## When to Flag vs Fix

**Flag for reader attention when:**
- Multiple valid interpretations of a passage exist
- Technical accuracy depends on domain knowledge the reviewer lacks
- A fix would change the overall assessment of the paper
- Understanding requires reading a prerequisite paper first

**Fix autonomously when:**
- The note clearly misrepresents what the paper says
- A connection to an existing paper note is obvious
- Formatting or structural issues in the notes
- Missing information that's clearly present in the paper

## Rules

- Read existing notes before modifying
- Run passes in order: comprehension → critique → integration
- Apply fixes from each pass before proceeding to next
- Always re-read the original paper (not just the notes) during critique pass
- Use Task tool for background research when checking technical claims
- Flag items requiring reader judgment — don't guess on ambiguous interpretations
- When in doubt about a paper's claim, note the doubt rather than resolving it
