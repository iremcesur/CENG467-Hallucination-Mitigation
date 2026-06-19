# Mitigating Hallucinations via Epistemic Uncertainty

**CENG 467 — Natural Language Understanding and Generation, Term Project**
İzmir Institute of Technology — Spring 2026
İrem Cesur (310201051)

A self-consistency and entropy-filtering approach to detecting and reducing
hallucinations in LLM outputs, evaluated on TruthfulQA and HaluEval using
Llama-3.1-8B-Instant via the Groq API.

## Summary

| Method | Accuracy | Coverage |
|---|---|---|
| Baseline 1 — Zero-Shot | 52.8% | 100% |
| Baseline 2 — Self-Consistency ($N{=}5$) | 53.0% | 100% |
| Baseline 3 — Entropy Filtering ($t{=}0.1$) | **62.7%** | 73.2% |

Entropy filtering yields a **+9.9 percentage-point** accuracy improvement over
zero-shot prompting by abstaining on the 26.8% of questions where the model's
self-consistency vote distribution is most uncertain.

Beyond accuracy/coverage, entropy is validated as a correctness predictor using
AUROC (0.640 TruthfulQA / 0.735 HaluEval), Precision/Recall/F1, and Expected
Calibration Error (ECE). Two proposed refinements are directly tested rather
than left as speculative future work:
- **Temperature scaling** reduces ECE by 82.0% (TruthfulQA) and 69.5% (HaluEval).
- **Semantic clustering** of HaluEval responses unexpectedly *degrades*
  discriminative power (AUROC 0.735 → 0.497), since sampled responses are
  overwhelmingly near-paraphrases regardless of correctness.

Full methodology, results, and discussion are in the final report:
[`report/310201051_TermProject.pdf`](report/310201051_TermProject.pdf).

## Repository Structure

```
.
├── 310201051_TermProject.ipynb   # Full, executable project notebook
│
├── data/                         # Downloaded & preprocessed datasets
│   ├── truthfulqa_mc.json        # TruthfulQA mc1, 817 questions
│   ├── halueval_qa_clean.json    # HaluEval QA, 10,000 samples (parsed)
│   └── halueval_qa_raw.json      # HaluEval QA, unprocessed
│
├── evaluation/                   # Raw evaluation outputs (model predictions,
│   │                             #   entropy values, correctness labels)
│   ├── baseline1_zeroshot_cot_results.json
│   ├── baseline2_selfconsistency_results.json
│   ├── baseline3_entropy_filtering_results.json
│   ├── halueval_selfconsistency_results.json
│   └── semantic_clustering_summary.json
│
├── report/                       # LNCS-format final report
│   ├── 310201051_TermProject.pdf
│   ├── main.tex
│   ├── references.bib
│   └── *.png                     # All report figures
│
├── requirements.txt
└── README.md
```

All three baselines (Zero-Shot, Self-Consistency, Entropy Filtering) are
implemented as sequential, documented sections within the single project
notebook rather than as separate script files — see "Notebook Structure"
below.

## Datasets

- **TruthfulQA** ([Lin et al., 2022](https://arxiv.org/abs/2109.07958)) — 817
  multiple-choice questions (mc1) targeting common misconceptions.
- **HaluEval QA** ([Ji et al., 2023](https://arxiv.org/abs/2305.11747)) — 200
  randomly sampled (seed=42) open-ended QA pairs with paired correct/hallucinated
  answers.

## Method

1. **Zero-Shot** — single greedy ($t{=}0.0$) completion per question.
2. **Self-Consistency** — $N{=}5$ stochastic ($T{=}0.7$) samples per question,
   aggregated via majority vote; Shannon entropy over the vote distribution
   serves as an uncertainty proxy.
3. **Entropy Filtering** — answer only when normalized entropy
   $H_{\text{norm}} < t$; sweep $t$ to find the accuracy/coverage trade-off.

See the report for full equations and the HaluEval-specific evaluation protocol.

## Notebook Structure

`310201051_TermProject.ipynb` is organized into the following sections:

| Section | Content |
|---|---|
| 1–3 | Environment setup, TruthfulQA & HaluEval download/inspection |
| 4 | Baseline 1 — Zero-Shot Prompting |
| 5 | Baseline 2 — Self-Consistency ($N{=}5$) |
| 6 | Baseline 3 — Entropy Filtering |
| 7 | HaluEval — Self-Consistency + Entropy Filtering |
| Additional Evaluation Metrics | AUROC, Precision/Recall/F1, Expected Calibration Error (ECE) |
| Extended Analyses | Temperature scaling (post-hoc calibration), semantic clustering of HaluEval responses |

## Reproducing the Results

1. Open `310201051_TermProject.ipynb` in Google Colab.
2. Run the setup and dataset-download cells (Sections 1–3).
3. Either re-run the baseline cells (Sections 4–7, requires a Groq API key) or
   load the existing outputs directly from `evaluation/` to skip straight to
   the Additional Evaluation Metrics and Extended Analyses sections — no new
   API calls are needed for those.

## References

Full reference list in [`report/references.bib`](report/references.bib),
covering Wang et al. (2023, self-consistency), Kuhn et al. (2023, semantic
entropy), Lin et al. (2022, TruthfulQA), Ji et al. (2023, HaluEval & survey),
Kadavath et al. (2022), and Kojima et al. (2022).
