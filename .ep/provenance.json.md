{
  "schema_version": "1.0",
  "build_name": "Where will this fine-tune break? pre-flight check for any open model you're about to train",
  "build_type": "baw.v3",
  "generation_timestamp": "2024-01-01T00:00:00Z",
  "disclosure": {
    "standard": "EU AI Act Article 50",
    "statement": "This content was generated with AI assistance and is disclosed as AI-drafted material."
  },
  "field_attribution": {
    "learner_provided": [],
    "ai_drafted": [
      "README.md",
      "charter.md",
      "blueprints/pre-flight-bench.md",
      "prompts/clause-walk-pack.md",
      "METHOD.md",
      "VERIFY.md",
      ".ep/provenance.json"
    ],
    "pooled": []
  },
  "learner_field_bag": {},
  "marking": "ai_drafted",
  "framework": {
    "name": "BREAK",
    "version": "1.0",
    "clauses": [
      "Baseline Stability",
      "Rate and Schedule",
      "Embedding and Normalization",
      "Attention and Memory",
      "Knowledge Preservation"
    ]
  },
  "verification": {
    "seeded_specimen": "specimen_seeded.py",
    "expected_finding": "Clause 3 - Normalization placement mismatch",
    "expected_lines": [15, 20, 21, 24]
  }
}