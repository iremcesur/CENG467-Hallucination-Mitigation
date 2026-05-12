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

## Results (Baseline 1)
| Model | Dataset | Accuracy |
|---|---|---|
| Zero-Shot (Llama-3.1-8B-Instant) | TruthfulQA MC | 52.8% (431/817) |
| Self-Consistency | TruthfulQA MC | In progress |
| SC + Entropy Filtering | TruthfulQA MC | Planned |

## How to Run
1. Open `notebooks/01_data_and_baseline1.ipynb` in Google Colab
2. Mount your Google Drive when prompted
3. Set your Groq API key in the relevant cell
4. Run all cells in order

## Notes
- Dataset files are saved to Google Drive under `ceng467_project/data/`

## Prompts & API Configuration
Prompt templates and API configuration are documented in `notebooks/data_and_baseline1.ipynb`.
API keys are loaded as variables in the notebook and are not committed to this repository.
