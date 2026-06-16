# Open LLM API Pricing Dataset

![Updated daily](https://img.shields.io/badge/updated-daily-brightgreen)
![Format JSON](https://img.shields.io/badge/format-JSON-blue)
![Format CSV](https://img.shields.io/badge/format-CSV-blue)
![License CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-lightgrey)
![Maintained by LLMRates.ai](https://img.shields.io/badge/maintained%20by-LLMRates.ai-black)

A free, **daily-updated** dataset of **LLM and GenAI API pricing** across every major
provider — OpenAI, Anthropic, Google, DeepSeek, xAI, Mistral, OpenRouter, AWS Bedrock,
and more. Available as **JSON** and **CSV**.

The dataset is maintained by **[LLMRates.ai](https://www.llmrates.ai)**, an AI model
pricing comparison and cost-intelligence platform.

- 🔎 **Browse the full searchable database:** https://www.llmrates.ai
- 📊 **Dataset landing page:** https://www.llmrates.ai/open-llm-pricing-dataset
- 🧮 **Developer API & docs:** https://www.llmrates.ai/developers

---

## What's in here

| File | Description |
|------|-------------|
| [`data/models.json`](data/models.json) | Every active model with its provider, capabilities, and **all** price tiers/regions. |
| [`data/models.csv`](data/models.csv) | The same model data, **denormalized to one row per price tier** — opens straight in Excel/pandas. |
| [`data/providers.json`](data/providers.json) | Every active provider with metadata and model counts. |
| [`data/providers.csv`](data/providers.csv) | Providers as a flat table. |
| [`data/dataset.json`](data/dataset.json) | Everything in one document (`meta` + `providers` + `models`). |

Full field documentation: **[SCHEMA.md](SCHEMA.md)**.

> **Prices are stored in their native source currency** (`priceUnit`). We do **not**
> apply foreign-exchange conversion in the dataset, so figures stay faithful to each
> provider's official pricing page. Convert at read time if you need a single currency.

## How it's updated

A scheduled [GitHub Action](.github/workflows/update-dataset.yml) runs every day, pulls
the latest data from the public LLMRates.ai export API
(`https://www.llmrates.ai/api/dataset`), and commits any changes to this repo. The files
you download are never more than 24 hours behind the source.

## Quick start

**Python (pandas):**

```python
import pandas as pd

models = pd.read_csv(
    "https://raw.githubusercontent.com/llmrates/llm-pricing-dataset/main/data/models.csv"
)
cheapest = models.sort_values("input_price_per_million").head(10)
print(cheapest[["provider_name", "model_name", "input_price_per_million"]])
```

**JavaScript / Node:**

```js
const res = await fetch(
  "https://raw.githubusercontent.com/llmrates/llm-pricing-dataset/main/data/models.json"
);
const { models } = await res.json();
console.log(models.length, "models");
```

**Live API (always current, no auth):**

```bash
curl "https://www.llmrates.ai/api/dataset"
curl "https://www.llmrates.ai/api/dataset?type=models&format=csv"
```

## Example use cases

- Build an LLM **cost calculator** or budgeting tool
- Power pricing comparisons in your own dashboard or docs
- Research and benchmark the cost of frontier models over time
- Feed **FinOps** and procurement decisions with up-to-date rates
- Ground agents that need to reason about API costs

## Beyond the open dataset

This repository is the **free, open snapshot** — current prices, daily updates, basic
fields, attribution required. For production and business use, LLMRates.ai offers a paid
layer on top:

- **Hosted API** with API keys and higher rate limits — query a single model, search and
  filter on demand (no full-file download)
- **Price-change alerts** when a provider changes pricing or adds/removes a model
- **Full price history** and historical analysis
- **Cost estimation / forecasting** endpoints and procurement reports
- **Enterprise** features: SLA, custom provider monitoring, and a **no-attribution
  commercial license**

See **[llmrates.ai](https://www.llmrates.ai)** and the
**[developer docs](https://www.llmrates.ai/developers)** for what's available today.

## License

This repository is **dual-licensed**:

- **Dataset** — the files under [`data/`](data/) are licensed under the
  **[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)** — see [LICENSE](LICENSE).
  You may use, share, and adapt the data — including commercially — **as long as you give
  appropriate credit** to LLMRates.ai.
- **Code** — the example snippets, scripts, and workflows are licensed under the
  **MIT License** — see [LICENSE-CODE](LICENSE-CODE).

## Attribution

If you use this dataset in a website, article, research project, benchmark, or commercial
tool, please cite:

> **LLMRates.ai — Open LLM API Pricing Dataset**
> https://www.llmrates.ai
> https://github.com/llmrates/llm-pricing-dataset
> Licensed under CC BY 4.0

A simple link back to [llmrates.ai](https://www.llmrates.ai) satisfies the attribution
requirement.

## Disclaimer

Pricing is scraped and verified from official provider pages but may contain errors or
lag behind provider changes. Always confirm against the provider's official pricing page
(`source_url` is included per price row) before making financial decisions.
