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
## Why Ollama?

We deliberately chose **Ollama** as the LLM runtime because:  
-  **Local & lightweight** → runs fully on the developer’s laptop, no need for external cloud APIs.  
-  **Privacy & control** → sensitive invoice data never leaves the machine.  
-  **Modular & flexible** → switching to a different open-source model (`phi3:mini`, `llama3`, `qwen2.5`, etc.) is as simple as changing the `model` parameter.  
-  **Cost-efficient** → no API keys or pay-per-token costs, ideal for early-stage automation prototypes.  

This aligns with Tech9’s vision of **agentic automation**: solutions that are adaptive, resilient, and do not rely on external dependencies to prove value.  

---

## Native UiPath Implementation

All logic was built **directly in UiPath Studio (C#, Windows project)** using only official UiPath activities:  
- **PDF & OCR Activities** → to extract raw text from invoices.  
- **WebAPI Activities** → to call the Ollama R

## Future Work & Business Flexibility

- **REF Integration**  
  This prototype was intentionally kept simple (single `Main.xaml`) to highlight the OCR + LLM logic.  
  In a production-grade deployment, the process should be migrated into the **Robotic Enterprise Framework (REF)**:  
  - Transactions managed through Orchestrator Queues.  
  - Built-in exception handling and retries.  
  - Logging, business rule exceptions, and resilience at scale.  

- **Business Flexibility**  
  The design was created to keep the **business logic adaptable**:  
  - Confidence thresholds can be tuned by stakeholders.  
  - Field mappings and JSON schema can evolve with client needs.  
  - Alternative LLMs or OCR engines can be swapped with minimal changes.  
  - Rule sets (tax validation, subtotal/total checks, etc.) can be parameterized per client or per industry.  

This ensures that the solution is not only technically robust but also **aligned with real business priorities** — allowing Tech9 to deliver automation that evolves alongside the customer’s growth.



## Setup
1) **Ollama**
```bash
# on host (Mac/Windows)
ollama pull phi3:mini
# expose to LAN (if calling from VM)
OLLAMA_HOST=0.0.0.0:11434 ollama serve




