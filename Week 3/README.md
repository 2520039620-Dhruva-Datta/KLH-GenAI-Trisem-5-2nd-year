# Week 3: Production LLM Client with Telemetry, Cost Calculation & MongoDB Logging

## Overview
An enterprise-grade LLM client framework incorporating full observability:
- **Telemetry**: Wall-clock latency (ms), prompt token count, completion token count.
- **Cost Engine**: Dynamic USD billing calculation per query for hosted models.
- **Resilience**: Exponential backoff retries on transient errors (timeouts, rate limits, 5xx server errors).
- **Persistent Storage**: MongoDB telemetry audit logs stored in `llm_monitoring.api_usage`.
- **Unit Testing**: Isolated unit test suite (`test_client.py`) with mock providers and loggers.

## Environment Variables (.env)
```env
GEMINI_API_KEY=your_gemini_api_key
MONGODB_URI=your_mongodb_connection_string
GEMINI_INPUT_PRICE_PER_MILLION=0.15
GEMINI_OUTPUT_PRICE_PER_MILLION=0.60
```

## How to Run

### Interactive Application
From repository root:
```powershell
& ".\myenv\Scripts\python.exe" "Week 3\main.py"
```
Or from inside `Week 3`:
```powershell
..\myenv\Scripts\python.exe main.py
```

### Automated Unit Tests
Verify token calculations and client logging without invoking external paid APIs:
```powershell
cd "Week 3"
..\myenv\Scripts\python.exe test_client.py
cd ..
```

## Expected Console Output
```text
Choose provider (gemini/ollama): ollama
Enter your question: What is AI?
Latency: 1250.45 ms
Input tokens: 14
Output tokens: 48
Estimated cost (USD): 0.0
Usage details saved to MongoDB.

LLM Response:
Artificial Intelligence refers to...
```
