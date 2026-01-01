# Orchestrator Handbook v1.2

> Your reference guide for running an LLM-augmented development workflow
> Pattern: Parallel fan-out with 2 decision gates

---

## Quick-Start

**Step 1:** Copy [session-template.md](session-template.md) to `sessions/[version].md`

**Step 2:** Fill in session variables at the top

**Step 3:** Follow the template step-by-step — it provides ready-to-copy prompts

**Step 4:** Make decisions at the two gates

That's it. The template guides you through the entire workflow.

---

## Philosophy

You are the **Product Owner**, **Decision Maker**, and **Manual Orchestrator**. LLMs are your team:
- They propose, you dispose
- They execute, you validate direction
- They review each other, you break ties
- **You trigger every handoff** — nothing moves without you

### The Two Gates

Your only mandatory decision points:

```
┌──────────────────────────────────────────┐
│  1. PROCEED TO DEVELOPMENT?              │
│     After Architect revision             │
│     You've seen all critic feedback      │
│     You decide: build or clarify scope   │
├──────────────────────────────────────────┤
│  2. MERGE?                               │
│     After code review consolidation      │
│     You've seen complexity concerns      │
│     You decide: ship or address issues   │
└──────────────────────────────────────────┘
```

Everything else is execution. The template guides you through it.

### Core Principle: Simplicity Over Cleverness

This workflow enforces **engineering constraints** at every stage:
- Architect must produce a "Cut List" of deferred features
- Critics hunt for over-engineering (especially Adversarial)
- Developer can REFUSE to implement over-engineered specs (ComplexityFlag)
- Reviewers audit complexity FIRST, before looking for bugs

Complexity is treated as a liability, not an asset.

---

## Role Monitoring (Simplified)

### Architect
**Watch for:** Scope creep, YAGNI violations, speculative design
**Red flag:** "This will allow us to easily..." — that's a YAGNI violation
**Action:** Send back with "Remove the flexibility, design for THIS version only"

### Developer
**Watch for:** Stuck loops, scope creep, ComplexityFlags
**Red flags:**
- "I'll also improve/refactor X" → STOP, scope creep
- "Should I do X or Y?" → Spec was ambiguous; clarify before continuing
- 10+ file changes when spec mentioned 3 → Review diff before proceeding
- 30+ minutes with no output → May be stuck

**Intervention phrase:** "Stop. Show me what you've done so far. Let's verify we're on track."

**Handling ComplexityFlags:**
When Developer raises a flag:
```
⚠️ COMPLEXITY FLAG
I cannot implement [X] as specified because it violates [YAGNI/DRY/KISS]:
- Spec asks for: [what]
- Problem: [why over-engineered]
- Suggestion: [simpler alternative]
```

Your options:
1. **Accept:** Use simpler alternative, update spec
2. **Override:** Explain why complexity IS needed (cite specific PRD requirement)
3. **Escalate:** If unclear, ask Architect to clarify intent

ComplexityFlags are quality control, not insubordination. Take them seriously.

### Reviewers
**Watch for:** Rubber-stamp approvals, vague feedback, shallow reviews
**Rule:** Side with Adversarial on complexity concerns

**Invalid Adversarial outputs (send back):**
- "Looks good, I couldn't find issues" — Too vague
- Short review (<300 words) — Not thorough
- No complexity audit section — Must check for bloat
- Approves complexity without justification — Must explain why necessary

---

## Parallel Execution Guide

The power of v1.2 is parallel fan-out. Don't wait for responses sequentially.

### Spec Critics (Phase 1)
Send to **Claude, Gemini, and GPT simultaneously**:
- Don't wait for Claude before sending to Gemini
- Don't wait for Gemini before sending to GPT
- Gather all three, then consolidate

### Code Reviewers (Phase 3)
Send to **Standard and Adversarial simultaneously**:
- Don't wait for Standard before sending to Adversarial
- Gather both, then consolidate

**Why this matters:** Sequential execution was the main overhead in v1.1. Parallel execution cuts handoffs by ~50%.

---

## One Revision Cap

