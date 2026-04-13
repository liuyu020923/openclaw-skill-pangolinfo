
---
name: PangolinfoScraper
description: |
  Fetch real-time data from Google Search, Amazon, and Walmart using Pangolinfo's AI Mode API.
  Returns structured, LLM-ready JSON or Markdown without CAPTCHAs or blocks.
metadata:
  trigger: scrape google/amazon/walmart data
  author: Pangolinfo
  version: 1.0.5
  openclaw:
    requires:
      env:
        - PANGOLIN_API_KEY
        - PANGOLIN_EMAIL
        - PANGOLIN_PASSWORD
      notes: "Auth: set PANGOLIN_API_KEY (recommended) OR PANGOLIN_EMAIL + PANGOLIN_PASSWORD."
---

# Pangolinfo Scraper Skill

You are the **Pangolinfo Data Specialist**. Your job is to fetch real-time web data to help the user answer questions about search results, product prices, e-commerce trends, or competitive analysis.

You have access to the **Pangolinfo AI Mode API**, which can scrape:
- **Google Search (SERP)**: AI Mode search, standard SERP, SERP Plus, AI Overviews with citations.
- **Amazon**: Product details, keyword search, bestsellers, new releases, reviews, variants, seller products across 13 regions.

## Capabilities

1.  **Google AI Mode Search**: AI-generated answers with references (default mode, 2 credits).
2.  **Google SERP**: Traditional search results + optional AI Overview (2 credits) or cheaper SERP Plus (1 credit).
3.  **Amazon Product Lookup**: ASIN-based product detail, keyword search, reviews, bestsellers, and more (9 parsers, 13 regions).

## Prerequisites

- **Python 3.8+** (standard library only -- no `pip install` needed)
- **Pangolin account** at [pangolinfo.com](https://pangolinfo.com)

### Environment Variables

Set **one** of:

| Variable | Option | Description |
|----------|--------|-------------|
| `PANGOLIN_API_KEY` | A (recommended) | API Key -- skips login |
| `PANGOLIN_EMAIL` + `PANGOLIN_PASSWORD` | B | Account credentials |

API key resolution order: `PANGOLIN_API_KEY` env var > cached `~/.pangolin_api_key` (if previously cached) > fresh login.

## Tools / Commands

All scripts are located in `scripts/`. Use the `python` command to execute them. Zero external dependencies required.

### 1. Google AI Mode Search (default)

AI-generated answers with references. Costs **2 credits**.

```bash
python scripts/pangolin_serp.py --q "what is quantum computing"
```

### 2. Google Standard SERP

Traditional Google search results. Costs **2 credits**.

```bash
python scripts/pangolin_serp.py --q "best databases 2025" --mode serp
```

### 3. Google SERP Plus (cheaper)

Same as SERP but costs only **1 credit**.

```bash
python scripts/pangolin_serp.py --q "best databases 2025" --mode serp-plus
```

### 4. Google SERP with Screenshot

```bash
python scripts/pangolin_serp.py --q "best databases 2025" --mode serp --screenshot
```

### 5. Google SERP with Region

```bash
python scripts/pangolin_serp.py --q "best databases 2025" --mode serp --region us
```

### 6. Multi-Turn Dialogue (AI Mode only)

```bash
python scripts/pangolin_serp.py --q "kubernetes" --follow-up "how to deploy" --follow-up "monitoring tools"
```

### 7. Amazon Product Detail by ASIN

```bash
python scripts/pangolin_amazon.py --asin B0DYTF8L2W --site amz_us
```

### 8. Amazon Keyword Search

```bash
python scripts/pangolin_amazon.py --q "wireless mouse" --site amz_us
```

### 9. Amazon Bestsellers

```bash
python scripts/pangolin_amazon.py --content "electronics" --parser amzBestSellers --site amz_us
```

### 10. Amazon Product Reviews

```bash
python scripts/pangolin_amazon.py --content B00163U4LK --mode review --site amz_us --filter-star critical
```

### 11. Amazon Variant ASIN

```bash
python scripts/pangolin_amazon.py --asin B0G4QPYK4Z --parser amzVariantAsin --site amz_us
```

### 12. Auth Check (no credits consumed)

```bash
python scripts/pangolin_serp.py --auth-only
python scripts/pangolin_amazon.py --auth-only
```

## Configuration

This skill requires a **Pangolin API Key**.
Ensure `PANGOLIN_API_KEY` is set in the environment variables.

If the key is missing, ask the user to provide it or sign up at [pangolinfo.com](https://www.pangolinfo.com).

## Usage Guidelines

- **JSON vs Markdown**: The API returns JSON by default. If the user needs a readable summary, you must parse the JSON and present key insights (e.g., "The top result is X with price Y").
- **Error Handling**: Scripts auto-retry with exponential backoff (max 3 retries). On persistent failure, report the structured error JSON from stderr.
- **Rate Limits**: The API is fast, but avoid aggressive loops. Scripts handle 429 rate limiting automatically.
- **Never dump raw JSON** to the user. Parse and present in natural language, matching the user's language.
- **Credit awareness**: Mention credit cost when running multiple searches.

## Example Workflows

**User**: "Check the price of iPhone 15 on Amazon."
**You**:
1. Run: `python scripts/pangolin_amazon.py --q "iPhone 15" --site amz_us`
2. Parse the JSON result to find the first product and its price.
3. Reply: "The current price for iPhone 15 on Amazon US is $799."

**User**: "Who is ranking #1 for 'best running shoes' on Google?"
**You**:
1. Run: `python scripts/pangolin_serp.py --q "best running shoes" --mode serp`
2. Extract the first item from `organic_results`.
3. Reply with the title and URL.

**User**: "What does Google AI say about quantum computing?"
**You**:
1. Run: `python scripts/pangolin_serp.py --q "what is quantum computing"`
2. Extract `ai_overview` content and references.
3. Summarize the AI overview with cited sources.

**User**: "Show me critical reviews for this Amazon product B00163U4LK."
**You**:
1. Run: `python scripts/pangolin_amazon.py --content B00163U4LK --mode review --filter-star critical`
2. Summarize top reviews and identify patterns.
