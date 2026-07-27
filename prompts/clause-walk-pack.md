# Clause Walk Prompt Pack

Five standalone prompts for auditing fine-tuning configurations. Each prompt examines one BREAK clause and returns either a finding with remediation or an earned clear.

Use these in any chat-capable model. Paste the prompt, then paste your code or config when prompted.

---

## Prompt 1: Baseline Stability

```
You are examining code for Baseline Stability issues in a fine-tuning setup.

Baseline Stability checks:
- Weight initialization matches pretrained expectations
- Checkpoint loading completes without silent failures
- Forward pass produces consistent outputs before training
- No unintended random reinitialization of layers

I will paste model initialization code or checkpoint loading code.

For the pasted code:
1. Quote the specific lines relevant to initialization/loading
2. Check for: missing strict=True in load_state_dict, random init after loading, missing keys warnings suppressed, dtype mismatches
3. Return exactly one of:
   - "FINDING: [severity: Critical/Warning/Note] [issue description] [specific remediation]"
   - "EARNED CLEAR: [what was verified and why it passes]"

Paste your code now.
```

---

## Prompt 2: Rate and Schedule

```
You are examining configuration for Rate and Schedule issues in a fine-tuning setup.

Rate and Schedule checks:
- Learning rate appropriate for fine-tuning (typically 1e-5 to 5e-5 for full fine-tune, 1e-4 to 3e-4 for LoRA)
- Warmup steps present and reasonable (typically 3-10% of total steps)
- Decay schedule specified and appropriate
- Batch size and gradient accumulation yield effective batch size

I will paste training configuration (YAML, JSON, or Python config dict).

For the pasted config:
1. Quote the specific keys/values for learning_rate, warmup, scheduler, batch_size
2. Check for: missing warmup, LR too high for method, no decay schedule, effective batch size calculation errors
3. Return exactly one of:
   - "FINDING: [severity: Critical/Warning/Note] [issue description] [specific remediation]"
   - "EARNED CLEAR: [what was verified and why it passes]"

Paste your configuration now.
```

---

## Prompt 3: Embedding and Normalization

```
You are examining code for Embedding and Normalization issues in a fine-tuning setup.

Embedding and Normalization checks:
- LayerNorm placement is consistent (pre-norm vs post-norm matching base model)
- Normalization epsilon values match base model (typically 1e-5 or 1e-6)
- Embedding layers frozen/unfrozen intentionally
- RMSNorm vs LayerNorm matches architecture

I will paste model architecture code or normalization configuration.

For the pasted code:
1. Quote the specific lines defining normalization layers and their placement
2. Check for: norm placement mismatch with base model, epsilon value changes, unintended embedding training, norm type substitution
3. Return exactly one of:
   - "FINDING: [severity: Critical/Warning/Note] [issue description] [specific remediation]"
   - "EARNED CLEAR: [what was verified and why it passes]"

Paste your code now.
```

---

## Prompt 4: Attention and Memory

```
You are examining code for Attention and Memory issues in a fine-tuning setup.

Attention and Memory checks:
- Attention mask handling correct for variable-length sequences
- Flash attention or memory-efficient attention configured properly
- Gradient checkpointing enabled if memory-constrained
- KV-cache disabled during training, enabled during inference

I will paste attention implementation or training configuration.

For the pasted code:
1. Quote the specific lines for attention computation, masking, memory optimization
2. Check for: causal mask errors, flash attention dtype requirements unmet, gradient checkpointing conflicts, KV-cache left enabled during training
3. Return exactly one of:
   - "FINDING: [severity: Critical/Warning/Note] [issue description] [specific remediation]"
   - "EARNED CLEAR: [what was verified and why it passes]"

Paste your code now.
```

---

## Prompt 5: Knowledge Preservation

```
You are examining code for Knowledge Preservation issues in a fine-tuning setup.

Knowledge Preservation checks:
- Catastrophic forgetting mitigations present for full fine-tune
- LoRA rank and alpha appropriate for task complexity
- Layer freezing strategy intentional and documented
- Regularization (weight decay, dropout) appropriate

I will paste fine-tuning configuration or adapter setup code.

For the pasted code:
1. Quote the specific lines for adapter config, frozen layers, regularization
2. Check for: no forgetting mitigation on full fine-tune, LoRA rank too low for complex task, inconsistent layer freezing, excessive weight decay
3. Return exactly one of:
   - "FINDING: [severity: Critical/Warning/Note] [issue description] [specific remediation]"
   - "EARNED CLEAR: [what was verified and why it passes]"

Paste your code now.
```

---

## Usage Notes

1. **Run all five** for complete coverage
2. **Paste real code** — these prompts expect actual specimens, not descriptions
3. **Collect findings** into charter.md format for documentation
4. **Earned clears** still require the auditor to state what was verified

---

## Batch Invocation

To run all clauses in one session:

```
I will paste code for a fine-tuning setup. Run all five BREAK clause checks in sequence:

1. Baseline Stability (initialization, checkpoint loading)
2. Rate and Schedule (LR, warmup, decay)
3. Embedding and Normalization (norm placement, epsilon, embedding freeze)
4. Attention and Memory (masks, memory optimization, gradient checkpointing)
5. Knowledge Preservation (forgetting mitigation, adapters, regularization)

For each clause: quote relevant lines, return FINDING with severity or EARNED CLEAR.
End with summary table and verdict.

[PASTE SPECIMEN HERE]
```

---

*Prompt pack version: 1.0*
*Framework: BREAK (see METHOD.md)*