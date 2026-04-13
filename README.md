
# Pangolinfo Scraper Skill for OpenClaw

<div align="center">

![Pangolinfo Scraper Banner](assets/banner.png)

**Give your AI Agent real-time vision.**
Scrape Amazon, Walmart, and Google Search results as clean JSON.
No CAPTCHAs. No IP bans. Just data.

[Website](https://www.pangolinfo.com) • [Documentation](https://docs.pangolinfo.com) • [Get API Key](https://tool.pangolinfo.com/)

</div>

---

## 🚀 Features

*   **⚡ Real-Time Data**: Fetch live pricing, stock status, and rankings in milliseconds.
*   **🧠 LLM-Ready**: Returns structured JSON that OpenClaw, ChatGPT, and Claude can understand instantly.
*   **🛡️ Unblockable**: Built-in residential proxy rotation and browser fingerprinting.
*   **🔌 Plug & Play**: Designed specifically as an **OpenClaw Skill**.

## 📦 Installation

This skill is designed for [OpenClaw](https://github.com/openclaw/openclaw).

1.  Clone this repository into your skills directory:
    ```bash
    git clone https://github.com/Pangolin-spg/openclaw-skill-pangolinfo.git skills/pangolinfo
    ```

2.  Configure your API Key (no `pip install` needed -- zero dependencies):
    ```bash
    export PANGOLIN_API_KEY="your_api_key_here"
    ```

## 🛠️ Usage

### Within OpenClaw

Just ask your agent!

> "Check the current price of Sony WH-1000XM5 on Amazon US."

> "What are the top 5 organic results for 'AI Agent' on Google?"

### Manual CLI Usage

You can also run the scraper scripts directly (zero dependencies, Python 3.8+ stdlib only):

```bash
# Google AI Mode search (default, 2 credits)
python scripts/pangolin_serp.py --q "what is quantum computing"

# Google standard SERP (2 credits)
python scripts/pangolin_serp.py --q "best laptops 2025" --mode serp

# Google SERP Plus (cheaper, 1 credit)
python scripts/pangolin_serp.py --q "best laptops 2025" --mode serp-plus

# Amazon product detail by ASIN
python scripts/pangolin_amazon.py --asin B0DYTF8L2W --site amz_us

# Amazon keyword search
python scripts/pangolin_amazon.py --q "wireless mouse" --site amz_us

# Amazon product reviews (critical only)
python scripts/pangolin_amazon.py --content B00163U4LK --mode review --filter-star critical

# Amazon bestsellers
python scripts/pangolin_amazon.py --content "electronics" --parser amzBestSellers --site amz_us
```

## 🧩 Supported Platforms

| Platform | Features | Script | Parsers |
| :--- | :--- | :--- | :--- |
| **Google** | AI Mode, SERP, SERP Plus, AI Overviews, Screenshots | `pangolin_serp.py` | `googleAiSearch`, `googleSearch`, `googleSearchPlus` |
| **Amazon** | Products, Search, Reviews, BSR, Bestsellers, Variants, 13 regions | `pangolin_amazon.py` | `amzProductDetail`, `amzKeyword`, `amzBestSellers`, `amzReviewV2`, + 5 more |

## 📄 Example Response

### Google AI Mode (pangolin_serp.py)

```json
{
  "success": true,
  "task_id": "1768988520324-766a695d93b57aad",
  "results_num": 1,
  "ai_overview_count": 1,
  "ai_overview": [
    {
      "content": ["Quantum computing uses quantum bits (qubits)..."],
      "references": [
        {
          "title": "Quantum Computing - Wikipedia",
          "url": "https://en.wikipedia.org/wiki/Quantum_computing",
          "domain": "Wikipedia"
        }
      ]
    }
  ]
}
```

### Amazon Product Detail (pangolin_amazon.py)

```json
{
  "success": true,
  "task_id": "02b3e90810f0450ca6d41244d6009d0f",
  "url": "https://www.amazon.com/dp/B0DYTF8L2W",
  "metadata": {
    "executionTime": 1791,
    "parserType": "amzProductDetail",
    "parsedAt": "2026-01-13T06:42:01.861Z"
  },
  "results": [ ... ],
  "results_count": 1
}
```

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

## 📄 License

[MIT](LICENSE) © [Pangolinfo](https://www.pangolinfo.com)
