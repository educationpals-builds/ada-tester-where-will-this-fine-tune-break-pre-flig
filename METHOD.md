# The BREAK Framework

## Framework Name

**BREAK** — Baseline, Rate, Embedding, Attention, Knowledge

A five-clause examination framework for pre-flight auditing of fine-tuning configurations.

---

## The Letters

### B — Baseline Stability

Verify the foundation before building on it.

**Examines:**
- Pretrained weight integrity
- Checkpoint loading correctness
- Forward pass consistency
- Initialization state

**Failure mode if skipped:** Training begins from corrupted or randomly reinitialized weights, wasting entire compute budget.

---

### R — Rate and Schedule

Confirm learning dynamics are appropriate for fine-tuning.

**Examines:**
- Learning rate magnitude
- Warmup configuration
- Decay schedule
- Effective batch size

**Failure mode if skipped:** Catastrophic forgetting from high LR, or no learning from LR too low. Training instability from missing warmup.

---

### E — Embedding and Normalization

Ensure architectural assumptions match the base model.

**Examines:**
- Normalization layer placement
- Epsilon values
- Embedding layer training state
- Norm type consistency

**Failure mode if skipped:** Subtle distribution shift causing degraded generation quality. Silent accuracy loss that only appears in evaluation.

---

### A — Attention and Memory

Validate attention mechanics and memory management.

**Examines:**
- Attention mask correctness
- Memory-efficient attention configuration
- Gradient checkpointing
- KV-cache state

**Failure mode if skipped:** OOM errors mid-training. Incorrect causal masking causing information leakage. Silent memory corruption.

---

### K — Knowledge Preservation

Protect existing capabilities while adding new ones.

**Examines:**
- Catastrophic forgetting mitigations
- Adapter/LoRA configuration
- Layer freezing strategy
- Regularization settings

**Failure mode if skipped:** Model gains new capability but loses general knowledge. Fine-tuned model worse than base on standard benchmarks.

---

## Severity Levels

| Level | Definition | Action |
|-------|------------|--------|
| **Critical** | Will cause training failure or total compute waste | Must fix before launch |
| **Warning** | May cause degraded results or partial failure | Should fix; document if accepting risk |
| **Note** | Suboptimal but functional | Consider fixing; no launch block |

---

## Verdict Definitions

**CLEAR:** All clauses pass or have only Notes. Proceed to training.

**CONDITIONAL:** One or more Warnings present. May proceed after documenting acceptance rationale and configuring tripwires.

**HOLD:** One or more Critical findings. Do not proceed until resolved.

---

## Application Order

Always examine clauses in B-R-E-A-K order. Earlier clauses can invalidate later ones:

1. If Baseline fails, Rate findings are meaningless (wrong weights)
2. If Rate fails critically, Embedding examination may be moot (training won't converge)
3. Attention and Knowledge assume earlier clauses pass

---

## Framework Principles

1. **Cite evidence:** Every finding must quote specific lines or keys
2. **Earn clears:** Passing requires stating what was verified, not just absence of findings
3. **Compound risk:** Multiple Warnings may aggregate to effective Critical
4. **Tripwire everything:** Even CLEAR verdicts should have monitoring

---

*BREAK Framework version: 1.0*