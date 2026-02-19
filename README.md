# Deep Learning Systems (P4)

## Project Description
This project implements a full deep learning experiment workflow for IIoT cybersecurity detection using the UCI RT-IoT2022 dataset. The notebook loads and preprocesses real network telemetry, trains Transformer-based binary classifiers (`attack` vs `benign`) in PyTorch, runs a controlled baseline-vs-experiment comparison, then extends to ablations and ensembles with a guardrail-aware score verifier. The goal is a reproducible, operations-oriented deep learning component for a broader DevSecAIOps local-cloud architecture.

## Project Repository
- https://github.com/Ohara124c41/deep-learning-iot-intrusion

## What I Built
- `deep_learning.ipynb`: end-to-end deep learning experiment notebook
- `requirements.txt`: project-scoped dependencies for reproducible execution
- `Deep_Learning_Systems_Analysis_Report_draft.pdf`: report aligned to notebook outputs

## Dataset
- Name: RT-IoT2022
- Source (UCI): https://archive.ics.uci.edu/dataset/942/rt-iot2022
- Access method: `ucimlrepo` fetch with local cache fallback at `data/raw/rtiot2022_raw.csv`

## Key Run Snapshot
- Raw ingestion shape: `123,117 x 84`
- Modeling frame: `120,000 x 87`
- Binary target counts: benign `12,171`, attack `107,829` (attack rate `0.8986`)
- Split sizes: train `84,000`, validation `18,000`, test `18,000`
- Final selected model (score verifier): `ensemble_weighted_top3_valf1`
- Final test metrics: `F1=0.9968`, `PR-AUC=0.99996`, `RMSE(prob)=0.0624`

## Controlled Experiment
- Baseline: Transformer with dropout `0.10`
- Experimental: identical setup except dropout `0.30`
- Test comparison (threshold `0.5`):
  - Baseline: `F1=0.9956`, `Recall=0.9923`, `PR-AUC=0.99993`, `RMSE(prob)=0.0878`
  - Experiment: `F1=0.9966`, `Recall=0.9949`, `PR-AUC=0.99986`, `RMSE(prob)=0.0743`

## Extended Analyses
- Threshold sweep and test-time transfer from validation-optimal threshold
- Error-case analysis by original attack labels
- Single-change ablations (`d_model`, depth, learning rate, optimizer, class weighting)
- Deep ensembles (mean and weighted top-3)
- Multi-metric score verifier with recall/FPR guardrails
- AIF360 subgroup audit (SPD/DI/EOD/AOD)
- NIST-aligned deterministic readiness checks

## How To Run
1. Create and activate a virtual environment:
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
- The notebook includes bootstrap checks for required/optional packages.
- Seeds, split strategy, and key hyperparameters are fixed in configuration cells.
- Data is cached locally after first fetch for repeat runs.
- Preprocessed split artifacts are exported to `data/processed/` (`.npz` arrays + metadata manifest) for deterministic reuse.
- Refresh lock file from your project venv before submission:
```bash
pip freeze > requirements.txt
```

## Bias and Risk Notes
- AIF360 selected subgroup `is_proto_tcp` and found measurable disparity (`SPD=-0.305`, `DI=0.671`, `EOD=-0.043`), indicating error-rate behavior differs by protocol subgroup.
- Error concentration is asymmetric: baseline false negatives were mostly `ARP_poisioning`, while false positives were mostly `Thing_Speak`, so production use should include class-level monitoring and human review gates.
