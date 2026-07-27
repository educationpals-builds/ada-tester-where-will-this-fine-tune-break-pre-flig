# Pre-Flight Charter: Fine-Tune Audit

## Specimen Under Review

**Model identifier:** [paste model name/path]

**Checkpoint source:** [URL or local path]

**Intended fine-tune task:** [describe downstream task]

**Compute budget:** [GPU-hours allocated]

---

## Standard Applied

BREAK Framework v1.0 (see METHOD.md)

All findings must cite:
- Specific line numbers or config keys
- Severity level (Critical / Warning / Note)
- Remediation path

---

## Clause Findings

### Clause 1: Baseline Stability

**Status:** [ ] Clear  [ ] Finding

**Examined:**
- Weight initialization scheme
- Pre-trained checkpoint integrity
- Base model forward pass consistency

**Finding (if any):**
```
Line/Key: 
Observation: 
Severity: 
Remediation: 
```

**Evidence block:**
```python
# Paste relevant code section
```

---

### Clause 2: Rate and Schedule

**Status:** [ ] Clear  [ ] Finding

**Examined:**
- Learning rate magnitude
- Warmup steps configuration
- Decay schedule appropriateness

**Finding (if any):**
```
Line/Key: learning_rate, warmup_steps, lr_scheduler_type
Observation: 
Severity: 
Remediation: 
```

**Evidence block:**
```yaml
# Paste training config section
```

---

### Clause 3: Embedding and Normalization

**Status:** [ ] Clear  [ ] Finding

**Examined:**
- Layer normalization placement (pre-norm vs post-norm)
- Embedding layer freeze/unfreeze state
- Normalization epsilon values

**Finding (if any):**
```
Line/Key: 
Observation: 
Severity: 
Remediation: 
```

**Evidence block:**
```python
# Paste normalization code
```

---

### Clause 4: Attention and Memory

**Status:** [ ] Clear  [ ] Finding

**Examined:**
- Attention mask handling
- KV-cache configuration for inference
- Memory-efficient attention flags
- Gradient checkpointing setup

**Finding (if any):**
```
Line/Key: 
Observation: 
Severity: 
Remediation: 
```

**Evidence block:**
```python
# Paste attention configuration
```

---

### Clause 5: Knowledge Preservation

**Status:** [ ] Clear  [ ] Finding

**Examined:**
- Catastrophic forgetting mitigations
- LoRA/adapter configuration (if applicable)
- Layer freezing strategy
- Regularization terms

**Finding (if any):**
```
Line/Key: 
Observation: 
Severity: 
Remediation: 
```

**Evidence block:**
```python
# Paste relevant code
```

---

## Severity Story

| Clause | Severity | Impact if Ignored |
|--------|----------|-------------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

**Aggregate risk level:** [ ] Low  [ ] Medium  [ ] High  [ ] Critical

**Narrative:**
[One paragraph explaining the overall risk profile and interaction effects between findings]

---

## Launch Call

**Verdict:** [ ] CLEAR  [ ] CONDITIONAL  [ ] HOLD

**Conditions (if CONDITIONAL):**
1. 
2. 
3. 

**Required before launch:**
- [ ] All Critical findings resolved
- [ ] Warning findings documented with acceptance rationale
- [ ] Tripwire thresholds configured
- [ ] Rollback checkpoint identified

---

## Tripwire Configuration

```yaml
tripwires:
  gradient_norm_multiplier: 10
  consecutive_violations: 3
  loss_nan_halt: true
  loss_inf_halt: true
  memory_zero_check: true
  checkpoint_failure_halt: true
  
baseline_metrics:
  initial_loss: [to be filled after first forward pass]
  initial_grad_norm: [to be filled after first backward pass]
  
rollback_checkpoint: [path to last known good state]
```

---

## Sign-Off

**Auditor:** 
**Date:** 
**Time spent:** 

**Charter hash:** [sha256 of this document before sign-off]

---

*This charter follows the BREAK framework. See METHOD.md for clause definitions.*