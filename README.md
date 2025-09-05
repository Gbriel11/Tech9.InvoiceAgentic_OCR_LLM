# Tech9.InvoiceAgentic_OCR_LLM
This repo demonstrates an UIpath+ OCR + LLM (Ollama) pipeline for intelligent invoice automation.
# Tech9.InvoiceAgentic — OCR + LLM (Ollama) Invoice Agent

This project demonstrates an **OCR + LLM pipeline** for intelligent invoice processing using **UiPath (C#)** and **Ollama** (local LLM). It reads PDF invoices (native or scanned), extracts text, prompts a local model to return a **strict JSON**, validates totals/confidence, and exports results to **CSV + per-invoice JSON**.

## Why this matters (Tech9 context)
- Early-stage back-offices need **adaptive automation**, not brittle scripts.
- This prototype shows how to evolve RPA into **agentic** behavior: fallback paths, confidence scoring, and business checks that scale.

## Architecture (high level)
1. Read PDF → text; fallback to OCR (Tesseract).
2. Build prompt (template + OCR text).
3. Call local LLM (Ollama `phi3:mini`) → JSON fields.
4. Persist outputs (CSV aggregate + JSON per file).

## Repo layout
- `/uipath` — UiPath project (`Main.xaml`, `project.json`)
- `/Prompts` — prompt templates
- `/Data/Input` — sample PDFs (small set for testing)
- `/Data/Output` — generated outputs (empty in repo)
- `/docs` — approach, demo script, troubleshooting

## Requirements
- UiPath Studio 25.x, **Windows** compatibility, **C#**
- Packages: UiPath.System, UiPath.PDF, UiPath.OCR, UiPath.WebAPI, UiPath.Excel
- .NET 6 Desktop Runtime
- **Ollama** running on the host with `phi3:mini` pulled

## Setup
1) **Ollama**
```bash
# on host (Mac/Windows)
ollama pull phi3:mini
# expose to LAN (if calling from VM)
OLLAMA_HOST=0.0.0.0:11434 ollama serve
