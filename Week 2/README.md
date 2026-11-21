# Week 2: Unified Multi-Provider LLM Client Architecture

## Overview
Implements the **Strategy Design Pattern** in Python to provide an abstracted interface (`LLMProvider`) supporting both hosted cloud models (Google Gemini) and offline local models (Ollama).

## Architecture
- `LLMProvider` (ABC): Defines the abstract method `generate(prompt)`.
- `GeminiProvider`: Implements Gemini API calls (`gemini-3.5-flash`).
- `OllamaProvider`: Implements REST API calls to local Ollama service (`http://localhost:11434/api/generate`) using `qwen2.5-coder:14b`.
- `LLMClient`: High-level wrapper providing a uniform call signature.

## Prerequisites
- For Gemini: `GEMINI_API_KEY` configured in `.env`.
- For Ollama: Ollama service running locally with model pulled (`ollama pull qwen2.5-coder:14b`).

## How to Run
From repository root:
```powershell
& ".\myenv\Scripts\python.exe" "Week 2\main.py"
```
Or from inside `Week 2`:
```powershell
..\myenv\Scripts\python.exe main.py
```

## Interactive Usage
1. Enter `1` for Gemini AI (Online) or `2` for Qwen2.5-Coder 14B (Local).
2. Enter your custom question/prompt when prompted.
3. The response from the selected provider will be displayed.
