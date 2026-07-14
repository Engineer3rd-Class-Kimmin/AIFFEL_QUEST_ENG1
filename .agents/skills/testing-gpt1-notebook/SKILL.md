---
name: testing-gpt1-notebook
description: Execute and validate the MQ03 GPT-1 notebook end-to-end, including training objectives, downstream heads, generation, and masking behavior.
---

# Testing the GPT-1 Notebook

## Devin Secrets Needed

None. The notebook runs locally without external services or credentials.

## Environment

Use the pinned runtime:

```bash
python -m pip install --user \
  "tensorflow-cpu==2.15.0" \
  "sentencepiece==0.2.0"
```

Verify versions before execution:

```bash
python -c "import tensorflow as tf, sentencepiece as spm; print(tf.__version__, spm.__version__)"
```

## Full execution

Run the notebook from `Main_Quest/MQ03` so relative `artifacts/gpt1` paths resolve consistently:

```bash
jupyter nbconvert \
  --to notebook \
  --execute \
  --ExecutePreprocessor.timeout=900 \
  --output-dir="<persistent-artifact-directory>" \
  --output="gpt1_executed.ipynb" \
  "gpt1_generative_pretraining.ipynb"
```

Do not use `--ExecutePreprocessor.cwd`; nbconvert does not recognize that option. Run the command with the notebook directory as the shell working directory instead.

With SentencePiece 0.2.0, request string pieces with `out_type=str`, not `out_type="pieces"`.

## Runtime checks

Confirm:

1. Every code cell has an execution count and there are no `output_type="error"` outputs.
2. All pre-training `L1`, fine-tuning classification `L2`, auxiliary `L1`, and combined `L3` values are finite.
3. Final pre-training `L1` is lower than the first value.
4. Final fine-tuning classification `L2` is lower than the first value.
5. The checkpoint exists and is larger than 100 KB.
6. Generation returns decoded text longer than its prompt.
7. LM logits, similarity logits, and multiple-choice logits have the expected shapes.

Treat tiny-corpus sentiment probes as explicit assertions. Report a failed probe as a limitation; do not replace or remove it to obtain a passing result. Add a balanced held-out set with per-class recall when broader generalization evidence is needed.

## Causal-mask check

Create two equal-length sequences with the same prefix and different suffixes. Run both with `training=False`.

- Shared-prefix LM logits must have maximum absolute difference `<=1e-6`.
- Suffix LM logits must differ by `>1e-6`.

This distinguishes a working causal mask from a model that can read future tokens.

## Padding-loss check

Run one semantic sequence in trimmed and right-padded forms.

- `language_model_loss` must differ by `<=1e-6`.

This validates that `<pad>` targets are excluded from the loss independently of attention masking.

## Evidence

Preserve:

- Executed `.ipynb`
- Rendered HTML from `jupyter nbconvert --to html`
- Full nbconvert command log and exit status
- Machine-readable metrics and assertion JSON
- Screenshots or plots showing losses, probes, and masking values

Report exact expected and observed values for every assertion, and never declare complete success if any check fails.
