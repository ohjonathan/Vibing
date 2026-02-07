# LLM Development Playbook v3.0: Battle-Tested Mandates

**Based on:** Comprehensive analysis of 8 conversations spanning Ontos v3.0.4 through v3.2
**Purpose:** Codify operational patterns that emerged from actual execution into formal playbook mandates
**Action:** Each mandate includes the exact new section content for integration into the playbook

---

## Evidence Base

| Release | Type | Key Patterns Observed |
|---------|------|----------------------|
| v3.0.4 | Patch | Spec deviation handling, CA decision framework |
| v3.0.5 | Patch + Triage | Pre-Phase A triage, factual misunderstanding vs. priority disagreement |
| v3.1.0 | Major (Track-based) | Track splitting, verification rounds, split verdict resolution, one-revision cap enforcement |
| v3.1.1 | Feature | Type hierarchy integration, scaffold patterns |
| v3.2 | Multi-topic | Proposal review cycle, prompt suite delivery, branch/PR reference corrections, environment detection |

All mandates below were observed **multiple times** across different releases. Where the user corrected deviations from these patterns, those corrections confirm they are **requirements, not preferences**.

---

## Mandate 1: Fresh Prompt Generation Principle

**Priority:** HIGH
**Evidence:** Every conversation (v3.0.4-v3.2). User consistently requested fresh prompts at each phase transition. Never said "use the prompt I generated earlier." In v3.1.0, user said "what's next?" expecting fresh D.1b generation, not a reference to previous output.
**Current gap:** No explicit principle in Section 5.

### Proposed Addition: Section 5.4 — Fresh Prompt Generation

```markdown
### 5.4 Fresh Prompt Generation

**Every prompt is generated fresh for the current step.** Never reference prompts from previous conversation turns or prior sessions.

| ❌ Don't | ✅ Do |
|----------|-------|
| "Use the Phase B prompt I generated earlier" | Generate a new Phase B prompt with current context |
| "Same as last time but change the PR number" | Full prompt with correct PR number embedded |
| "Refer to the implementation prompt above" | Complete, self-contained prompt |

**Why this matters:**
- Context windows compact or reset between sessions
- PR numbers, branch names, and file states change
- Self-contained prompts prevent stale references
- Each model instance starts fresh — give it everything it needs

**Rule:** If you can't copy-paste a prompt into a fresh model session and have it work, the prompt is incomplete.
```

---

## Mandate 2: Prompt Delivery Standards

**Priority:** HIGH
**Evidence:** v3.0.5: User explicitly requested "all prompts be delivered as separate markdown files for easy copy-pasting rather than displayed in chat." v3.1.0: User requested D.1b-D.6b ready upfront. v3.2: User requested complete Phase B suite as files.
**Current gap:** Section 10 covers prompt engineering content but not delivery format.

### Proposed Addition: Section 10.6 — Prompt Delivery Standards

```markdown
### 10.6 Prompt Delivery Standards

**Every prompt is delivered as a separate file.** Not inline in chat. Not in code blocks. Actual `.md` files the orchestrator can copy-paste into each model.

**Delivery method:**
1. Generate each prompt as a standalone `.md` file
2. Use `create_file` (or equivalent) — not inline code blocks
3. Name files using convention (see Section 12)
4. Present all files at end for easy access

**File naming for prompts:**
```
[Version]_[Phase]_[Step]_[Role/Model].md
```

**Examples:**
- `v3.2_PhaseB_B1_Peer_Gemini.md`
- `v3.2_PhaseB_B1_Alignment_Claude.md`
- `v3.2_PhaseB_B1_Adversarial_Codex.md`
- `v3.2_PhaseB_B2_Consolidation.md`

**Why files, not chat:**
- Orchestrator copies each prompt to its target model
- Chat messages get lost in conversation history
- Files persist, can be referenced, version-controlled
- Separate files enforce self-containment (Mandate 1)

**Self-containment check:** Each file must include:
- [ ] Role assignment
- [ ] All reference document paths
- [ ] Complete review template/framework
- [ ] Output file naming instructions
- [ ] Critical instructions section
```

---

## Mandate 3: Prompt Suite Delivery

**Priority:** HIGH
**Evidence:** v3.1.0: User preferred having D.1b-D.6b ready upfront when Track B PR was created. v3.2: User requested complete Phase B suite upfront. Pattern: User consistently chooses full suite over step-by-step.
**Current gap:** No guidance on when to generate individual prompts vs. full phase suites.

