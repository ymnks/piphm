# Data

This repository does **not** redistribute any monitoring data. The datasets belong to
their providers and are governed by their own terms; we document the sources and the
preprocessing contract so that the pipeline can be re-run on legally obtained copies.

## Sources

| Site | Type | Source | Terms |
|---|---|---|---|
| Åknes, Norway | Hard rockslide; borehole inclinometer network, pore pressure, seismicity, meteorology | Published monitoring release, DOI `10.11582/2024.00036` | CC-BY-4.0 |
| Three Gorges Reservoir, China | Deposit / soil landslides; surface displacement surveys, rainfall, reservoir level | Published by the responsible monitoring authority | Access conditions apply; not redistributed here |

The release notes accompanying the code will list the exact version, file list and
checksums of the copies used, so that a reader who obtains the same release can verify
that the inputs match.

## What we ask of users

- Obtain the data from the official source, not from a mirror.
- Do not attempt to re-identify monitoring points beyond what the source already
  publishes.
- Cite the data providers alongside this repository.

## Preprocessing contract

These rules are part of the method, not implementation detail, and are enforced in
`data/`:

1. **Standardisation statistics are computed on the training split only** and applied
   unchanged to validation and test.
2. **Splits are chronological**; no shuffling, and no window may straddle a split
   boundary.
3. **Labels are derived only from information available at prediction time.** The onset
   catalogue is used to construct targets, never as an input feature.
4. **Every transformation is fitted inside the training fold**, including detrending,
   deseasonalisation and any threshold used for labelling.
5. **Run metadata records the exact channel set used**, so that a run can be reproduced
   channel by channel.

## Evaluation protocol

- Metrics are computed on the held-out test split only.
- The decision threshold is selected on the validation split; test labels are never used
  for threshold selection or early stopping.
- Where a metric depends on a tolerance or an alarm budget, both are reported alongside
  the metric value.
