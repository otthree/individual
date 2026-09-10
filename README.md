# Study of individual differences in Roy Lab

Multi-cohort study of T1-weighted ratio as a marker of individual differences.

> **Status: early stage (2026).** Research question and modeling approach are being finalized. This repository will hold the public-facing code, configs, and documentation as the project matures.

## Overview

Most neuroimaging models are trained to predict a group-level outcome and treat between-subject variability as noise. This project takes the opposite view: individual differences are the signal. Using the T1-weighted ratio derived from structural MRI as a candidate biomarker, the goal is to build deep learning models that explicitly represent subject-level variation across multiple cohorts, with an eye toward precision-medicine applications (diagnosis, prognosis, treatment response, and subtyping).

The work is done in collaboration with Roy Seo at CAMH / KCNI (Toronto). I am responsible for the engineering and data science side: data pipeline, training infrastructure, experiment tracking, and model development.

## Approach (candidates under evaluation)

- **Biomarker:** T1-weighted ratio computed from structural MRI, harmonized across cohorts
- **Modeling individual differences:** subject embeddings, FiLM / LoRA style conditioning, mixed-effects layers
- **Training:** PyTorch on a SLURM-managed GPU cluster, with a fixed GPU-hour budget split between pretraining, fine-tuning, and ablations
- **Tracking and release:** Weights & Biases for runs and checkpoints, Hugging Face Hub for released models and datasets

## Tech stack

- Python 3.11, PyTorch, torchvision
- MONAI, nibabel (neuroimaging I/O and transforms)
- Hugging Face `transformers` / `datasets` / `huggingface_hub`
- Weights & Biases, Hydra (config management)
- conda environment, SLURM job scripts

## Engineering principles

- Large data (on the order of 1 TB) lives on cluster storage or the Hugging Face Datasets Hub, never in git.
- Model checkpoints go to W&B Artifacts or the Hugging Face Hub; the repo keeps only the configs needed to reproduce them.
- Every experiment is a W&B run backed by a versioned config in `src/configs/`.
- Secrets (API keys, tokens) are excluded via `.gitignore` and loaded from the environment.

## Getting started

```bash
git clone https://github.com/otthree/individual.git
cd individual
conda env create -f environment.yml
conda activate project-env
wandb login
huggingface-cli login
```

## License

MIT
