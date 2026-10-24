# PI-PHM

**Physics-Guided and Stage-Aware Hybrid Model for Landslide Creep-Burst Event Anticipation**

> **Status: code release in preparation.**
> This repository currently contains the release skeleton and the documentation.
> The full implementation, training scripts and evaluation code will be published here
> **after the paper is accepted**. The project is under active development.

---

## What this repository is

PI-PHM reformulates landslide warning as **stage-constrained event anticipation**:
instead of regressing displacement, the model predicts whether a creep burst will occur
inside a future window, while a differentiable penalty discourages stage transitions
that the known progressive-failure sequence does not allow.

The repository is organised around the method's components:

| Component | Where it lives |
|---|---|
| Physics-derived multimodal representation | `data/` |
| Sequential Token-to-State backbone | `models/` |
| Stage-transition regularisation | `losses/` |
| Training procedure | `training/` |
| Metrics and decision rule | `evaluation/` |
| Main comparisons and sensitivity analyses | `experiments/` |

---

## Repository layout

```
.
├── configs/          Configuration files
├── data/             Data access, preprocessing, features, labels, splits
├── models/           Patch embedding, PatchTST encoder, Mamba block, prediction heads
├── losses/           Event, stage and transition-regularisation terms
├── training/         Trainer, early stopping, checkpointing
├── evaluation/       Metrics, lead time, threshold calibration, placebo tests
├── experiments/      Scripts reproducing each table and sensitivity analysis
├── scripts/          One-command entry points
├── utils/            Seeding, logging, I/O helpers
├── docs/             Data notes and reproducibility notes
└── assets/           Figures
```

### `configs/`

Configuration files for the model, training and evaluation settings. These are added
together with the code.

### `data/`

| File | Contents |
|---|---|
| `loader.py` | Reader for the multimodal monitoring sequences (displacement, pore pressure, rainfall, reservoir level, seismicity). |
| `preprocess.py` | Standardisation fitted on the training split only. |
| `physics_features.py` | Velocity, acceleration, inverse velocity, pore-pressure rate, reservoir-level rate. |
| `labels.py` | Event labels from the published onset catalogue; automatic stage labels. |
| `splits.py` | Chronological 70/10/20 split. |

> **No monitoring data are hosted in this repository.** See `docs/DATA.md` for the
> official sources, licences and how to obtain them.

### `models/`

| File | Contents |
|---|---|
| `patch_embed.py` | Non-overlapping patch segmentation and linear projection. |
| `patchtst_encoder.py` | Transformer encoder over patch tokens. |
| `mamba_block.py` | Selective state-space block with ZOH discretisation. |
| `heads.py` | Event head and stage head. |
| `pi_phm.py` | The assembled model, returning event logits and per-step stage logits. |

### `losses/`

| File | Contents |
|---|---|
| `event.py` | Binary cross-entropy for event anticipation. |
| `stage.py` | Cross-entropy over the four stages. |
| `transition.py` | Transition penalty over adjacent-step stage distributions. |
| `total.py` | Weighted sum of the event, stage and transition terms. |

### `training/`

| File | Contents |
|---|---|
| `trainer.py` | Training loop. |
| `early_stopping.py` | Best-checkpoint tracking on validation PR-AUC. |
| `checkpoint.py` | Save/restore, including the configuration hash. |

### `evaluation/`

| File | Contents |
|---|---|
| `metrics.py` | PR-AUC, F1, recall, precision. |
| `lead_time.py` | Lead time, averaged over true-positive events. |
| `calibration.py` | Threshold selection on the validation split, and the alarm-persistence rule. |
| `confusion.py` | Per-class stage confusion matrix. |
| `placebo.py` | Circular-shift null distribution for onset detection. |

### `experiments/`

| File | Purpose |
|---|---|
| `main_results.py` | Event-anticipation performance on the three sites. |
| `ablation.py` | Component ablation. |
| `hyperparameter_sensitivity.py` | λ1/λ2 grid. |
| `window_sensitivity.py` | Sensitivity to the prediction window. |
| `efficiency.py` | Parameter counts, FLOPs, latency and peak memory. |
| `transfer.py` | Leave-one-landslide-out transfer and fine-tuning. |

### `scripts/`

| File | Contents |
|---|---|
| `train.sh` | Train one model on one site with one seed. |
| `evaluate.sh` | Evaluate a checkpoint and write the metric record. |
| `reproduce_all.sh` | End-to-end run of every table. |

### `docs/`

| File | Contents |
|---|---|
| `DATA.md` | Data sources, licences, access conditions and the preprocessing contract. |
| `REPRODUCIBILITY.md` | Seeds, split indices, input hash manifest and the commands behind each table. |

### `utils/` and `assets/`

Shared seeding, logging and I/O helpers; figures used in the README and in the manuscript.

---

## Release plan

1. **Now** — skeleton and documentation.
2. **Before camera-ready** — full implementation, training and evaluation scripts.
3. **Upon acceptance** — split indices, input hash manifest and the exact commands behind
   each reported number.
4. **Ongoing** — the code is maintained after release; corrections are recorded in the
   changelog rather than applied silently.

## What is **not** released here

- **Raw monitoring data.** The datasets are distributed by their respective providers
  under their own terms; we link to the official sources instead of mirroring them.
- **Trained weights.** Released only if the hosting terms allow it.

## Reproducibility commitments

- Every reported number will be traceable to a script, a configuration and a seed set.
- Standardisation statistics, thresholds and early-stopping decisions are fitted on the
  training/validation splits only.
- Split indices and an input hash manifest will be published with the code.

## Citation

```bibtex
@inproceedings{piphm,
  title     = {PI-PHM: Physics-Guided and Stage-Aware Hybrid Model for
               Landslide Creep Burst Event Anticipation},
  author    = {Anonymous},
  booktitle = {Under review},
  year      = {2026}
}
```

Author identities and the final bibliographic record will be filled in upon publication.

## Licence

The licence will be added together with the code release. Until then this repository is
provided for reference only, and no rights are granted for reuse.