### Proposed Addition: Section 10.7 — Prompt Suite Delivery

```markdown
### 10.7 Prompt Suite Delivery

When entering a new phase, offer two delivery options:

| Option | When Best | What's Delivered |
|--------|-----------|-----------------|
| **Next step only** | Uncertain about approach, need to iterate | Single prompt file |
| **Full phase suite** | Approach is settled, ready to execute | All prompts for remaining steps |

**In practice, the orchestrator almost always wants the full suite.** Default to offering it.

**Full suite contents by phase:**

| Phase | Suite Contains |
|-------|---------------|
| B (Spec Review) | B.1 Peer, B.1 Alignment, B.1 Adversarial, B.2 Consolidation |
| C (Implementation) | C.1 Implementation Prompt |
| D (Code Review) | D.1 CA PR Review, D.2 Peer, D.2 Alignment, D.2 Adversarial, D.3 Consolidation, D.4 Fix Summary template, D.5 Verification, D.6 Final Approval |

**Suite generation rules:**
1. All prompts reference the same branch name and PR number
2. All prompts reference the same spec version
3. Generate in workflow order
4. Deliver as separate files (per Mandate 2)
5. Include a manifest file listing the suite contents and execution order
```

---

## Mandate 4: Pre-Phase A — Triage & Validation Workflow

**Priority:** HIGH
**Evidence:** v3.0.5: Full triage cycle executed with CA categorization, Review Board validation, consolidation distinguishing "CA misunderstood the finding" (blocking) from "Reviewer disagrees with priority" (not blocking). v3.2: Multi-topic triage across support protocol, environment detection, and ontos activation.
**Current gap:** Section 14 covers Strategic Analysis and Technical Architecture but has no operational pre-phase workflows.

### Proposed Addition: Section 14.3 — Pre-Phase A: Triage & Validation

```markdown
### 14.3 Pre-Phase A: Triage & Validation

**When to use:** After an audit, bug report, or discovery phase surfaces multiple items that need categorization before spec development begins.

```
Pre-A.1: Chief Architect Triage
           │
           ▼
Pre-A.2: Review Board Validation (Parallel)
     ├── Peer Reviewer
     ├── Alignment Reviewer
     └── Adversarial Reviewer
           │
           ▼
Pre-A.3: Consolidation
           │
           ▼
Pre-A.4: CA Response (if challenges exist)
           │
           ▼
     Approved scope → Phase A
```

#### Pre-A.1: Chief Architect Triage

CA categorizes each finding into:

| Category | Meaning | Action |
|----------|---------|--------|
| **In Scope** | Will be addressed in this release | Proceed to Phase A |
| **Deferred** | Valid but not now | Add to backlog with rationale |
| **Rejected** | Not a real issue or already handled | Document reasoning |

#### Pre-A.2: Review Board Validation

Reviewers challenge triage decisions. Focus areas:

| Reviewer | Focus |
|----------|-------|
| Peer | Are deferred items actually deferrable? Are rejected items truly handled? |
| Alignment | Do scope decisions match roadmap priorities? |
| Adversarial | What's the risk of deferring each deferred item? |

#### Pre-A.3: Consolidation

**Critical distinction:**

| Challenge Type | Blocking? | Resolution |
|----------------|-----------|------------|
| **CA misunderstood the finding** | YES | CA must re-evaluate with correct understanding |
| **Reviewer disagrees with priority** | NO | Note dissent, CA decision stands |
| **Factual error in triage rationale** | YES | CA must correct facts and re-evaluate |
| **Reviewer wants broader scope** | NO | CA decides scope boundaries |

Only blocking challenges require CA response. Priority disagreements are noted but do not reopen decisions.

#### Pre-A.4: CA Response

Address ONLY blocking challenges. For each:
1. Acknowledge the misunderstanding or factual error
2. Re-evaluate with correct information
3. Update triage decision if warranted
4. Document final decision with reasoning
```

---

## Mandate 5: Pre-Phase A — Proposal Review Cycle

**Priority:** MEDIUM
**Evidence:** v3.2: Full proposal review cycle executed for three-topic release. Distinct from spec review — less formal, focused on approach/philosophy, may iterate.
**Current gap:** No documented workflow for reviewing proposals before they become specs.

### Proposed Addition: Section 14.4 — Pre-Phase A: Proposal Review

