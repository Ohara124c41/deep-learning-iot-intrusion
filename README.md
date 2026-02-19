# Deep Learning Systems (P4)

## Project Description
This project implements a deep learning experiment workflow for IIoT cybersecurity detection using the UCI RT-IoT2022 dataset. The notebook loads and preprocesses network telemetry, trains a Transformer-based binary classifier (`attack` vs `benign`) in PyTorch, and runs a controlled comparison where exactly one major factor changes. The workflow emphasizes reproducibility, experimental rigor, and security-relevant interpretation rather than only maximizing accuracy.

## Project Repository
- https://github.com/Ohara124c41/deep-learning-iot-intrusion

## What I Built
- `deep_learning.ipynb`: end-to-end deep learning experiment notebook
- `requirements.txt`: Python dependencies for reproducible execution
- `Deep_Learning_Systems_Analysis_Report_draft.md`: report draft template (export to PDF for submission)

## Dataset
- Name: RT-IoT2022
- Source (UCI): https://archive.ics.uci.edu/dataset/942/rt-iot2022
- In-notebook access: `ucimlrepo` fetch (with local cache fallback to `data/raw/rtiot2022_raw.csv`)

## How To Run
1. Create and activate a virtual environment (WSL/Ubuntu example):
```bash
python3 -m venv .venv
source .venv/bin/activate
```
2. Install dependencies:
```bash
pip install -r requirements.txt
```
3. Launch Jupyter:
```bash
jupyter notebook
```
4. Open and run:
`deep_learning.ipynb`

## Reproducibility Notes
- The notebook includes package bootstrap logic for missing dependencies.
- Data is cached locally under `data/raw/` after first successful download.
- Deterministic controls include fixed random seeds and fixed split strategy.
- Before submission, refresh environment lock file:
```bash
pip freeze > requirements.txt
```

## Controlled Experiment Definition
- Baseline: Transformer model with dropout `0.10`.
- Experimental model: identical architecture and training setup, except dropout `0.30`.
- This satisfies the requirement of changing exactly one major aspect.

## Extended Analyses 
- Validation-threshold sweep and test-time threshold transfer.
- Error-case inspection (false negatives/false positives by original class labels).
- Multi-ablation suite (architecture, optimizer, learning rate, class-weight settings).
- Deep ensembles (mean and weighted top-k probability ensembles).
- Score verifier with guardrail-aware multi-metric model selection.
- AIF360 subgroup-risk audit and NIST-aligned readiness checks.
- V&V checklist cell for explicit requirement traceability.
