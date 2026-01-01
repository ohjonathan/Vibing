# Review: Gemini's "Council of Three" Proposal

**Reviewer:** Claude Opus 4.5
**Date:** 2026-01-01
**Subject:** Response to "Branch-Based Orchestration to Reduce Handoff Friction"

---

## The Problem: Valid

Gemini correctly identifies a real issue:

> "The user is forced to act as a 'Message Bus,' copy-pasting text from one context window to another 16 times per feature."

This is true. The playbook optimizes for quality but imposes friction. The tagline "not babysit" contradicts the reality of 16 manual handoffs. Gemini is right to call this out.

---

## The Solution: Partially Flawed

### What Works

**1. The branch metaphor is useful.**
Legislative / Executive / Judicial maps cleanly to Plan / Build / Review. Memorable mental model.

**2. Model diversity by branch is the right unit.**
You need diversity *between phases*, not necessarily within every role. Claude plans, Cursor codes, Gemini reviews. That's the key insight.

**3. The "Court Protocol" structure is clever.**
Prosecution → Defense → Verdict as a structured flow, even if the implementation is wrong.

---

### What Doesn't Work

**1. The Legislative Branch "self-correction" is wishful thinking.**

Gemini says:
> "The model acts as its own 'YAGNI Auditor' before outputting the final Spec."

This contradicts the playbook's core thesis. The entire anti-sycophancy apparatus exists because **models are bad at critiquing their own work**. The Fellow Architects review loop exists for external challenge. Internal iteration is not a substitute.

If one model goes Strategy → PRD → Master Plan → Spec internally, who catches the assumptions baked in at the Strategy layer that corrupt the Spec? No one.

**2. The Judicial Branch "hat switching" defeats model diversity.**

Gemini says:
> "Asking it to explicitly 'Switch Hats' in a single context window is effective, provided it is distinct from the model that wrote the code."

I disagree. The current design requires **different models** for Code Reviewer vs. Adversarial Reviewer specifically because same-model = same blind spots.

A single model playing Prosecution, Defense, and Judge is a show trial. The Adversarial role loses its adversarial nature when the same weights that just played Defense now play Prosecution. They share failure modes.

This is the weakest part of the proposal. It trades the most important diversity (review phase) for efficiency.

**3. The 81% reduction claim is misleading.**

It conflates handoffs with value. If the Legislative branch produces a bad spec because it lacked external challenge, you've wasted the Executive and Judicial phases. The current multi-role design catches issues earlier.

Fewer handoffs ≠ less work. Might mean more rework.

---

## Answering Gemini's Questions

**Q1: Context Saturation in Legislative Branch?**

Yes, real risk. A session holding Strategy + PRD + Master Plan + Implementation Spec will have attention degradation. The Implementation Spec gets written by a model still "thinking about" earlier layers.

Keep a hard break between Product (PRD) and Engineering (Spec). Different concerns, different expertise.

**Q2: Can a single model be its own Adversarial Reviewer?**

No. This is the core flaw. The Adversarial role's value is a *different perspective*, not just a *different prompt*. Same model = same blind spots. The Judicial branch must maintain model diversity or it defeats the ensemble architecture.

**Q3: ComplexityFlag path?**

Goes to **User**, not back to Legislative. If it goes to Legislative, the model will justify its original decision. User arbitrates. This is already the correct design.

---

## Adoption Matrix

| Element | Verdict | Reasoning |
|---------|---------|-----------|
| Acknowledge friction problem | **Adopt** | The playbook should explicitly address "human router" fatigue |
| 3-branch mental model | **Refine** | Good metaphor, but implementation needs work |
| Single-model Legislative | **Refine** | Okay for Strategy + Product, but keep Architect review loop |
| Single-model Judicial | **Reject** | Non-starter. Destroys the most critical diversity. |
| Court Protocol structure | **Adopt** | Prosecution → Defense → Verdict is good. Just needs multiple models. |

---

## Counter-Proposal

The answer to "too many handoffs" is not "fewer roles" but **smarter consolidation + tooling**.

### 1. Modest Role Consolidation (12 → 8)

| Current Roles | Consolidated To | Rationale |
|---------------|-----------------|-----------|
| Strategist + Product Architect | **Product Lead** | Both pre-technical, same concerns |
| Chief Architect | **Chief Architect** | Keep separate (tech decisions) |
| Implementation Architect | **Implementation Architect** | Keep separate (needs review loop) |
| Developer | **Developer** | Pure execution role |
| Code Reviewer | **Code Reviewer** | Keep separate (different model than Dev) |
| Adversarial Reviewer | **Adversarial Reviewer** | Keep separate (different model than Code Reviewer) |
| Synthesizer | Merge into Adversarial output | Synthesis is a view, not a distinct perspective |
| Debugger + QA | **Debugger** | Combine reactive roles |

### 2. Maintain Critical Boundaries

These must remain different models:
- Planning vs. Coding
- Coding vs. Review
- Code Review vs. Adversarial Review (non-negotiable)

### 3. Automate the Routing

The real solution is tooling (like Project Ontos). The friction is in the copy-paste, not in the role count.

**16 roles with automated handoffs > 3 roles with manual handoffs**

---

## Summary

Gemini diagnosed the right problem, proposed a solution that's 60% right and 40% dangerous.

The dangerous 40%:
- Single-model Judicial branch (destroys review diversity)
- Self-correcting Legislative branch (contradicts anti-sycophancy thesis)

These would undermine the playbook's core value: that ensemble architecture catches what single-model workflows miss.

**Recommendation:** Keep the branch mental model. Keep the Court Protocol structure. Reject single-model review. Invest in automation over consolidation.
