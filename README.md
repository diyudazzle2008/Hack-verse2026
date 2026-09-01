# Hack-verse2026
# 📊 AI Investment Intelligence System

**HackVerse: Into the Web — Sprint 1 (24-Hour Hackathon)**
**IEEE Robotics & Automation Society, VIT Chennai Student Chapter**

## Problem Statement
PS-01: Multi-Agent Autonomous Financial Intelligence System for Retail Investors

India's retail investors have access to public market data but lack the
infrastructure to turn it into personalized, explainable investment guidance.
This project builds a multi-agent AI system that reasons over price momentum,
market sentiment, and regulatory filings — then synthesizes a transparent,
risk-adjusted recommendation for each user.

## What Makes This Unique
Most multi-agent systems silently average their agents' outputs. Ours doesn't.

When our agents disagree — e.g. price momentum signals "bullish" while news
sentiment signals "bearish" — the system surfaces that conflict explicitly
and shows how the Synthesis Agent resolves it based on the specific user's
risk profile. Judges (and users) see the AI's internal debate, not just its
final answer.

## Architecture

### Agents
| Agent | Role | Input |
|---|---|---|
| **Momentum Agent** | Analyzes price trend & volume signals | Price/volume summary |
| **Sentiment Agent** | Classifies news sentiment | Recent headlines |
| **RAG Agent** | Grounds analysis in company filings, with citation | Vector-retrieved filing excerpts |
| **Synthesis Agent** | Resolves agent disagreement, personalizes for user risk profile | Outputs of all 3 agents + user profile |

Each agent runs independently and returns structured JSON:
`{signal, confidence, reasoning, ...}`. The Synthesis Agent explicitly
flags whether agents agreed, summarizes any conflict, and explains its
final call relative to the user's risk tolerance.

### Data
- **Price data**: Historical price/volume signals for 3 stocks (Reliance,
  HDFC Bank, Zomato)
- **Filings**: Synthetic quarterly earnings summaries per company, used for
  retrieval-augmented generation
- **Headlines**: Synthetic news headlines per company, used for sentiment
  analysis
- **User profiles**: Two test users (conservative vs. aggressive risk
  tolerance) with distinct holdings, stored in SQLite

### Tech Stack
- **LLM**: Google Gemini (`gemini-3.5-flash-lite`)
- **Vector DB**: ChromaDB (semantic retrieval over filings)
- **Dashboard**: Streamlit
- **Database**: SQLite (user profiles, session logs)
- **Deployment**: Streamlit Community Cloud

## Key Features (Minimum Requirements Coverage)
- ✅ Signal classification across 3 independent dimensions (momentum, sentiment, fundamentals) with stated confidence
- ✅ RAG component grounding output in filing excerpts, with source attribution shown in UI
- ✅ 3 specialized agents with structured output, consumed by a synthesis layer
- ✅ User profiling that demonstrably changes output for different risk profiles on identical market data
- ✅ Live interface showing signals, synthesized recommendation with sources, and user context
- ✅ Session logging (latency, confidence, risk profile)
- ✅ Full end-to-end demo: raw data → multi-agent reasoning → recommendation, with reasoning chain visible
- ✅ Graceful degraded-data handling — simulated data outage toggle, synthesis agent explicitly notes reduced confidence rather than guessing

## How to Run Locally
```bash
pip install -r requirements.txt
streamlit run app.py