```markdown
### 14.4 Pre-Phase A: Proposal Review

**When to use:** Developing new approaches, evaluating design directions, or scoping multi-feature releases before committing to specs.

**How it differs from spec review:**

| Aspect | Proposal Review | Spec Review (Phase B) |
|--------|----------------|----------------------|
| Formality | Lighter structure | Full structured templates |
| Focus | Approach, philosophy, feasibility | Implementation correctness |
| Iteration | May iterate multiple times | One revision cap |
| Output | Refined proposal → feeds Phase A | Approved spec v1.1 → feeds Phase C |

**Workflow:**
```
Proposal Development
        │
        ▼
Review Board reviews proposal
        │
        ▼
Consolidation
        │
        ▼
CA responds to findings
        │
        ▼
Review Board validates response (optional)
        │
        ▼
Finalized proposal → Phase A
```

**Key principle:** Proposal reviews are exploratory. The one-revision cap does NOT apply here. However, if a proposal requires more than 3 review rounds, the scope is likely too broad — split it.
```

---

## Mandate 6: Verification Round Protocols & Split Verdict Resolution

**Priority:** HIGH
**Evidence:** v3.1.0 Track A: Gemini approved, Codex flagged issues. Required structured resolution of split verdicts. CA had to verify facts, assess positions, and make final ruling. This workflow is not documented — B.4 currently has only 4 lines.
**Current gap:** Section 7.6 (B.4) is a stub. No B.5 or B.6 exists.

### Proposed Addition: Expand Section 7.6 and add 7.7-7.8

```markdown
### 7.6 B.4: Verification Review (If Needed)

**When to use:** CA made significant changes in B.3 (rejected recommendations, changed scope, resolved open questions differently than recommended).

**Who participates:** Only reviewers who flagged blocking issues in B.1. If all three flagged blocking issues, all three verify. If only Codex did, only Codex verifies.

**Format:** Brief (~1 page per reviewer). Focus on:

| Check | Question |
|-------|----------|
| Fix adequacy | Did CA's changes actually address my concern? |
| New issues | Did CA's changes introduce new problems? |
| Readiness | Is this now ready for implementation? |

**Output:** `[Phase]_Spec_Verification_[Model].md`

**Verdicts allowed:**
- **Accept** — Issue resolved, ready for implementation
- **Accept with Note** — Resolved but reviewer wants concern on record
- **Challenge** — Issue NOT resolved, requires further CA attention

### 7.7 B.5: Verification Consolidation (If Split Verdict)

**When to use:** B.4 produces a split verdict (some Accept, some Challenge).

**Process:**
1. Synthesize all B.4 verdicts
2. For each Challenge: identify whether it's a factual disagreement or priority disagreement
3. Present decision-ready summary to CA

**Consolidation must distinguish:**

| Disagreement Type | Meaning | CA Action |
|-------------------|---------|-----------|
| **Factual** | Reviewer says CA got facts wrong | CA must verify facts and respond |
| **Priority** | Reviewer disagrees with CA's weighting | CA may override with documented reasoning |
| **Scope** | Reviewer wants broader/narrower scope | CA decides, documents rationale |

### 7.8 B.6: Final CA Decision (If Split Verdict)

**Process:**
1. Verify contested facts independently
2. Assess each position on its merits
3. Make ruling — no fence-sitting
4. Document dissenting position for the record
5. Update spec if warranted

**Decision framework:**

| Situation | Action |
|-----------|--------|
| Reviewer is factually correct, CA was wrong | Accept, update spec |
| Reviewer is factually wrong | Override with evidence |
| Legitimate priority disagreement | CA decides, documents dissent |
| Consensus impossible after 2 rounds | Scope problem — escalate to re-spec |

**After B.6:** Spec is approved. No further review rounds. If fundamental disagreements persist, the problem is scope or requirements, not the spec — return to Pre-A or Phase A.
```

---

## Mandate 7: One-Revision Cap Decision Matrix

**Priority:** HIGH
**Evidence:** v3.1.0 Track A, Phase D: After Round 2, Codex flagged missing edge case tests (minor severity). CA used explicit decision framework: Issue Type × Severity → Action. Chose to add tests (Option A) over overriding (Option B). This matrix was improvised — should be formalized.
**Current gap:** Section 2.2 describes the principle but provides no actionable decision framework.

### Proposed Addition: Section 2.2.1 — One-Revision Cap Decision Matrix

