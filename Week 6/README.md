# Week 6: Golden Question Set Evaluation & Baseline Run (Release v0.1)

## Overview
Automated benchmark evaluation framework that evaluates a local Large Language Model against a curated **Golden Question Set** of 30 generative AI questions, scoring keyword coverage, and producing an official **Release v0.1** evaluation report.

## Components
- `golden_set.json`: 30 benchmark questions covering:
  - Tokenization (BPE, WordPiece, SentencePiece)
  - Token economics & API costing
  - Decoding strategies (greedy decoding, sampling, temperature, top-$k$, top-$p$)
  - Embeddings & token IDs
  - Positional encodings (sinusoidal, RoPE)
  - Transformer architecture & autoregressive generation
- `main.py`: Harness that:
  1. Loads `golden_set.json`
  2. Sends queries to local Ollama API (`http://localhost:11434/api/generate`) with model `qwen2.5-coder:14b` (temperature = 0)
  3. Evaluates response against expected keywords
  4. Classifies as `PASS` (score $\ge 60\%$) or `REVIEW`
  5. Exports detailed records to `baseline_results.csv`
  6. Prints executive summary
- `baseline_results.csv`: Saved evaluation records.

## Prerequisites
Ensure Ollama is running and the model is pulled:
```powershell
ollama pull qwen2.5-coder:14b
```

## How to Run
From repository root:
```powershell
& ".\myenv\Scripts\python.exe" "Week 6\main.py"
```
Or from inside `Week 6`:
```powershell
..\myenv\Scripts\python.exe main.py
```

## Baseline Evaluation Summary (Release v0.1)
```text
======================================
BASELINE EVALUATION SUMMARY
======================================
Release Version: v0.1
Model: qwen2.5-coder:14b
Total Questions: 30
Successful Responses: 29
Passed Questions: 15
Questions for Review: 14
Failed Requests: 1
Average Keyword Coverage: 59.48 %
Results saved to: baseline_results.csv
```
