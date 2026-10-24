# Reproducibility

## What is fixed

| Item | Value |
|---|---|
| Seeds | 42, 123, 456 |
| Split | chronological 70 / 10 / 20, no shuffling |
| Input window `T` | 30 days |
| Prediction window `T_f` | 14 days |
| Standardisation | fitted on the training split only |
| Early stopping | validation PR-AUC, patience 10 |
| Threshold for lead time | selected on the validation split |

## What will be published with the code

1. **Split indices** — the exact day index of every train/validation/test window, so that
   a reader does not have to re-derive them.
2. **Input hash manifest** — MD5 of every input file actually read, so that a mismatch in
   the data release is detected rather than silently absorbed.
3. **Configuration hash** — recorded in every checkpoint, so a metric can be traced to the
   configuration that produced it.
4. **One-command regression** — a script whose exit code is non-zero if any published
   number changes.

## Environment

The reference environment (Python, PyTorch, CUDA, `mamba-ssm`) will be pinned in
`requirements.txt` / `environment.yml` at release, together with the exact versions used
for the reported runs. Hardware differences affect wall-clock time only; any metric that
is hardware-sensitive will be marked as such.

## Changelog

Changes to the code or to this documentation are recorded here after release rather than
applied silently.