If major issues persist after one revision:
- **Requirements were unclear** → Fix requirements, don't iterate on spec
- **Scope is too large** → Split the feature
- **Fundamental disagreement** → You decide

More rounds don't fix these. They defer the problem.

**Rationale:** Convergence loops in v1.1 averaged 3-5 rounds. Most value came in round 1. Later rounds were often exhaustion, not agreement. Force clarity upfront instead.

---

## Emergency Procedures

### E1: Developer Stuck in Loop
1. STOP the agent immediately
2. Read the last 3 attempts yourself
3. Identify what assumption is wrong
4. Restart with explicit constraint: "Do NOT try [X] again. The issue is [Y]. Try [Z] instead."

### E2: Post-Merge Bug Discovered
1. REVERT immediately. Do not debug on main.
2. Create hotfix branch from last known good commit
3. Fix → Single focused review → Merge
4. Add Post-Mortem to session log

### E3: Security Vulnerability Found
1. Do NOT discuss in public PR comments or issues
2. Assess severity: Critical/High/Medium/Low
3. After fix: Document in private security log

### E4: Agentic Developer Goes Rogue
1. STOP the agent
2. `git diff` to see actual changes vs. expected
3. `git checkout` or `git revert` as needed
4. Restart with explicit constraint

### E5: Context Collapse
1. Do NOT continue
2. Use the Context Refresh prompt
3. Correct or start fresh session

### E6: Reviewers All Approve Too Easily
1. Do NOT merge
2. Ask specific questions about what they reviewed
3. Add new reviewer with "find something wrong" instruction

### E7: Developer Raises ComplexityFlag
1. READ the flag carefully
2. Evaluate: Is the simpler alternative viable?
3. Accept / Override / Escalate
4. Do NOT ignore

---

## Escalation Matrix (Simplified)

| Conflict | Resolution |
|----------|------------|
| Critics disagree | Consolidator synthesizes; you decide on conflicts |
| Adversarial blocks, Standard approves | **Adversarial wins by default** |
| ComplexityFlag raised | You decide: Accept / Override / Escalate |
| Major issues after revision | Split scope or clarify requirements |
| Reviewers all approve too easily | Add skeptical reviewer, don't merge |

**Side with the pessimist:** Complexity is sticky; bugs are transient. It's harder to remove bloat later than to fix a bug now.

---

## Quality Checklist

### Before Development (Gate 1)
- [ ] Spec has Assumptions section
- [ ] Cut List exists (or "Scope is Minimal" justified)
- [ ] Complexity concerns from Adversarial Critic addressed
- [ ] No speculative "future flexibility" in spec
- [ ] Spec is detailed enough that Developer won't need judgment calls
- [ ] **Your decision: PROCEED**

### Before Merge (Gate 2)
- [ ] Adversarial Reviewer completed complexity audit FIRST
- [ ] Adversarial review is 300+ words
- [ ] No unresolved conflicts between reviewers
- [ ] If Adversarial flagged bloat, it's resolved or explicitly accepted
- [ ] **Your decision: MERGE**

---

## Quick Reference

### Key Rules
- **Parallel fan-out:** Send to multiple models simultaneously, don't wait
- **One revision cap:** If major issues persist, scope is wrong
- **Side with pessimist:** If Adversarial blocks, default is BLOCK
- **ComplexityFlags are valid:** Developer can refuse over-engineered specs

### Anti-Sycophancy Triggers
- "What would YOU have done differently?"
- "Ultrathink" / "Think step by step"
- "Keep your integrity"
- "What's the strongest argument AGAINST this?"

### Anti-Complexity Triggers
- "What breaks if we delete this?"
- "Show me 3 concrete uses for this abstraction"
- "What PRD requirement necessitates this complexity?"

### Emergency Actions
| Situation | Immediate Action |
|-----------|-----------------|
| Developer looping | Stop → Read → Constrain |
| Post-merge bug | Revert first |
| ComplexityFlag raised | Read → Accept/Override/Escalate |
| Reviewers too easy | Add skeptical reviewer |

---

*For the session workflow, use [session-template.md](session-template.md). For LLM prompts and role definitions, see [llm-development-playbook.md](llm-development-playbook.md).*
