# AI-First Supply Chain Analytics — Programmatic Ads POC

A pilot project demonstrating AI-first analytics on programmatic advertising supply chain data using AWS Bedrock (Claude), FAISS vector DB, and RAG.

## What This Does

Analyzes the programmatic ad supply chain funnel end-to-end:
```
Ad Request → Punt → Bid Request → No-Bid → Bid → Impression Served
```

Uses LLM (Claude via AWS Bedrock) to generate plain English insights and answer questions about supply chain health.

## Project Phases

| Phase | Description | Status |
|-------|-------------|--------|
| 1 | LLM Analytics — funnel analysis + Bedrock insights | ✅ Done |
| 2 | Dynamic prompts + evaluation (cost, latency, faithfulness) | ✅ Done |
| 3 | RAG + Vector DB (FAISS) + eval results saved to S3 | ✅ Done |
| 4 | MCP + Athena — natural language to SQL | 🔨 In Progress |
| 5 | Full evaluation dashboard | Planned |
| 6 | Agentic analytics — autonomous monitoring | Planned |

## Data Sources

Four programmatic ad log files (S3, CSV format):
- `bid_log` — auctions where DSPs submitted bids
- `impression_log` — successfully served impressions
- `no_bid_log` — auctions where DSPs declined to bid
- `punt_log` — requests blocked by SSP before reaching DSPs

## Tech Stack

- **Python 3.13** with pandas, boto3
- **AWS Bedrock** — Claude Haiku for LLM inference
- **AWS S3** — data storage + eval results
- **FAISS** — local vector database
- **sentence-transformers** — text embeddings (all-MiniLM-L6-v2)

## Setup

```bash
# Clone the repo
git clone <your-repo-url>
cd <repo-folder>

# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Set up credentials
cp .env.example .env
# Edit .env with your AWS credentials
```

## Environment Variables

Create a `.env` file (never commit this):
```
AWS_ACCESS_KEY_ID=your_key_here
AWS_SECRET_ACCESS_KEY=your_secret_here
AWS_REGION=us-east-1
AWS_S3_BUCKET_NAME=your_bucket_name
```

## Running the Notebook

```bash
source venv/bin/activate
jupyter notebook
# Open supply_funnel_analysis.ipynb
```

## Key Findings (Sample Data)

```
Total Ad Requests:     73,000
Punted by SSP:          8,000  (11%)  — top reason: TIMEOUT (35.7%)
No-Bid from DSPs:      15,000  (20.5%) — top reason: DSP_NO_RESPONSE (30.1%)
Impressions Served:    33,960  (46.5%)
```

## Semantic Layer

`semanticlayer.yaml` defines all tables, columns, metrics and domain context.
Used as the knowledge base for RAG-based Q&A.
