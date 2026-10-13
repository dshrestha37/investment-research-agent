# AAI 520 — Investment Research Agent

Autonomous multi-agent system that takes one equity ticker and produces a cited analyst note.  
Built for the USD MS Applied AI final team project (AAI 520).

**GitHub:** https://github.com/dshrestha37/investment-research-agent

## Team roles

- **Deepika Shrestha** — Agent core: plan, tools, prompt chain, router, evaluator–optimizer, memory.
- **Christian Lopez** — News ingestion: Yahoo headlines, category tagging, FAISS side index.
- **Cameron Aljilani** — Integration of both notebooks, bug fixes, news relevance filters, Colab wrapper, and executed runs.

## What the agent does

1. **Plans** research steps for a given ticker.
2. **Uses tools** dynamically (Yahoo Finance prices/fundamentals, NewsAPI or Yahoo news, FRED macro, SEC EDGAR, memory recall).
3. **Self-reflects** on the quality of the note.
4. **Learns across runs** (JSON memory of prior metrics and lessons).

**Workflow patterns**
- Prompt chaining: ingest → preprocess → classify → extract → summarize (news).
- Routing: articles and tools directed to market, earnings, or news specialists.
- Evaluator–optimizer: draft → score → refine.

## How to run

1. Upload `AAI520_Final_Investment_Research_Agent_Colab.ipynb` to Google Colab.
2. Add secrets (Colab sidebar, key icon):
   - `GEMINI_API_KEY` (required for LLM mode; rule fallback still works without it)
   - `NEWSAPI_KEY` (optional)
   - `FRED_API_KEY` (optional)
   - `SEC_USER_AGENT` (contact string, e.g. `AAI520-IRA you@school.edu`)
3. Runtime → Run all.
4. Ticker is set in one cell only (`TICKER = "XXX"`). Change it and re-run; nothing else is hardcoded. Examples are `AAPL`, `META`, `DIS`, `NVDA`, `AMZN`, `WMT`.
5. Download the executed notebook (File → Download → .ipynb) and export PDF or HTML for Canvas submission.

Offline tests (`python test_pipeline.py`) pass with no keys or network.

## Repository layout

```
├── AAI520_Final_Investment_Research_Agent_Colab.ipynb   # submission notebook
├── ira_modules/                                         # same modules the notebook writes
│   ├── schema.py
│   ├── market_tools.py
│   ├── news_tools.py
│   ├── extra_tools.py
│   ├── llm.py
│   ├── memory.py
│   ├── chain.py
│   ├── router.py
│   ├── evaluator.py
│   ├── research_agent.py
│   ├── news_index.py
│   └── test_pipeline.py
├── IRA_README.md                                        # short index (this file expands it)
└── README.md
```

## Key fixes and improvements

- Router phrase bug fixed (“trading between of its averages” → complete phrases).
- Claude model id corrected; Gemini is the path used.
- Empty news returns `ToolResult.failure` (synthetic fallback article removed).
- News relevance filters added (generic financial keywords; works for any ticker).
- Memory path is `ira_state/memory.json`; second run shows changes since the prior run.
- Citations are only `[S#]` and `[N#]`. No BUY/SELL line.

## AI disclosure

AI-assisted tools (Gemini via API, and coding assistants for integration/review) were used to accelerate development, debugging, and documentation. All agent logic, tool contracts, workflow patterns, and final notebook content were reviewed, modified, and executed by the team. Outputs are grounded in the fetched data; no citations or metrics are invented.

## Requirements

Python 3.10+, packages listed in the notebook install cell (`yfinance`, `google-genai`, `langchain-*`, `sentence-transformers`, `faiss-cpu`, etc.). Colab is the intended runtime.
