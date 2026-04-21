# CLAIM-CAL: Claim-Level Self-Verification for Calibrated LLM Reliability

This repository contains the code, results, figures, and paper for **CLAIM-CAL**, a research experiment on improving the reliability of Large Language Models through claim-level self-verification and uncertainty calibration.

## Overview

Large Language Models often generate fluent answers with high confidence, even when those answers are factually incorrect. CLAIM-CAL addresses this problem by estimating answer reliability at the level of individual factual claims.

Instead of directly trusting the model's confidence, CLAIM-CAL:

1. Generates an initial answer.
2. Decomposes the answer into atomic factual claims.
3. Verifies each claim using multiple verification probes.
4. Labels each claim as Supported, Contradicted, or Unknown.
5. Converts claim-level verdicts into a risk score.
6. Converts answer-level risk into confidence.
7. Applies post-hoc calibration to improve confidence reliability.

## Method

For each claim, CLAIM-CAL computes:

```text
support_score = supported_count / total_checks
contradiction_score = contradicted_count / total_checks
unknown_score = unknown_count / total_checks

Claim-level risk is calculated as:

risk_i = 0.5 * contradiction_score
       + 0.3 * unknown_score
       + 0.2 * (1 - support_score)

Answer-level risk is the average claim risk:

answer_risk = average(risk_i)

Raw confidence is:

confidence = 1 - answer_risk

Post-hoc calibration is then applied using isotonic regression.

Dataset

The experiment uses the TruthfulQA generation benchmark.

Compared Methods

The following methods are compared:

Direct Answer
Verbal Confidence
Self-Consistency
Simple Self-Verification
CLAIM-CAL
CLAIM-CAL + Calibration
Evaluation Metrics

The experiment reports:

Accuracy
Incorrect Rate
Expected Calibration Error
Brier Score
Average Confidence
Selective Accuracy @ 0.7
Coverage @ 0.7
Risk-Coverage Curve
Reliability Diagram
Key Finding

CLAIM-CAL + Calibration achieved the best calibration performance while maintaining strong answer accuracy. The results suggest that claim-level verification is useful as a reliability signal, especially when combined with post-hoc calibration.

Repository Structure
paper/       Final research paper
notebook/    Google Colab notebook
results/     CSV and JSON experiment results
figures/     Reliability, risk-coverage, and comparison plots

How to Run
Clone the repository:
git clone https://github.com/abhigyanpal1/claim-cal-llm-reliability.git
cd claim-cal-llm-reliability
Install dependencies:
pip install -r requirements.txt
Set your OpenAI API key:
export OPENAI_API_KEY="your_api_key_here"

On Windows PowerShell:

$env:OPENAI_API_KEY="your_api_key_here"
Run the notebook in Google Colab or run the Python export from src/.
Important Note

This project uses LLM-based verification and LLM-as-judge evaluation. An automated second-judge robustness check is included, but this should not be treated as a substitute for full human evaluation.

Citation

If you use this work, please cite:

@misc{claimcal2026,
  title={Improving Reliability of Large Language Models via Claim-Level Self-Verification and Uncertainty Calibration},
  author={Abhigyan Pal},
  year={2026},
  note={Preprint}
}
License

This project is released under the MIT License.


Replace:

```text
abhigyanpal1