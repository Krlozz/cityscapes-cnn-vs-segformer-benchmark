---
title: "Cityscapes CNN vs SegFormer Benchmark"
output: github_document
---

## Overview
This repository contains a reproducible benchmark for **urban semantic segmentation** on **Cityscapes**, comparing:

- **CNN baseline:** DeepLabV3-style model  
- **Transformer baseline:** SegFormer

The focus is not on proposing a new architecture, but on measuring how these model families behave under the **same protocol**, across:

1. **Data efficiency** (training with 10/25/50/100% of the training split),
2. **Robustness under corruptions** (blur, noise, compression, etc.),
3. **Compute cost** (latency, VRAM, parameter count),
4. **Qualitative behavior** (GT vs predictions).

---

## What’s inside (quick map)
- `papercnn-transformers.ipynb`  
  Main notebook (training + evaluation + figure/table exports).

- `outputs/`  
  Generated artifacts (checkpoints, logs, splits, figures, tables) and the stage tracker.

---

## Repository structure
- `papercnn-transformers.ipynb`  
- `outputs/`
  - `checkpoints/` — best checkpoints for each run (CNN / SegFormer, per % split)
  - `figures/` — plots and qualitative visualizations used in the paper
  - `logs/` — training histories exported to CSV
  - `splits/` — deterministic subsets for data-efficiency experiments
  - `tables/` — CSV tables used to build the Results section
  - `run_state.json` — stage completion file (prevents re-running completed stages)

---

## Main experimental stages
The notebook is organized as staged execution so results are reproducible and the pipeline can resume without repeating finished work.

### 1) Training and checkpointing
- CNN trained and best checkpoint selected on validation.
- SegFormer trained and best checkpoint selected on validation.

### 2) Data efficiency
Models are trained using fixed fractions of the training set:
- 10%, 25%, 50%, 100%

### 3) Robustness under corruptions
Performance is evaluated under synthetic corruptions (severity controlled), and deltas are computed relative to clean validation.

### 4) Efficiency measurement
Compute is reported using practical indicators measured in runtime:
- inference latency (ms/image)
- peak GPU VRAM (GB)
- parameters (M)

### 5) Qualitative inspection
Predictions are exported to visualize where models behave similarly or diverge.

---

## Outputs (CSV tables used in the paper)
All results are exported as CSV and should be treated as the source-of-truth for tables/plots in the manuscript.

### Data efficiency
- `outputs/tables/cnn_data_efficiency_summary.csv`
- `outputs/tables/sf_data_efficiency_summary.csv`

### Efficiency (latency/VRAM/params)
- `outputs/tables/efficiency_cnn.csv`
- `outputs/tables/efficiency_sf.csv`
- `outputs/tables/efficiency_compare.csv`

### Robustness (corruptions)
- `outputs/tables/robustness_cnn.csv`
- `outputs/tables/robustness_sf.csv`
- `outputs/tables/robustness_compare.csv`

---

## Figures
Below are the **two figures** that best summarize the repository without making the README too long:

1) **Data efficiency comparison** (CNN vs SegFormer)

2) **Qualitative comparison** (Image + GT + CNN Pred + SegFormer Pred)


---

## Figure 1 — Data efficiency (CNN vs SegFormer)
**File:** `outputs/figures/data_efficiency_curve_compare.png`

## Figure 2 — Qualitative comparison (Image + GT + CNN Pred + SegFormer Pred)
**File:** `outputs/figures/val_compare_gt_vs_pred_0_k195.png`