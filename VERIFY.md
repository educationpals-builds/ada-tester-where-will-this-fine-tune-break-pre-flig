# Stranger Verification Procedure

## Purpose

This document enables any third party to verify that the pre-flight bench correctly identifies known issues. The verification uses a seeded specimen with an intentional flaw.

---

## Seeded Specimen

The following code contains a deliberate normalization-placement issue:

```python
# specimen_seeded.py
# Fine-tuning configuration for a pre-norm transformer
# INTENTIONAL FLAW: Post-norm placement on a pre-norm base model

import torch
import torch.nn as nn

class FineTuneBlock(nn.Module):
    def __init__(self, hidden_size=768, num_heads=12):
        super().__init__()
        self.attention = nn.MultiheadAttention(hidden_size, num_heads)
        self.feed_forward = nn.Sequential(
            nn.Linear(hidden_size, hidden_size * 4),
            nn.GELU(),
            nn.Linear(hidden_size * 4, hidden_size)
        )
        # Line 15: Post-norm placement (base model uses pre-norm)
        self.norm1 = nn.LayerNorm(hidden_size, eps=1e-5)
        self.norm2 = nn.LayerNorm(hidden_size, eps=1e-5)
    
    def forward(self, x):
        # Line 20-21: Post-norm pattern (norm AFTER residual)
        attn_out, _ = self.attention(x, x, x)
        x = self.norm1(x + attn_out)  # Post-norm: wrong for pre-norm base
        
        # Line 24: Post-norm pattern continues
        ff_out = self.feed_forward(x)
        x = self.norm2(x + ff_out)  # Post-norm: wrong for pre-norm base
        return x

# Base model specification (for reference):
# Architecture: Pre-norm transformer
# Expected pattern: x = x + attention(norm(x))
# This code implements: x = norm(x + attention(x))
```

---

## Verification Steps

### Step 1: Load the Auditor

Open any chat interface and paste the one-paste invocation from `blueprints/pre-flight-bench.md`:

```
You are a fine-tuning pre-flight auditor following the BREAK framework.

I will paste model code or training configuration. For each paste:
1. Identify what I've provided
2. Walk all five BREAK clauses (Baseline, Rate, Embedding, Attention, Knowledge)
3. For each clause: quote relevant lines, state CLEAR or FINDING with severity
4. End with severity story and verdict (CLEAR/CONDITIONAL/HOLD)

Begin when I paste the specimen.
```

### Step 2: Submit the Seeded Specimen

Paste the entire `specimen_seeded.py` code block above.

### Step 3: Verify Finding Detection

The auditor response MUST include:

**Required elements for passing verification:**

1. **Clause identification:** Must identify this as Clause 3 (Embedding and Normalization) issue

2. **Line citation:** Must reference one or more of:
   - Line 15 (norm layer definition)
   - Line 20-21 (post-norm in attention)
   - Line 24 (post-norm in feed-forward)

3. **Issue description:** Must identify the mismatch between:
   - Post-norm implementation in the code
   - Pre-norm expectation from base model

4. **Severity:** Must be Warning or Critical (not Note)

5. **Remediation:** Must suggest changing to pre-norm pattern:
   - `x = x + attention(norm(x))` instead of `x = norm(x + attention(x))`

### Step 4: Confirm Verdict

Final verdict must be **CONDITIONAL** or **HOLD** (not CLEAR).

---

## Expected Output Pattern

A passing verification will include text similar to:

```
## Clause 3: Embedding and Normalization

Examining: LayerNorm placement pattern

Lines reviewed:
> Line 21: x = self.norm1(x + attn_out)  # Post-norm
> Line 25: x = self.norm2(x + ff_out)    # Post-norm

Result: FINDING

Severity: Warning
Issue: Code implements post-norm pattern but base model specification 
       indicates pre-norm architecture. This mismatch will cause 
       distribution shift during fine-tuning.
Remediation: Refactor to pre-norm pattern:
  x = x + self.attention(self.norm1(x), ...)
  x = x + self.feed_forward(self.norm2(x))
```

---

## Verification Checklist

- [ ] Auditor identifies Clause 3 issue
- [ ] Specific line numbers cited (15, 20-21, or 24)
- [ ] Post-norm vs pre-norm mismatch described
- [ ] Severity is Warning or Critical
- [ ] Remediation suggests pre-norm refactor
- [ ] Final verdict is not CLEAR

**Verification status:** PASS if all boxes checked, FAIL otherwise.

---

## Failure Modes

If verification fails, check:

1. **Auditor didn't examine Clause 3:** Re-prompt with specific clause prompt from `prompts/clause-walk-pack.md`

2. **Wrong lines cited:** Acceptable if any normalization-related line identified

3. **Issue mischaracterized:** Partial credit if normalization mentioned but pattern mismatch not explicit

4. **Verdict was CLEAR:** Verification fails; auditor missed critical issue

---

*Verification procedure version: 1.0*