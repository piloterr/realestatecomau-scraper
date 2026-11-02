<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="realestate.com.au Scraper API by Piloterr – scrape realestate.com.au data as JSON" width="100%"></a>
</p>

# realestate.com.au Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr realestate.com.au Scraper API** lets you extract public data from realestate.com.au as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **realestate.com.au Search API** — Search realestate.com.au buy, rent, or sold results.

Use it for property price monitoring, market analytics, investment sourcing or rental yield models.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [realestate.com.au Search API](#realestatecomau-search-api) | `/v2/realestatecomau/search` | 2 | Search realestate.com.au buy, rent, or sold results. A place such as Sydney, NSW defaults to homes for sale. Each card has price, address, rooms, and a property URL. |

## Table of contents

- [How it works](#how-it-works)
- [realestate.com.au Search API](#realestatecomau-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## realestate.com.au Search API

Search realestate.com.au buy, rent, or sold results. A place such as Sydney, NSW defaults to homes for sale. Each card has price, address, rooms, and a property URL.

`GET | POST https://api.piloterr.com/v2/realestatecomau/search` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Place or a realestate.com.au results URL. |
| `channel` | string | — | buy, rent, or sold. Used when query is a place. |
| `page` | number | — | Page from 1. |
| `return_page_source` | boolean | — | Optional. Default false. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/realestatecomau/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "Sydney, NSW",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type RealestateComAuSearch = {
  results?: {
    id?: number;
    url?: string;
    badge?: unknown;
    image?: string;
    price?: number;
    state?: string;
    suburb?: string;
    address?: string;
    channel?: string;
    bedrooms?: number;
    currency?: string;
    postcode?: string;
    bathrooms?: number;
    price_max?: unknown;
    agent_name?: string;
    car_spaces?: number;
    agency_name?: string;
    building_size?: number;
    price_display?: string;
    property_type?: string;
    building_size_display?: string;
  }[];
  pagination?: {
    next?: string;
    page?: number;
    per_page?: number;
    total_pages?: number;
    total_results?: number;
  };
};

const params = new URLSearchParams({
  "query": "Sydney, NSW",
});

const response = await fetch(`https://api.piloterr.com/v2/realestatecomau/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as RealestateComAuSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/realestatecomau/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "Sydney, NSW"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "results": [
    {
      "id": 149690532,
      "url": "https://www.realestate.com.au/property-apartment-nsw-sydney-149690532",
      "badge": null,
      "image": "https://i2.au.reastatic.net/800x600/0912e9a1bea76a94ba5a397b993767175e6714ec2a59081478ee67d3f622b4cb/image.jpg",
      "price": 4000000,
      "state": "NSW",
      "suburb": "Sydney",
      "address": "Level 22/6 Watermans Quay, Sydney, NSW 2000",
      "channel": "buy",
      "bedrooms": 2,
      "currency": "AUD",
      "postcode": "2000",
      "bathrooms": 2,
      "price_max": null,
      "agent_name": "Leanne Pan",
      "car_spaces": 1,
      "agency_name": "Ray White Residential - Sydney CBD",
      "building_size": 107,
      "price_display": "$4,000,000",
      "property_type": "Apartment",
      "building_size_display": "107m²"
    }
  ],
  "pagination": {
    "next": "https://www.realestate.com.au/buy/in-sydney,+nsw/list-2",
    "page": 1,
    "per_page": 25,
    "total_pages": 25,
    "total_results": 604
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `results[]` | object[] |  |
| `results[].id` | integer |  |
| `results[].url` | string |  |
| `results[].badge` | object |  |
| `results[].image` | string |  |
| `results[].price` | integer |  |
| `results[].state` | string |  |
| `results[].suburb` | string |  |
| `results[].address` | string |  |
| `results[].channel` | string |  |
| `results[].bedrooms` | integer |  |
| `results[].currency` | string |  |
| `results[].postcode` | string |  |
| `results[].bathrooms` | integer |  |
| `results[].price_max` | object |  |
| `results[].agent_name` | string |  |
| `results[].car_spaces` | integer |  |
| `results[].agency_name` | string |  |
| `results[].building_size` | integer |  |
| `results[].price_display` | string |  |
| `results[].property_type` | string |  |
| `results[].building_size_display` | string |  |
| `pagination` | object |  |
| `pagination.next` | string |  |
| `pagination.page` | integer |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Search |
|---|---|
| cURL (Bash) | [`search`](examples/curl/search.sh) |
| PHP | [`search`](examples/php/search.php) |
| C# | [`search`](examples/csharp/Search.cs) |
| Go | [`search`](examples/go/search.go) |
| Java | [`search`](examples/java/SearchExample.java) |
| Ruby | [`search`](examples/ruby/search.rb) |
| Node.js | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Price monitoring** — Track asking prices and price drops per area.
- **Market analytics** — Measure inventory, days on market and price per m² / sqft.
- **Investment sourcing** — Detect undervalued properties as soon as they are listed.
- **Rental yield models** — Combine sale and rent listings to estimate yields.
- **Proptech products** — Power portals, valuation tools and alerts with structured listings.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — realestate.com.au plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape realestate.com.au?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/realestatecomau/search` **2**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the realestate.com.au API return?

Each endpoint returns structured JSON: **realestate.com.au Search** — `results[]`, `results[].id`, `results[].url`, `results[].badge`, `results[].image`, `results[].price`.

### How do I paginate realestate.com.au results?

Use the `page` parameter and keep requesting while the response indicates more results.

### Can I get the raw HTML instead of JSON?

Yes. Set `return_page_source=true` and the response becomes `{"result": "..."}` with the page source.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Related scrapers

- [Apartments Scraper API](https://github.com/piloterr/apartments-scraper)
- [Argenprop Scraper API](https://github.com/piloterr/argenprop-scraper)
- [Centris Scraper API](https://github.com/piloterr/centris-scraper)
- [Domain.com.au Scraper API](https://github.com/piloterr/domaincomau-scraper)
- [Fotocasa Scraper API](https://github.com/piloterr/fotocasa-scraper)
- [Funda Scraper API](https://github.com/piloterr/funda-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>realestate.com.au is a trademark of its respective owner. This repository is not affiliated with or endorsed by realestate.com.au. Examples are released under the MIT license.</sub>
