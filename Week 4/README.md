# Week 4: Tokenizer Lab — Subword Tokenization Comparison

## Overview
Compares three foundational subword tokenization paradigms across three distinct languages (English, Telugu, and Hindi):
1. **Byte Pair Encoding (BPE)** via `gpt2`
2. **WordPiece** via `bert-base-multilingual-cased`
3. **SentencePiece** via `xlm-roberta-base`

## Objectives
- Observe how words are partitioned into subword units vs complete words.
- Compare token pieces and numeric token IDs.
- Demonstrate why token counts differ significantly across tokenizer vocabularies and training datasets (e.g. byte-level splitting in Indic scripts).

## How to Run
From repository root:
```powershell
& ".\myenv\Scripts\python.exe" "Week 4\tokenizer_comparison.py"
```
Or from inside `Week 4`:
```powershell
..\myenv\Scripts\python.exe tokenizer_comparison.py
```

## Comparative Results

| Language | Input Text | BPE (GPT-2) | WordPiece (mBERT) | SentencePiece (XLM-R) |
|---|---|:---:|:---:|:---:|
| **English** | `Artificial intelligence is changing the world.` | **8 tokens** | 10 tokens | 10 tokens |
| **Telugu** | `కృత్రిమ మేధస్సు ప్రపంచాన్ని మారుస్తోంది.` | **112 tokens** | 18 tokens | **14 tokens** |
| **Hindi** | `कृत्रिम बुद्धिमत्ता दुनिया को बदल रही है।` | **68 tokens** | 17 tokens | **14 tokens** |

### Key Observation
GPT-2 was primarily trained on English text and falls back to raw UTF-8 byte sequences for unsupported characters, causing significant token inflation for Telugu (112 tokens) and Hindi (68 tokens). In contrast, multilingual tokenizers (mBERT and XLM-R) maintain dedicated Indic subword vocabularies, producing far more efficient token representations (14–18 tokens).
