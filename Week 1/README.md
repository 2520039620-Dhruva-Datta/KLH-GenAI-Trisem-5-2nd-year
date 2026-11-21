# Week 1: First LLM API Call with Google Gemini

## Overview
Demonstrates how to connect to Google's hosted Gemini API using the official `google-genai` Python SDK and perform content generation with the `gemini-3.5-flash-lite` model.

## Prerequisites
- Python virtual environment (`myenv`)
- Valid Google Gemini API Key

## Setup
Create or verify `.env` in this directory:
```env
GEMINI_API_KEY=your_actual_gemini_api_key_here
```

## How to Run
From repository root:
```powershell
& ".\myenv\Scripts\python.exe" "Week 1\main.py"
```
Or from inside `Week 1`:
```powershell
..\myenv\Scripts\python.exe main.py
```

## Expected Output
The program prints the generated explanation of Artificial Intelligence directly to the console:
```text
===== LLM Response =====
Artificial Intelligence (AI) refers to the simulation of human intelligence in machines...
```
