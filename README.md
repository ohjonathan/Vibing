# LLM Development Playbook

A multi-agent review workflow for LLM-augmented software development. Uses intentional model diversity and role differentiation to catch issues that single-model workflows miss.

## Files

| File | Purpose |
|------|---------|
| `llm-development-playbook.md` | Source of truth — all principles, phases, roles, and protocols |
| `session-variables.md` | Project and release variables template (fill in before starting) |

## How It Works

1. **Fill in** `session-variables.md` with your project and release details.
2. The playbook defines a **four-phase workflow**: Spec Development (A) → Spec Review (B) → Implementation (C) → Code Review (D).
3. Each review phase uses **three specialized reviewers** (Peer, Alignment, Adversarial) running in parallel across different model families.
4. The **Chief Architect** (human-directed) makes decisions at phase gates.
5. **Prompts are generated fresh** per phase transition — self-contained, not reused from templates.

### Workflow

```
PHASE A: Spec Development          PHASE B: Spec Review
  CA writes spec v1.0                B.1: Review Board (3 models, parallel)
         │                           B.2: Consolidation
         ▼                           B.3: CA Response → Spec v1.1
  Submit for review ──────────▶      B.4: Verification (if needed)
                                            │
                    ┌───────────────────────┘
                    ▼
PHASE C: Implementation             PHASE D: Code Review
  C.1: CA writes impl prompt         D.1: CA PR Review
  C.2: Developer implements          D.2: Review Board (3 models, parallel)
  C.3: PR created ──────────────▶    D.3: Consolidation
                                     D.4: Developer fixes (if needed)
                                     D.5: Adversarial verification
                                     D.6: CA approval → MERGE
```

## Key Concepts

| Concept | Summary | Reference |
|---------|---------|-----------|
| **Model Diversity** | Different AI models for different roles — prevents shared blind spots | §2.1 |
| **One-Revision Cap** | Two rounds max per review cycle. If it doesn't converge, re-scope. | §2.2 |
| **Role Differentiation** | Peer (quality), Alignment (compliance), Adversarial (breaking) — three distinct lenses | §2.3 |
| **Document-Driven Review** | Reviews anchored to approved docs, not author framing | §2.4 |
| **Fresh Prompt Generation** | Every prompt is self-contained and generated for the current step | §2.7 |
| **Context Window Management** | Deliberate budgeting of context to keep models in the attention sweet spot | §7.8 |