```markdown
#### 2.2.1 Decision Matrix After Round 2

When Round 2 (verification) surfaces remaining or new issues, use this matrix:

| Issue Type | Critical Severity | Major Severity | Minor Severity |
|------------|-------------------|----------------|----------------|
| **Factual error by reviewer** | Override with evidence | Override with evidence | Override with evidence |
| **Legitimate bug/gap found** | Fix (back to D.4) | Fix (back to D.4) | CA decides: fix or defer |
| **Style/preference disagreement** | Note and proceed | Note and proceed | Note and proceed |
| **Scope expansion request** | Reject — out of scope | Reject — out of scope | Reject — out of scope |
| **Missing tests for existing code** | Fix | CA decides | Defer to next release |
| **Missing tests for new code** | Fix | Fix | CA decides |

**"CA decides" criteria:**
- Core tests pass? If yes, lean toward proceed
- Is this blocking user-facing functionality? If no, lean toward proceed
- Is the fix < 30 minutes? If yes, lean toward fix
- Does deferring create technical debt? If yes, lean toward fix

**Escalation:** If after applying this matrix, the issue STILL isn't resolved, the problem is scope or requirements. Return to Phase A. Do NOT start Round 3.
```

---

## Mandate 8: Spec Deviation Protocol

**Priority:** MEDIUM
**Evidence:** v3.0.4: Antigravity added `package_root` to PYTHONPATH during implementation — not in spec. Required explicit CA decision. Two clear options with criteria. This pattern will recur any time implementation discovers spec gaps.
**Current gap:** Section 9 covers code review but has no protocol for when implementation diverges from spec.

### Proposed Addition: Section 9.10 — Spec Deviation Protocol

```markdown
### 9.10 Spec Deviation Protocol

**When triggered:** Code review discovers implementation diverges from approved spec v1.1.

**Two options only — no middle ground:**

| Option | When to Use | Actions |
|--------|-------------|---------|
| **A: Approve Deviation** | Deviation fixes a real problem the spec missed | 1. Document deviation rationale. 2. Update spec to v1.2. 3. Continue review against v1.2. |
| **B: Require Revert** | Deviation is unnecessary, cargo-culted, or introduces risk | 1. Flag in code review. 2. Antigravity reverts to match spec. 3. Continue review against v1.1. |

**Decision criteria:**

| Question | Leans Toward |
|----------|-------------|
| Does the deviation fix a problem the spec didn't anticipate? | Option A |
| Is the deviation necessary or could the spec's approach work? | Necessary → A, Not necessary → B |
| What's the risk of reverting? | High risk → A, Low risk → B |
| Was the spec thoroughly reviewed? | Yes → B (spec was probably right), No → A |
| Does the deviation change public API or user-facing behavior? | Yes → extra scrutiny, likely B unless critical bug |

**Who decides:** Chief Architect. Decision is final and must be documented in PR comments.

**If Option A is chosen:** All subsequent review steps (D.2-D.6) reference spec v1.2, not v1.1. Consolidation must note the version change.
```

---

## Mandate 9: Track-Based Implementation

**Priority:** MEDIUM
**Evidence:** v3.1.0: User explicitly chose track-based over monolithic for "context management perspective." Track A (Obsidian + token efficiency) and Track B (7 command migrations) reviewed independently. Benefits validated: focused context windows, independent verification, cleaner git history, failure isolation.
**Current gap:** Section 13 has Feature, Patch, and Hotfix variants but no track-based variant.

### Proposed Addition: Section 13.4 — Track-Based Implementation

```markdown
### 13.4 Track-Based Implementation

**When to use:** Release has multiple conceptually distinct work streams that can be developed and reviewed independently.

**Triggers:**
- >10 files changed across distinct subsystems
- 2+ features with minimal file overlap
- Context window too small for monolithic review
- Independent verification is valuable (one track can merge while another iterates)

**Structure:**

```
Phase A → Phase B → Approved Spec v1.1
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
         Track A                 Track B
     C.1a → C.2a → C.3a     C.1b → C.2b → C.3b
     D.1a → D.6a → Merge    D.1b → D.6b → Merge
```

**Rules:**
1. Single spec covers all tracks (approved together in Phase B)
2. Each track gets its own C and D cycle
3. Tracks execute sequentially (not in parallel) to avoid merge conflicts
4. Track A merges before Track B begins implementation
5. Track B's implementation prompt must reference Track A's merged state

**File naming:**
```
[Phase]_Track_[Letter]_[Step]_[Role].md
```

**Examples:**
- `Phase3_Track_A_Implementation_Prompt_Antigravity.md`
- `Phase3_Track_B_Code_Review_Codex.md`

**Benefits over monolithic:**
- Focused context windows — each review only sees relevant changes
- Independent verification — Track A issues don't block Track B review
- Cleaner git history — separate PRs per track
- Failure isolation — if Track B fails, Track A is already merged

**When NOT to use:**
- Changes are tightly coupled (files overlap significantly)
- Total scope is small enough for one context window
- Sequential dependency (Track B can't start until Track A's code exists)
```

