# Mitigating Hallucinations via Epistemic Uncertainty

CENG 467 – Natural Language Understanding and Generation  
İzmir Institute of Technology, Spring 2026  
Student ID: 310201051

## Project Description

This project designs a generation pipeline that estimates epistemic uncertainty
of LLM outputs using self-consistency sampling and entropy-based filtering to
mitigate hallucinations in question answering tasks.

## Datasets

- **TruthfulQA** – 817 multiple-choice questions (Hugging Face: `truthful_qa`)
- **HaluEval QA** – 200-sample subset (from 10,000 QA pairs with hallucinated/correct answer pairs)

## Methods

| Approach | Description |
|----------|-------------|
| Zero-Shot Prompting | Single deterministic response (temperature=0) |
| Self-Consistency | Majority vote over N=5 sampled responses (temperature=0.7) |
| Entropy Filtering | Abstain on questions where normalized entropy ≥ threshold |

## Results

### TruthfulQA (817 samples, multiple-choice)

| Method | Accuracy | Coverage |
|--------|----------|----------|
| Baseline 1: Zero-Shot | 52.8% (431/817) | 100% |
| Baseline 2: Self-Consistency (N=5) | 53.0% (433/817) | 100% |
| Baseline 3: Entropy Filtering (t=0.1) | 62.7% (598/817) | 73.2% |

### HaluEval QA (200 samples, open-ended)

| Method | Accuracy | Avg H_norm |
|--------|----------|------------|
| Self-Consistency (N=5) | 92.5% (185/200) | 0.081 |
| Low entropy subset (H < 0.3) | 96.1% | — |
| High entropy subset (H ≥ 0.7) | 60.0% | — |

## Key Finding

Entropy is a reliable uncertainty signal across both datasets and task formats:
- Low entropy → model is confident → higher accuracy
- High entropy → model is uncertain → accuracy drops significantly

## Setup

```bash
pip install groq datasets numpy matplotlib
```

## How to Run

1. Open `467_term_project.ipynb` in Google Colab
2. Mount your Google Drive when prompted
3. Set your Groq API key in the relevant cell
4. Run all cells in order

## Repository Structure
├── 467_term_project.ipynb   # Main experiment notebook
├── baselines/               # Baseline scripts
├── data/                    # Data preprocessing
├── evaluation/              # Evaluation metrics
├── requirements.txt
└── README.md

## Notes

- Dataset files are saved to Google Drive under `ceng467_project/data/`
- API keys are not committed to this repository
