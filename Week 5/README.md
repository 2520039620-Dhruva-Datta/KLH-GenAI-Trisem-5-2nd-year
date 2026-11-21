# Week 5: Token Economics & Rate-Limit Budgeting Tool

## Overview
A financial and capacity planning tool designed to evaluate whether a proposed LLM-powered feature fits within configured financial budgets and provider rate limits (RPM and TPM) before building it.

## Key Concepts
- **Input Tokens**: System prompt, chat history, user query, retrieved RAG context.
- **Output Tokens**: Model response length.
- **Requests Per Minute (RPM)**: Maximum allowed queries per minute.
- **Tokens Per Minute (TPM)**: Maximum throughput allowed per minute.
- **Safety Margin**: Buffer percentage reserved for spikes, retries, and estimation variance.

## Configuration (.env)
```env
INPUT_PRICE_PER_MILLION=0.30
OUTPUT_PRICE_PER_MILLION=1.20
REQUESTS_PER_DAY=2000
AVG_INPUT_TOKENS=1200
AVG_OUTPUT_TOKENS=300
PEAK_REQUESTS_PER_MINUTE=30
RPM_LIMIT=60
TPM_LIMIT=100000
MONTHLY_BUDGET_USD=50
DAILY_BUDGET_USD=2
SAFETY_MARGIN_PERCENT=20
```

## How to Run

### Run Budget Evaluation Report
From repository root:
```powershell
& ".\myenv\Scripts\python.exe" "Week 5\budget.py"
```
Or from inside `Week 5`:
```powershell
..\myenv\Scripts\python.exe budget.py
```

### Run Automated Unit Tests
```powershell
cd "Week 5"
..\myenv\Scripts\python.exe test_budget.py
cd ..
```

## Expected Output
```text
TOKEN ECONOMICS REPORT
-----------------------------------
Input tokens/day:  2,400,000
Output tokens/day: 600,000
Total tokens/day:  3,000,000
Estimated daily cost:   $1.4400
Estimated monthly cost: $43.20
Peak RPM with margin: 36.0
Peak TPM with margin: 54000

CHECKS
RPM capacity: PASS
TPM capacity: PASS
Daily budget: PASS
Monthly budget: REVIEW
```
