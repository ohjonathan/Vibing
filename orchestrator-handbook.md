# Orchestrator Handbook
> Your reference guide for running an LLM-augmented development workflow
> Last updated: December 2024

---

## How to Use This Document

**Audience:** This document is for you, the human orchestrator. For LLM prompts and role definitions, see [llm-development-playbook.md](llm-development-playbook.md).

**To generate prompts:** Give llm-development-playbook.md to an LLM along with your variables (see Quick-Start below).

**Context Persistence:** This playbook assumes [Project Ontos](https://github.com/ohjona/Project-Ontos) handles context sync between agents.

---

## Quick-Start: The Generator Prompt

**Step 1:** Open [session-variables.md](session-variables.md) and fill in your values for this session.

**Step 2:** Give an LLM these three things:
1. [llm-development-playbook.md](llm-development-playbook.md)
2. [session-variables.md](session-variables.md) (with your values filled in)
3. The Generator Prompt below

**Step 3:** Get all your prompts generated at once.

### Generator Prompt

\`\`\`
I'm starting a work session. 

My session variables are in the attached session-variables.md file.

Based on the LLM Development Playbook, generate ready-to-paste prompts for:

**Spec Convergence Phase:**
1. Implementation Architect (writing the spec)
2. Implementation Plan Review (for fellow architects - initial)
3. Implementation Architect Response to Review
4. Fellow Architects Re-Review
5. Implementation Architect Iteration (for round 2+)
6. Spec Finalization (after convergence)

**Development Phase:**
7. Development Initiation (kick off coding)

**Code Review Phase:**
8. Code Review (for reviewers - initial)
9. Adversarial Review
10. Developer Response to Review
11. Code Reviewer Re-Review
12. Developer Iteration (for round 2+)

**Synthesis & Post-Ship:**
13. Synthesizer
14. Synthesis Cross-Review
15. Chief Architect Reality Sync (if version just shipped)

**Utility:**
16. Context Refresh (when sensing context drift)

Substitute my variable values from session-variables.md. Output each prompt in a code block I can copy.
\`\`\`

---

## Philosophy

You are the **Product Owner**, **Decision Maker**, and **Manual Orchestrator**. LLMs are your team:
- They propose, you dispose
- They execute, you validate direction
- They review each other, you break ties
- **You trigger every handoff**—nothing moves without you asking the next agent to work

Your competitive advantage isn't coding—it's judgment, taste, and knowing what to build.

### Core Principle: Simplicity Over Cleverness

This workflow enforces **engineering constraints** (YAGNI, DRY, KISS) at every stage:
- Product Architect must produce a "Cut List" of deferred features
- Chief Architect must justify any non-boring technology
- Implementation Architect designs for THIS version only
- Developer can REFUSE to implement over-engineered specs
- Adversarial Reviewer hunts complexity with the same rigor as bugs

Complexity is treated as a liability, not an asset.

### Your Operating Model
\`\`\`
┌─────────────────────────────────────────────────────────┐
│                         YOU                              │
│  ┌─────────────────────────────────────────────────┐    │
│  │ • Write strategy & product vision               │    │
│  │ • Trigger each LLM to do their job              │    │
│  │ • Review synthesized outputs (not raw code)     │    │
│  │ • Make go/no-go decisions at each gate          │    │
│  │ • Direct debugging when things break            │    │
│  └─────────────────────────────────────────────────┘    │
│                           │                              │
│                           ▼                              │
│    ┌──────────┬──────────┬──────────┬──────────┐        │
│    │Strategist│ Architect│ Developer│ Reviewer │  ...   │
│    └──────────┴──────────┴──────────┴──────────┘        │
└─────────────────────────────────────────────────────────┘
\`\`\`

You don't read code. You read summaries, make decisions, and approve merges.

---

## Role Monitoring Guide

For each role, here's what you watch for and when to intervene.

### Strategist
**Your job:** Ensure output matches your vision, cut scope creep

### Product Architect
**Your job:** Ensure PRD matches your vision, verify Cut List exists and makes sense
**Watch for:** Cutting essential features just to satisfy the constraint — Cut List can be empty if justified

### Chief Architect
**Your job:** Sanity check complexity, approve phase boundaries, verify "boring tech" defaults
**Watch for:** Unjustified complexity — if there's no Complexity Tradeoff section for non-standard tech, send back
**Ongoing:** After each version ships, trigger Reality Sync to keep Master Plan aligned with reality

### Implementation Architect
**Your job:** Decide on flagged tradeoffs; approve when all architects converge
**Watch for:** Speculative design ("this will allow us to easily...") — that's YAGNI violation

### Developer (Agentic)
**Your job:**
- Monitor for stuck states (same error 3+ times)
- Watch for scope creep (changes not in spec)
- Intervene if agent asks a question and waits indefinitely
- Verify PR matches spec before requesting review
- **Handle ComplexityFlags when raised**

**Red Flags:**
- Developer says "I'll also improve/refactor X" → STOP, that's scope creep
- Developer asks "Should I do X or Y?" → Spec was ambiguous; clarify before continuing
- Developer commits 10+ files when spec mentioned 3 → Review diff before proceeding
- Developer has been running 30+ minutes with no output → May be stuck

**Intervention phrase:** "Stop. Show me what you've done so far. Let's verify we're on track before continuing."

**Handling ComplexityFlags:**
The Developer has authority to REFUSE implementation if the spec violates engineering principles. When you see a ComplexityFlag:

\`\`\`
⚠️ COMPLEXITY FLAG
I cannot implement [X] as specified because it violates [YAGNI/DRY/KISS]:
- Spec asks for: [what]
- Problem: [why over-engineered]
- Suggestion: [simpler alternative]
\`\`\`

**Your response options:**
1. **Accept the flag:** Update spec to use simpler alternative, then continue
2. **Override with justification:** Explain why the complexity IS needed (cite specific PRD requirement)
3. **Escalate:** If unclear, ask Implementation Architect to clarify intent

ComplexityFlags are quality control, not insubordination. Take them seriously.

### Code Reviewer
**Your job:** Read synthesis, break ties, final merge decision

### Adversarial Reviewer
**Your job:** If they find real issues, block merge. If they provide vague approval, send back for more detail.

**Expanded Mandate:** Adversarial Reviewer now hunts for **complexity AND bugs**, with complexity audited FIRST.

**Invalid outputs (send back):**
- "Looks good, I couldn't find issues" — Too vague
- "I tried hard to break it" — What specifically?
- Short review (<300 words) — Not thorough
- No complexity audit section — Must check for bloat
- Approves complexity without justification — Must explain why it's necessary

### Synthesizer
**Your job:** Read the synthesis, make decisions. If synthesizers disagree, dig into raw reviews.

**New rule:** Synthesizer "sides with the pessimist" — if Code Reviewer approves but Adversarial blocks for complexity, default is BLOCK.

### Debugger
**Your job:** Direct which errors to prioritize, approve fix approach

### QA Analyst
**Your job:** Prioritize which bugs matter

---

## Quality Gates

### Before Phase 3 (Implementation Planning)
- [ ] Strategy approved by you
- [ ] PRD reviewed and scope locked
- [ ] PRD includes Cut List (or explicit "Scope is Minimal" statement)
- [ ] Master plan phase boundaries make sense
- [ ] Master plan uses boring tech (or has Complexity Tradeoffs)

### Before Phase 4 (Development)
- [ ] Implementation spec passed Gate Check (Reality + Simplicity)
- [ ] Implementation spec has Assumptions section
- [ ] All architects have converged (not just stopped arguing)
- [ ] You've decided on all flagged tradeoffs
- [ ] Spec is detailed enough that Developer won't need judgment calls
- [ ] No speculative "future flexibility" in the spec

### Before Merge
- [ ] Code Reviewer approved with spec citations
- [ ] Adversarial Reviewer completed complexity audit FIRST
- [ ] Adversarial Reviewer listed specific attack vectors tried
- [ ] Adversarial Reviewer response is 300+ words
- [ ] Synthesizer flagged any shallow reviews
- [ ] No unresolved conflicts between reviewers
- [ ] If Adversarial flagged bloat, it's resolved or you've explicitly accepted it
- [ ] You've read synthesis and made decision

### Before Shipping Version
- [ ] Acceptance criteria validated by QA
- [ ] Critical bugs fixed
- [ ] You've actually used it yourself

---

## Emergency Procedures

### E1: Developer Stuck in Loop
**Protocol:**
1. STOP the agent immediately
2. Read the last 3 attempts yourself
3. Identify what assumption is wrong
4. Restart with explicit constraint: "Do NOT try [X] again. The issue is [Y]. Try [Z] instead."

### E2: Post-Merge Bug Discovered
**Protocol:**
1. REVERT immediately. Do not debug on main.
2. Create hotfix branch from last known good commit
3. Fix → Single focused review → Merge
4. Add Post-Mortem to session log

### E3: Security Vulnerability Found
**Protocol:**
1. Do NOT discuss in public PR comments or issues
2. Assess severity: Critical/High/Medium/Low
3. After fix: Document in private security log

### E4: Agentic Developer Goes Rogue
**Protocol:**
1. STOP the agent
2. git diff to see actual changes vs. expected
3. git checkout or git revert as needed
4. Restart with explicit constraint

### E5: Context Collapse
**Protocol:**
1. Do NOT continue
2. Use the Context Refresh prompt
3. Correct or start fresh session

### E6: Reviewers All Approve Too Easily
**Protocol:**
1. Do NOT merge
2. Ask specific questions about what they reviewed
3. Add new reviewer with "find something wrong" instruction

### E7: Developer Raises ComplexityFlag
**Protocol:**
1. READ the flag carefully
2. Evaluate: Is the simpler alternative viable?
3. Accept / Override / Escalate
4. Do NOT ignore

---

## Escalation Matrix

### Architect Disagreements
| Scenario | Resolution |
|----------|------------|
| 2 agree, 1 disagrees | Dissenter provides alternative; stronger wins |
| All 3 disagree | You decide after asking each for core concern |
| Simplicity audit fails | Must cite SPECIFIC PRD requirement or simplify |

### Review Disagreements
| Scenario | Resolution |
|----------|------------|
| Code Reviewer approves, Adversarial blocks | **Adversarial wins by default** |
| Adversarial flags bloat, others approve | **Side with pessimist** — default is BLOCK |

### Convergence Stalls
| Scenario | Resolution |
|----------|------------|
| 5 rounds, still debating | **Mandatory scope split** |
| Exhaustion ≠ agreement | Require: "I agree this is better because [X]" |

---

## Failure Modes Catalog

| Failure Mode | Prevention |
|--------------|------------|
| **Spec Drift** | Require spec-citation in code review |
| **Review Theater** | Require specific line citations; use Adversarial |
| **Scope Creep** | Not in spec = not in PR |
| **Sycophancy Cascade** | Use different models |
| **Complexity Debt** | Gate Check, ComplexityFlag, Adversarial audit |

---

## Quick Reference

### Anti-Sycophancy Triggers
- "What would YOU have done differently?"
- "Ultrathink" / "Think step by step" / "Take your time"
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

### Quality Gates Checklist
- [ ] PRD has Cut List
- [ ] Spec passed Gate Check
- [ ] Adversarial completed complexity audit FIRST
- [ ] Adversarial review is 300+ words
- [ ] No unresolved conflicts

---

*This document is for you. To generate prompts, give an LLM llm-development-playbook.md and session-variables.md with your values filled in.*
