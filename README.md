# ECcompass E-Commerce Intelligence

> Search and analyze **14,000,000+** e-commerce domains — right from your AI agent.

ECcompass E-Commerce Intelligence gives your AI agent direct access to the world's largest DTC e-commerce database. Ask natural language questions like "find Shopify stores selling pet food" or "look up ooni.com" and get structured analytics instantly.

## Features

- **Keyword Search** — Search across 14M+ domains by product category, brand name, or any keyword. Results ranked by relevance × GMV.
- **Domain Analytics** — Get 100+ fields for any domain: GMV revenue (2023–2026), traffic, social media (6 platforms with 30d/90d trends), product catalog, tech stack, geography, contact info, and reviews.
- **Competitor Analysis** — Discover competitors by keyword, then drill into detailed analytics for each.
- **JSON Export** — Get raw JSON output for programmatic processing and integration.

## Setup

1. Sign up at [https://eccompass.ai](https://eccompass.ai)
2. Go to **Dashboard → API Access → Create Token**
3. Set the environment variable:

```bash
export APEX_TOKEN="your_token_here"
```

## Quick Start

```bash
# Search by keyword
python3 scripts/query.py search "pet food"

# Search with country + platform filters
python3 scripts/query.py search "coffee" --country CN --platform shopify

# Filter only (no keyword)
python3 scripts/query.py search --country US --platform shopify --min-gmv 1000000

# Get full analytics for a domain
python3 scripts/query.py domain ooni.com

# Historical GMV and traffic trends
python3 scripts/query.py historical ooni.com

# Installed apps/plugins
python3 scripts/query.py apps ooni.com

# LinkedIn contacts
python3 scripts/query.py contacts ooni.com
```

## Data Coverage

| Metric | Value |
|--------|-------|
| Total domains | 14,000,000+ |
| Countries | 200+ |
| Platforms | Shopify, WooCommerce, Magento, BigCommerce, Wix, Squarespace, and more |
| GMV data | 2023–2026 yearly + last 12 months |
| Social media | Instagram, TikTok, Twitter/X, YouTube, Facebook, Pinterest |
| Update frequency | Monthly |

## Analytics Fields

Each domain profile includes:

- **Basic Info** — domain, brand name, platform, plan, status, creation date, language
- **Revenue** — GMV 2023–2026, last 12 months, YoY growth, estimated monthly/yearly sales
- **Products** — count, average price, price range, variants, images
- **Traffic** — monthly visits, page views, Alexa rank, platform rank
- **Social Media** — followers + 30d/90d change for 6 platforms
- **Tech Stack** — technologies, installed apps, theme, monthly app spend
- **Geography** — country, city, state, coordinates, company location
- **Contact** — emails, phones, contact page URL
- **Reviews** — Trustpilot, Yotpo ratings

## Requirements

- Python 3.6+
- Network access to `api.eccompass.ai`
- `APEX_TOKEN` environment variable (get yours at [eccompass.ai](https://eccompass.ai))

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/public/api/v1/search` | POST | Search domains with keyword, filters, ranges, and sorting |
| `/public/api/v1/domain/{domain}` | GET | Full analytics for a single domain |
| `/public/api/v1/historical/{domain}` | GET | Monthly GMV and traffic history (2023+) |
| `/public/api/v1/installed-apps/{domain}` | GET | Installed apps/plugins with vendor details |
| `/public/api/v1/contacts/{domain}` | GET | LinkedIn contacts (name, position, email) |

## Documentation

- [AI Instructions](SKILL.md) — How the agent uses this skill
- [API Schema](references/schema.md) — Full response format and field definitions
- [Usage Examples](references/examples.md) — Real-world scenarios with sample output

## License

Proprietary — [ECcompass](https://eccompass.ai)

## Support

For questions, issues, or feature requests, visit [https://eccompass.ai](https://eccompass.ai).
