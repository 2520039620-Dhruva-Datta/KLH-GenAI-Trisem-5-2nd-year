# KLH Gen AI Skills Lab — Trimester 5 (2nd Year)

Comprehensive practical laboratory implementations and engineering coursework for Generative AI, covering foundational API calls, multi-provider architectural patterns, telemetry & usage logging, subword tokenization, token economics budgeting, and benchmark evaluation using local LLMs.

---

## 📑 Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Prerequisites & Environment Setup](#prerequisites--environment-setup)
- [Weekly Implementations](#weekly-implementations)
  - [Week 1: First LLM API Call with Google Gemini](#week-1-first-llm-api-call-with-google-gemini)
  - [Week 2: Multi-Provider LLM Client Architecture](#week-2-multi-provider-llm-client-architecture)
  - [Week 3: Production LLM Client with Telemetry & MongoDB Logging](#week-3-production-llm-client-with-telemetry--mongodb-logging)
  - [Week 4: Tokenizer Lab (BPE vs. WordPiece vs. SentencePiece)](#week-4-tokenizer-lab-bpe-vs-wordpiece-vs-sentencepiece)
  - [Week 5: Token Economics & Rate-Limit Budgeting](#week-5-token-economics--rate-limit-budgeting)
  - [Week 6: Golden Question Set Evaluation & Baseline Run (Release v0.1)](#week-6-golden-question-set-evaluation--baseline-run-release-v01)
- [Quick Reference Commands](#quick-reference-commands)
- [Automated Testing](#automated-testing)
- [Troubleshooting & Common Issues](#troubleshooting--common-issues)

---

## 🌟 Overview

| Lab Week | Core Topic | Tech Stack / Models | Primary Outcome |
|---|---|---|---|
| **Week 1** | First LLM API Call | Google GenAI SDK, `gemini-3.5-flash-lite` | Basic API authentication and content generation |
| **Week 2** | Unified Client Architecture | Gemini API, Ollama (`qwen2.5-coder:14b`), OOP Strategy Pattern | Abstract provider switching between cloud & local LLMs |
| **Week 3** | Observability & Telemetry | Gemini, Ollama, MongoDB, Requests | Token accounting, latency measurement, retry backoff, DB logging |
| **Week 4** | Subword Tokenization | Hugging Face `transformers`, `sentencepiece`, GPT-2, mBERT, XLM-R | Multi-lingual (EN, TE, HI) token piece and count analysis |
| **Week 5** | Token Economics | Python `dataclasses`, `python-dotenv`, `unittest` | Pre-build financial & capacity feasibility budgeting tool |
| **Week 6** | Model Evaluation | Ollama (`qwen2.5-coder:14b`), Python Benchmark Harness | Golden set (30 questions) keyword coverage baseline run |

---

## 📂 Repository Structure

```text
Skill Work/
├── myenv/                      # Shared Python 3.14 Virtual Environment
├── README.md                   # Master Documentation (this file)
├── Week 1/
│   ├── .env                    # GEMINI_API_KEY
│   ├── main.py                 # Gemini Flash Lite introductory script
│   └── requirements.txt        # google-genai, python-dotenv
├── Week 2/
│   ├── .env                    # GEMINI_API_KEY
│   ├── llm_client.py           # LLMProvider abstract base, GeminiProvider, OllamaProvider
│   ├── main.py                 # Interactive provider selection CLI
│   └── requirements.txt        # google-genai, python-dotenv, requests
├── Week 3/
│   ├── .env                    # GEMINI_API_KEY, MONGODB_URI, pricing rates
│   ├── llm_client.py           # Production client with retries, latency & cost calculation
│   ├── usage_logger.py         # MongoDB persistent telemetry writer
│   ├── main.py                 # Interactive client runner
│   ├── test_client.py          # Unit tests using fake provider & fake logger
│   └── requirements.txt        # google-genai, python-dotenv, requests, pymongo
├── Week 4/
│   ├── requirements.txt        # transformers, sentencepiece
│   └── tokenizer_comparison.py # BPE, WordPiece, SentencePiece comparator
├── Week 5/
│   ├── .env                    # Token rates, traffic assumptions, RPM/TPM limits
│   ├── budget.py               # Token economics budgeting and capacity engine
│   ├── test_budget.py          # Automated unit test suite
│   └── requirements.txt        # python-dotenv
└── Week 6/
    ├── golden_set.json         # 30 benchmark questions, expected answers, and sources
    ├── main.py                 # Baseline evaluation harness with keyword scoring
    ├── baseline_results.csv    # Exported evaluation output dataset
    └── requirements.txt        # requests
```

---

## ⚙️ Prerequisites & Environment Setup

### 1. Python & Virtual Environment
- **Python Version**: Recommended Python 3.10 to 3.14.
- An active virtual environment (`myenv`) is provided at the repository root.

To activate the environment in PowerShell:
```powershell
.\myenv\Scripts\Activate.ps1
```

To run scripts directly using the virtual environment's python interpreter without activating:
```powershell
.\myenv\Scripts\python.exe <script_path>
```

### 2. Ollama Setup (for Local LLM inference in Weeks 2, 3, and 6)
Download and install [Ollama](https://ollama.com/). Ensure the Ollama background service is running on `http://localhost:11434`.

Pull the required local model:
```powershell
ollama pull qwen2.5-coder:14b
```
*(Optional fallback model: `ollama pull llama3.2`)*

### 3. Environment Variables (.env)
Create or verify `.env` files in `Week 1`, `Week 2`, `Week 3`, and `Week 5`:

```env
# Required for Gemini Cloud in Weeks 1, 2, and 3
GEMINI_API_KEY=your_gemini_api_key_here

# Required for MongoDB logging in Week 3
MONGODB_URI=your_mongodb_connection_string

# Gemini pricing estimates (USD per 1M tokens) for Week 3
GEMINI_INPUT_PRICE_PER_MILLION=0.15
GEMINI_OUTPUT_PRICE_PER_MILLION=0.60
```

---

## 📖 Weekly Implementations

### Week 1: First LLM API Call with Google Gemini
- **Objective**: Learn how to authenticate and invoke a remote generative AI model using Google's modern SDK (`google-genai`).
- **Implementation Highlights**:
  - Securely reads `GEMINI_API_KEY` from `.env` using `python-dotenv`.
  - Initializes `genai.Client`.
  - Sends a prompt to `gemini-3.5-flash-lite` and outputs the generated response.
- **How to Run**:
  ```powershell
  cd "Week 1"
  ..\myenv\Scripts\python.exe main.py
  cd ..
  ```

---

### Week 2: Multi-Provider LLM Client Architecture
- **Objective**: Implement the **Strategy Pattern** to decouple application code from specific model vendors, allowing transparent switching between hosted cloud providers and offline local engines.
- **Implementation Highlights**:
  - `LLMProvider` (Abstract Base Class): Declares standard contract `generate(prompt)`.
  - `GeminiProvider`: Implements calls via `google-genai` (`gemini-3.5-flash`).
  - `OllamaProvider`: Implements REST calls to Ollama (`http://localhost:11434/api/generate`) with `qwen2.5-coder:14b`.
  - `LLMClient`: High-level orchestrator interfacing with any provider.
- **How to Run**:
  ```powershell
  cd "Week 2"
  ..\myenv\Scripts\python.exe main.py
  cd ..
  ```

---

### Week 3: Production LLM Client with Telemetry & MongoDB Logging
- **Objective**: Build enterprise-grade LLM client infrastructure with observability, token auditing, financial cost computation, and error resilience.
- **Implementation Highlights**:
  - **Telemetry Tracking**: Records wall-clock latency in milliseconds, prompt tokens, and completion tokens.
  - **Dynamic Costing**: Computes hosted API costs per query based on configured USD rates per 1,000,000 tokens (local Ollama inference marked as $0 API charge).
  - **Fault Tolerance**: Implements exponential backoff retries (`2^attempt` delay) for transient errors (HTTP 408, 429, 5xx, socket timeouts, rate limits).
  - **Persistent Auditing**: `UsageLogger` writes all query metrics directly into MongoDB collection `llm_monitoring.api_usage`.
  - **Unit Testing**: `test_client.py` uses mock providers and mock loggers to verify token counting and client behavior without billing or external dependencies.
- **How to Run**:
  ```powershell
  cd "Week 3"
  # Run the interactive client:
  ..\myenv\Scripts\python.exe main.py

  # Run the unit test suite:
  ..\myenv\Scripts\python.exe test_client.py
  cd ..
  ```

---

### Week 4: Tokenizer Lab (BPE vs. WordPiece vs. SentencePiece)
- **Objective**: Investigate subword tokenization algorithms across different languages (English, Telugu, and Hindi) to observe token piece boundaries, vocabulary representation, and token count discrepancies.
- **Tokenizers Analyzed**:
  - **Byte Pair Encoding (BPE)**: `gpt2` (English-centric, byte-level fallback for non-Latin alphabets).
  - **WordPiece**: `bert-base-multilingual-cased` (Learned subword vocabulary with `##` prefix).
  - **SentencePiece (BPE / Unigram)**: `xlm-roberta-base` (Language-independent raw text subword model using ` ` space prefix).
- **Key Findings**:
  - **English** (`Artificial intelligence is changing the world.`):
    - GPT-2: 8 tokens | Multilingual BERT: 10 tokens | XLM-R: 10 tokens
  - **Telugu** (`కృత్రిమ మేధస్సు ప్రపంచాన్ని మారుస్తోంది.`):
    - GPT-2: **112 tokens** *(severe token explosion due to UTF-8 byte-level fragmentation)*
    - Multilingual BERT: 18 tokens
    - XLM-R: **14 tokens** *(compact representation via dedicated multilingual vocabulary)*
  - **Hindi** (`कृत्रिम बुद्धिमत्ता दुनिया को बदल रही है।`):
    - GPT-2: **68 tokens**
    - Multilingual BERT: 17 tokens
    - XLM-R: **14 tokens**
- **How to Run**:
  ```powershell
  cd "Week 4"
  ..\myenv\Scripts\python.exe tokenizer_comparison.py
  cd ..
  ```

---

### Week 5: Token Economics & Rate-Limit Budgeting
- **Objective**: Develop a pre-implementation budgeting tool to forecast token volumes, monthly hosted API spend, and peak throughput against provider Rate Limits (RPM / TPM).
- **Core Formulas**:
  $$\text{Input Tokens/Day} = R \times I$$
  $$\text{Output Tokens/Day} = R \times O$$
  $$\text{Daily Cost} = \frac{(R \times I \times P_i) + (R \times O \times P_o)}{1{,}000{,}000}$$
  $$\text{Monthly Cost (30-day)} = \text{Daily Cost} \times 30$$
  $$\text{Peak Total TPM (with Margin)} = \text{Peak Requests/Min} \times (I + O) \times \left(1 + \frac{\text{Margin \%}}{100}\right)$$
- **Files**:
  - `budget.py`: Loads configuration from `.env`, applies safety margin, and outputs pass/review checks for RPM, TPM, daily budget, and monthly budget.
  - `test_budget.py`: Automated unit tests validating cost calculations and quota limit boundaries.
- **How to Run**:
  ```powershell
  cd "Week 5"
  # Run the budgeting report:
  ..\myenv\Scripts\python.exe budget.py

  # Run automated test suite:
  ..\myenv\Scripts\python.exe test_budget.py
  cd ..
  ```

---

### Week 6: Golden Question Set Evaluation & Baseline Run (Release v0.1)
- **Objective**: Perform an automated, reproducible baseline evaluation (Release v0.1) of a local model against a curated 30-question Golden Dataset covering Generative AI fundamentals.
- **Architecture**:
  - **Dataset** (`golden_set.json`): 30 standardized questions spanning tokenization, decoding methods (greedy, sampling, top-$k$, top-$p$, temperature), embeddings, positional encodings (sinusoidal, RoPE), and Transformer mechanics.
  - **Scoring Harness** (`main.py`): Queries Ollama model `qwen2.5-coder:14b`, calculates keyword coverage percentage against target ground-truth terms, classifies questions as `PASS` ($\ge 60\%$) or `REVIEW`, and exports records to `baseline_results.csv`.
- **Benchmark Summary (Release v0.1 — `qwen2.5-coder:14b`)**:
  - Total Questions: **30**
  - Successful Queries: **29 / 30**
  - Passed Questions ($\ge 60\%$ keyword coverage): **15**
  - Questions for Review: **14**
  - Average Keyword Coverage: **59.48%**
  - Results Output: `baseline_results.csv`
- **How to Run**:
  *(Ensure Ollama service is running)*
  ```powershell
  cd "Week 6"
  ..\myenv\Scripts\python.exe main.py
  cd ..
  ```

---

## ⚡ Quick Reference Commands

Run any lab directly from the repository root:

```powershell
# Week 1: Gemini API
& ".\myenv\Scripts\python.exe" "Week 1\main.py"

# Week 2: Unified Multi-Provider Client
& ".\myenv\Scripts\python.exe" "Week 2\main.py"

# Week 3: Production Client with MongoDB Telemetry
& ".\myenv\Scripts\python.exe" "Week 3\main.py"
& ".\myenv\Scripts\python.exe" "Week 3\test_client.py"

# Week 4: Tokenizer Comparison
& ".\myenv\Scripts\python.exe" "Week 4\tokenizer_comparison.py"

# Week 5: Token Economics Budgeting & Tests
& ".\myenv\Scripts\python.exe" "Week 5\budget.py"
& ".\myenv\Scripts\python.exe" "Week 5\test_budget.py"

# Week 6: Golden Set Baseline Run
& ".\myenv\Scripts\python.exe" "Week 6\main.py"
```

---

## 🧪 Automated Testing

Both Week 3 and Week 5 include automated unit test suites that verify business logic and edge cases without invoking external paid APIs:

```powershell
# Week 3 Client & Logger Mock Tests
Set-Location "Week 3"; ..\myenv\Scripts\python.exe test_client.py; Set-Location ..

# Week 5 Token Economics Capacity Tests
Set-Location "Week 5"; ..\myenv\Scripts\python.exe test_budget.py; Set-Location ..
```

---

## 🔧 Troubleshooting & Common Issues

1. **`Connection refused` on Ollama (`http://localhost:11434`)**:
   - Ensure the Ollama desktop application is active or run `ollama serve` in a background terminal.
   - Verify installed models with `ollama list`.

2. **`Gemini API key not found`**:
   - Make sure your `.env` file exists in the respective week folder and contains `GEMINI_API_KEY=your_key`.

3. **`MONGODB_URI not found` in Week 3**:
   - Set a valid MongoDB Atlas connection string in `Week 3/.env` (e.g., `MONGODB_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/?retryWrites=true&w=majority`).

4. **Windows Smart App Control / WDAC DLL Notice in Week 4**:
   - Python 3.14 includes an alias workaround in `tokenizer_comparison.py` to route regex operations through Python's built-in `re` module, preventing unsigned binary restriction blocks.
