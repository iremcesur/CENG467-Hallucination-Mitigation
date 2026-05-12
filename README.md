# Mitigating Hallucinations via Epistemic Uncertainty
CENG 467 – Natural Language Understanding and Generation  
İzmir Institute of Technology, Spring 2026

## Project Description
This project designs a generation pipeline that estimates epistemic uncertainty 
of LLM outputs using self-consistency sampling and entropy-based filtering to 
mitigate hallucinations in question answering tasks.

## Datasets
- **TruthfulQA** – 817 multiple-choice questions (Hugging Face: `truthful_qa`)
- **HaluEval QA** – 10,000 QA samples with hallucinated/correct answer pairs

## Repository Structure
├── data/               # Dataset download and preprocessing scripts

├── baselines/          # Baseline implementation scripts

├── evaluation/         # Evaluation and metric scripts

├── notebooks/          # Experiment notebooks

├── requirements.txt    # Python dependencies

└── README.md

## Setup
```bash
pip install -r requirements.txt
```

## Methods
| Approach | Description |
|---|---|
| Zero-shot CoT | Single response with chain-of-thought prompting |
| Self-Consistency | Majority vote over N sampled responses |
| SC + Entropy Filtering | Abstain when response entropy exceeds threshold |

## Evaluation
- **TruthfulQA** (MC): Accuracy
- **HaluEval QA**: Pairwise hallucination detection

## Requirements
See `requirements.txt`


