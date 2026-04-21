# CLAIM-CAL: Claim-Level Self-Verification for Calibrated LLM Reliability

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19680282.svg)](https://doi.org/10.5281/zenodo.19680282)

This repository contains the code, results, figures, and research paper for **CLAIM-CAL**, a research experiment focused on improving the reliability of Large Language Models through claim-level self-verification and uncertainty calibration.

## Overview

Large Language Models often generate fluent answers with high confidence, even when those answers are factually incorrect. This creates a reliability problem because users may trust answers that sound confident but are actually wrong.

**CLAIM-CAL** addresses this problem by estimating answer reliability at the level of individual factual claims instead of trusting the model's direct confidence.

Instead of directly accepting the model's confidence, CLAIM-CAL:

1. Generates an initial answer.
2. Decomposes the answer into atomic factual claims.
3. Verifies each claim using multiple verification probes.
4. Labels each claim as **Supported**, **Contradicted**, or **Unknown**.
5. Converts claim-level verdicts into a risk score.
6. Converts answer-level risk into confidence.
7. Applies post-hoc calibration to improve confidence reliability.

## Method

For each claim, CLAIM-CAL computes:

```text
support_score = supported_count / total_checks
contradiction_score = contradicted_count / total_checks
unknown_score = unknown_count / total_checks
```

Claim-level risk is calculated as:

```text
risk_i = 0.5 * contradiction_score
       + 0.3 * unknown_score
       + 0.2 * (1 - support_score)
```

Answer-level risk is the average claim risk:

```text
answer_risk = average(risk_i)
```

Raw confidence is calculated as:

```text
confidence = 1 - answer_risk
```

Post-hoc calibration is then applied using isotonic regression.

## Dataset

The experiment uses the **TruthfulQA generation benchmark**.

TruthfulQA is useful for this research because it contains questions where language models may generate confident but false answers.

## Compared Methods

The following methods are compared:

- Direct Answer
- Verbal Confidence
- Self-Consistency
- Simple Self-Verification
- CLAIM-CAL
- CLAIM-CAL + Calibration

## Evaluation Metrics

The experiment reports the following metrics:

- Accuracy
- Incorrect Rate
- Expected Calibration Error
- Brier Score
- Average Confidence
- Selective Accuracy @ 0.7
- Coverage @ 0.7
- Risk-Coverage Curve
- Reliability Diagram

## Key Finding

**CLAIM-CAL + Calibration achieved the best calibration performance while maintaining strong answer accuracy.**

The results suggest that claim-level verification is useful as a reliability signal, especially when combined with post-hoc calibration.

In simple terms, CLAIM-CAL does not just ask whether an entire answer is correct. It checks the smaller factual claims inside the answer and uses those checks to produce a more reliable confidence score.

## Repository Structure

```text
claim-cal-llm-reliability/
│
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
│
├── paper/
│   ├── CLAIM_CAL_Final_Paper.pdf
│   └── CLAIM_CAL_Final_Paper.docx
│
├── notebook/
│   └── CLAIM_CAL_TruthfulQA_Experiment.ipynb
│
├── results/
│   ├── results.csv
│   ├── results_with_bootstrap_ci.csv
│   ├── ablation_results.csv
│   ├── automated_judge_robustness_summary.csv
│   ├── automated_judge_confusion_matrix.csv
│   └── experiment_config.json
│
└── figures/
    ├── reliability_plot.png
    ├── risk_coverage_plot.png
    ├── confidence_distribution_plot.png
    └── method_comparison_bar_chart.png
```

## How to Run

Clone the repository:

```bash
git clone https://github.com/abhigyanpal1/claim-cal-llm-reliability.git
cd claim-cal-llm-reliability
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Set your OpenAI API key.

For Linux or macOS:

```bash
export OPENAI_API_KEY="your_api_key_here"
```

For Windows PowerShell:

```powershell
$env:OPENAI_API_KEY="your_api_key_here"
```

Then open and run the notebook:

```text
notebook/CLAIM_CAL_TruthfulQA_Experiment.ipynb
```

The notebook is designed to run in Google Colab.

## Outputs

The experiment produces:

- Results tables
- Ablation results
- Bootstrap confidence intervals
- Reliability diagram
- Risk-coverage curve
- Confidence distribution plot
- Method comparison chart
- Automated judge robustness results

## Important Note

This project uses LLM-based verification and LLM-as-judge evaluation.

An automated second-judge robustness check is included, but this should not be treated as a replacement for full human evaluation.

## Citation

If you use this work, please cite:

```bibtex
@misc{pal2026claimcal,
  title={Improving Reliability of Large Language Models via Claim-Level Self-Verification and Uncertainty Calibration},
  author={Pal, Abhigyan},
  year={2026},
  doi={10.5281/zenodo.19680282},
  url={https://doi.org/10.5281/zenodo.19680282},
  note={Research code, results, and artifacts}
}
```

## Author

**Abhigyan Pal**

GitHub: [abhigyanpal1](https://github.com/abhigyanpal1)

## License

This project is released under the MIT License.
