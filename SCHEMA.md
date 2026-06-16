# Dataset Schema

All money values are **per 1,000,000 tokens** unless the field name says otherwise
(e.g. `imagePrice` is per image, `pagePrice` is per 1,000 pages). Prices are in the
**native source currency** given by `priceUnit` — no FX conversion is applied.

## `dataset.json`

```jsonc
{
  "meta": {
    "name": "Open LLM API Pricing Dataset",
    "publisher": "LLMRates.ai",
    "source": "https://www.llmrates.ai",
    "datasetUrl": "https://www.llmrates.ai/open-llm-pricing-dataset",
    "repoUrl": "https://github.com/llmrates/llm-pricing-dataset",
    "license": "CC-BY-4.0",
    "licenseUrl": "https://creativecommons.org/licenses/by/4.0/",
    "attribution": "Data from LLMRates.ai ...",
    "note": "Prices are in their native currency (priceUnit); no FX conversion.",
    "modelCount": 0,
    "providerCount": 0,
    "priceRowCount": 0
  },
  "providers": [ /* Provider[] */ ],
  "models": [ /* Model[] */ ]
}
```

## Provider

| Field | Type | Notes |
|-------|------|-------|
| `id` | number | Stable internal id |
| `slug` | string | URL slug (e.g. `openai`) |
| `name` | string | Display name |
| `nameLocal` | string \| null | Native-language name (e.g. for Chinese providers) |
| `website` | string | Provider homepage |
| `pricingUrl` | string \| null | Official pricing page |
| `providerType` | string | `direct` \| `aggregator` \| `cloud` |
| `description` | string \| null | Short description |
| `modelCount` | number | Active models from this provider |

## Model

| Field | Type | Notes |
|-------|------|-------|
| `id` | number | Stable internal id |
| `sid` | string | Stable short id |
| `name` | string | Model display name |
| `slug` | string | URL slug |
| `family` | string \| null | Model family (e.g. `gpt-4`) |
| `modelType` | string | e.g. `general`, `reasoning`, `image`, `audio` |
| `contextWindow` | number \| null | Max context, in tokens |
| `maxOutput` | number \| null | Max output, in tokens |
| `supportsTools` | boolean | Function/tool calling |
| `supportsBatch` | boolean | Batch API |
| `supportsCaching` | boolean | Prompt caching |
| `supportsStreaming` | boolean | Streaming responses |
| `releaseDate` | string \| null | ISO 8601 |
| `knowledgeCutoff` | string \| null | ISO 8601 |
| `deprecatedAt` | string \| null | ISO 8601 |
| `provider` | object | `{ name, slug, providerType }` |
| `modalities` | string[] | e.g. `["text", "image"]` |
| `prices` | Price[] | Every tier / region / processing mode |

## Price

| Field | Type | Notes |
|-------|------|-------|
| `inputPricePerMillion` | number \| null | Prompt tokens |
| `cachedInputPricePerMillion` | number \| null | Cached prompt tokens |
| `outputPricePerMillion` | number \| null | Completion tokens |
| `thinkingOutputPricePerMillion` | number \| null | Reasoning tokens |
| `cachedWritePricePerMillion` | number \| null | Cache-write tokens |
| `imagePrice` | number \| null | Per image |
| `imagePricePerMillion` | number \| null | Per 1M image tokens |
| `characterPricePerMillion` | number \| null | Per 1M characters (TTS) |
| `audioPricePerHour` | number \| null | Per hour (STT) |
| `audioPricePerMillion` | number \| null | Per 1M audio tokens |
| `videoPrice` | number \| null | Per video |
| `videoPricePerMillion` | number \| null | Per 1M video tokens |
| `trackPrice` | number \| null | Per audio track |
| `pagePrice` | number \| null | Per 1,000 pages (OCR/doc) |
| `searchPricePerThousand` | number \| null | Per 1,000 searches |
| `processingTier` | string | `standard` \| `batch` \| `fast` |
| `tokenTierMin` | number \| null | Lower bound of a usage tier |
| `tokenTierMax` | number \| null | Upper bound of a usage tier |
| `tierLabel` | string \| null | Human label for the tier |
| `priceUnit` | string | Source currency (e.g. `USD`, `CNY`) |
| `freeTier` | string \| null | Free-tier note, if any |
| `region` | string \| null | Region the price applies to |
| `sourceUrl` | string \| null | Official page this was sourced from |

> The open dataset contains **current prices only**. Full price history, verification
> timestamps, and effective dates are reserved for the [LLMRates.ai](https://www.llmrates.ai)
> paid API.

## CSV files

- **`models.csv`** is denormalized: **one row per price row**, with the model and
  provider columns repeated. Column headers are `snake_case`
  (`provider_slug`, `model_name`, `input_price_per_million`, …). `modalities` is a
  pipe-joined string (e.g. `text|image`). Models with no price still emit one row with
  empty price columns.
- **`providers.csv`** is one row per provider.