---

## Mandate 10: Reference Accuracy Protocol

**Priority:** LOW (but prevents rework)
**Evidence:** v3.2 Ontos Activation: User corrected branch from `feature/v3.2-ontos-activation` to `feature/v3.2.0-ontos-activation`. User corrected PR from #59 to #60. Required systematic updates across 8 Phase D prompt files. Avoidable with upfront confirmation.
**Current gap:** No verification step before prompt suite generation.

### Proposed Addition: Section 12.3 — Reference Accuracy Protocol

```markdown
### 12.3 Reference Accuracy Protocol

**Before generating any prompt suite,** confirm the following with the orchestrator:

| Reference | Confirm | Example |
|-----------|---------|---------|
| Branch name | Exact format including version | `feature/v3.2.0-ontos-activation` |
| PR number | Current PR number | `#60` |
| Spec version | Which version is approved | `v1.1` |
| Base branch | What branch PR targets | `main` |

**Why:** A single wrong PR number propagates across 8+ prompt files. Fixing after generation is tedious and error-prone. 30 seconds of confirmation prevents 30 minutes of corrections.

**If references change after generation:**
1. Identify all affected files
2. Update systematically (search-and-replace, not manual)
3. Re-deliver corrected files
4. Confirm corrections with orchestrator
```

---

## Integration Summary

### New Sections Required

| Section | Title | Priority | Mandate |
|---------|-------|----------|---------|
| 2.2.1 | One-Revision Cap Decision Matrix | HIGH | 7 |
| 5.4 | Fresh Prompt Generation | HIGH | 1 |
| 7.6 | B.4: Verification Review (expanded) | HIGH | 6 |
| 7.7 | B.5: Verification Consolidation | HIGH | 6 |
| 7.8 | B.6: Final CA Decision | HIGH | 6 |
| 9.10 | Spec Deviation Protocol | MEDIUM | 8 |
| 10.6 | Prompt Delivery Standards | HIGH | 2 |
| 10.7 | Prompt Suite Delivery | HIGH | 3 |
| 12.3 | Reference Accuracy Protocol | LOW | 10 |
| 13.4 | Track-Based Implementation | MEDIUM | 9 |
| 14.3 | Pre-Phase A: Triage & Validation | HIGH | 4 |
| 14.4 | Pre-Phase A: Proposal Review | MEDIUM | 5 |

### Version Bump

Current: v2.0 (2026-01-15)
Proposed: v3.0 (2026-02-01)

Rationale: 12 new sections addressing operational workflow gaps constitutes a major version change, not a minor update. The playbook's scope expands from "how to run phases A-D" to "how to run the complete development lifecycle including pre-phase workflows, prompt logistics, and decision frameworks."

### Appendix B Update

```markdown
| 3.0 | 2026-02-01 | Added: Fresh prompt generation principle (5.4), Prompt delivery standards (10.6), Prompt suite delivery (10.7), Pre-Phase A triage workflow (14.3), Pre-Phase A proposal review (14.4), Verification round protocols (7.6-7.8), One-revision cap decision matrix (2.2.1), Spec deviation protocol (9.10), Track-based implementation variant (13.4), Reference accuracy protocol (12.3). Battle-tested through Ontos v3.0.4-v3.2. |
```

### Quick Reference Updates (Section 17)

Add to 17.1 Phase Checklist:

```markdown
#### Pre-Phase A: Triage (if applicable)
- [ ] Pre-A.1: CA triages findings (In Scope / Deferred / Rejected)
- [ ] Pre-A.2: Review Board validates triage
- [ ] Pre-A.3: Consolidate (distinguish misunderstandings from disagreements)
- [ ] Pre-A.4: CA responds to blocking challenges only

#### Pre-Phase A: Proposal Review (if applicable)
- [ ] Develop proposal
- [ ] Review Board reviews proposal
- [ ] Consolidate findings
- [ ] CA responds
- [ ] Finalize → proceed to Phase A

#### Prompt Generation (every phase transition)
- [ ] Confirm branch name, PR number, spec version with orchestrator
- [ ] Generate all prompts as separate .md files
- [ ] Each prompt is self-contained (copy-pasteable to fresh session)
- [ ] Deliver full suite unless orchestrator requests step-by-step
```
