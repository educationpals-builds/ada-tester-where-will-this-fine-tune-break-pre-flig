# Where Will This Fine-Tune Break? — Pre-Flight Check

A systematic audit bench for identifying failure points in open models before you commit compute to fine-tuning.

## The Specimen

Any open-weights model checkpoint or training configuration you're about to fine-tune. The bench examines:
- Model architecture definitions
- Training hyperparameters
- Data pipeline code
- Normalization and initialization schemes
- Loss function implementations

## The Verdict

Each audit produces one of three outcomes:

| Verdict | Meaning |
|---------|--------|
| **CLEAR** | No blocking issues found; proceed to training |
| **CONDITIONAL** | Issues found but mitigable; document fixes before launch |
| **HOLD** | Critical issues require resolution before any compute spend |

## The Tripwire

Automated checks that halt training if:
- Gradient norm exceeds 10× baseline for 3 consecutive steps
- Loss becomes NaN or Inf
- Memory allocation fails silently (returns zeros)
- Checkpoint save fails without explicit error

## One-Paste Rebuild Block

```bash
# Clone and run pre-flight on your model
git clone <this-repo> preflight-bench
cd preflight-bench

# Paste your model config into specimen.yaml, then:
python -c "
import yaml
with open('specimen.yaml') as f:
    config = yaml.safe_load(f)
print('Specimen loaded:', config.get('model_name', 'unnamed'))
print('Ready for clause walk. See prompts/clause-walk-pack.md')
"
```

## Quick Start

1. Read `METHOD.md` for the framework
2. Copy your model code/config as the specimen
3. Run each prompt from `prompts/clause-walk-pack.md`
4. Compile findings into `charter.md` format
5. Verify with `VERIFY.md` procedure

## Files

- `charter.md` — Full audit template with all five clauses
- `blueprints/pre-flight-bench.md` — Auditor specification
- `prompts/clause-walk-pack.md` — Standalone prompts for each clause
- `METHOD.md` — The BREAK framework
- `VERIFY.md` — Stranger verification procedure

---

*Provenance: ai_drafted, disclosed per `.ep/provenance.json`*

<!-- educationpals-build-verified -->