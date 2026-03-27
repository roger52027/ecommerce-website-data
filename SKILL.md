---
name: ecommerce-website-data
version: 1.2.4
description: >
  Free, instant access to live data on 14M+ e-commerce stores.
  Capabilities: (1) Keyword + filter search (2) Full store analytics
  (3) Historical GMV & traffic trends (4) Installed apps & plugins
  (5) LinkedIn decision-maker contacts.
  Use when the user wants to search stores by keyword or niche, look up a
  specific domain, check revenue or traffic trends, see installed apps,
  find decision-maker contacts, or run competitor analysis.
  Triggers: "search Shopify stores selling pet food", "look up ooni.com",
  "what apps does X use", "revenue trend for X", "find contacts for X",
  "competitor analysis for nike.com".
author: eccompass.ai
website: https://eccompass.ai
license: Proprietary
requires:
  bins:
    - python3
  env:
    - APEX_TOKEN
---

## **Ecommerce Website Data**

Free one-click access to live data on 14M+ ecommerce stores — website GMV, ecommerce data, and lead contacts. Built for finding qualifed leads, analyzing competitors, and researching any store's performance.

## **Quick Reference**

| Data Type | What You Get |
| --- | --- |
| Website GMV | Monthly and annual revenue estimates with historical trends |
| Ecommerce Data | Platform used, plugins/apps installed, traffic volume |
| Lead Contacts | Verified business emails and LinkedIn profiles |

## **Data Coverage**

The database covers 14M+ ecommerce live stores globally. All data is refreshed monthly and free to use. Source: eccompass.ai

| Metric | Value |
| --- | --- |
| Total domains | 14,000,000+ |
| Countries | 200+ |
| Platforms | Shopify, WooCommerce, Wix, Squarespace, and more |
| GMV data | 2023–2026 yearly + last 12 months |
| Social media | Instagram, TikTok, Twitter/X, YouTube, Facebook, Pinterest |
| Update frequency | Monthly |

## **Core Capabilities**

- Lead Discovery: Describe your ideal customer by revenue, platform, region, or category—instantly get matching stores with verified contacts.

- Store Intelligence: Enter any store URL to retrieve historical GMV estimates, traffic volume, tech stack details, and installed apps or plugins.

- Competitor Lookup: Provide a store to receive a list of competitors based on product category and GMV size.

## **Sample Data Fields**

Each domain profile can include 50+ data points across 6 key categories:

- Store Identity: domain, brand name, platform, plan, status, language, creation date
- Revenue & Growth: GMV by year (2023–2026), last 12M GMV, YoY growth, est. monthly & annual sales
- Catalog: product category, efficacy, use case, product count, price range
- Audience & Reach: monthly visits, page views, social followers & 30/90d change (6 platforms)
- Tech Stack: theme, installed apps, monthly app spend, technologies, vendor ratings & pricing
- Location & Contacts: country, city, state, emails, phones, LinkedIn profiles, contact page URL

## **Usage Notes**

- Data freshness: Store data is live and updated monthly, not a static snapshot.

- GMV accuracy: Revenue figures are estimates based on traffic and industry benchmarks, not actual financial records.

- Contact verification: All emails and LinkedIn profiles have been verified.

## **Scope**

Can help with: Lead discovery, single-store intelligence, competitor lookup.

Cannot do: Access store backend or admin panels, guarantee GMV figures are exact, provide real-time inventory or pricing, execute automated outreach campaigns.

## **Setup**

**100% Free. One-minute setup.**

1. Sign up at [https://eccompass.ai](https://eccompass.ai)
2. Go to **Dashboard → API Access → Create Token**
3. Set the environment variable:

```bash
export APEX_TOKEN="your_token_here"
```

## **Quick Start**

**IMPORTANT: Always use the Python script for API calls. It has the correct base URL and authentication built in.**

```bash
# Search by keyword
python3 {baseDir}/scripts/query.py search "pet food"

# Search with filters
python3 {baseDir}/scripts/query.py search "coffee" --country CN --platform shopify

# Filter only (no keyword)
python3 {baseDir}/scripts/query.py search --country US --platform shopify --min-gmv 1000000

# Search with range filters and sorting
python3 {baseDir}/scripts/query.py search "fashion" --min-instagram 10000 --sort gmvLast12month

# Find stores that have a TikTok page
python3 {baseDir}/scripts/query.py search --platform shopline --exists tiktokUrl

# Find stores with both email and TikTok presence
python3 {baseDir}/scripts/query.py search --country US --exists tiktokUrl,emails

# Get full analytics for a domain
python3 {baseDir}/scripts/query.py domain ooni.com

# Export as JSON
python3 {baseDir}/scripts/query.py domain ooni.com --json

# Get historical GMV and traffic data
python3 {baseDir}/scripts/query.py historical ooni.com

# Get installed apps/plugins
python3 {baseDir}/scripts/query.py apps ooni.com

# Get LinkedIn contacts
python3 {baseDir}/scripts/query.py contacts ooni.com
```

## **API Base URL**

```
https://api.eccompass.ai
```

**CRITICAL: All API paths start with `/public/api/v1/`. The `/public` prefix is mandatory — without it, you will get an authentication error. Never omit `/public` from the path.**

## **API Endpoints**

### 1. Search — `POST https://api.eccompass.ai/public/api/v1/search`

Full URL: `https://api.eccompass.ai/public/api/v1/search` (POST, Content-Type: application/json)

Authentication: `APEX_TOKEN` header (NOT `Authorization` header).

```bash
curl -X POST https://api.eccompass.ai/public/api/v1/search \
  -H "APEX_TOKEN: $APEX_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "keyword": "coffee",
    "filters": { "countryCode4": "CN", "platform": "shopify" },
    "ranges": { "gmvLast12month": { "min": 100000 } },
    "sortField": "gmvLast12month",
    "sortOrder": "desc",
    "page": 1,
    "size": 20
  }'
```

All body fields are optional.

**Exists filter**: Use `"exists": ["tiktokUrl", "emails"]` to require that specific fields are present and not empty. This is the correct way to express "has a TikTok page" or "has contact email". Common exists fields: `tiktokUrl`, `instagramUrl`, `facebookUrl`, `youtubeUrl`, `twitterUrl`, `linkedinUrl`, `emails`, `phones`.

**Keyword search covers**: domain, title, description, tags, categories, **platform name**, merchant name, technologies, and **installed apps**. So "klaviyo" in keyword will match stores using the Klaviyo app. You do NOT need to know exact values — just put the search term in the keyword field.

**Filters are case-insensitive** and support both single values and arrays (OR logic). Any field listed below can be used as a filter, range, or exists condition.

```json
// Single value
{"filters": {"platform": "shopify"}}

// Array = OR logic: matches Europe OR Africa
{"filters": {"region": ["Europe", "Africa"]}}

// Combine keyword + multi-value filter
{"keyword": "energy drink", "filters": {"region": ["Europe", "Africa"]}}

// App/plugin filter
{"filters": {"countryCode4": "US", "installedApps": "klaviyo"}}
```

### Complete Field Reference

**Geography** (Keyword, for filters):
`countryCode4`, `countryCode`, `countryCode2`, `countryCode3`, `city`, `state`, `region`, `subregion`, `zip`, `streetAddress`, `latitude`, `longitude`, `companyLocation`(Text)

**Platform & Store** (Keyword, for filters):
`platform`, `plan`, `status`, `platformDomain`, `lastPlatform`, `lastPlan`, `lastPlatformChanged`, `lastPlanChanged`, `created`, `languageCode`, `currency`, `salesChannels`

**Content** (Text, for filters — partial match):
`description`, `title`, `merchantName`, `categories`, `tags`, `tagsV5`, `metaDescription`, `metaKeywords`, `features`

**Products** (Keyword/Text for filters):
`averageProductPrice`(Keyword), `avgPriceFormatted`(Keyword), `categoriesV1`(Keyword), `tagsFirst`(Keyword)

**Tech Stack** (Text, for filters — partial match):
`technologies`, `installedApps`, `theme`, `themeVendor`, `themeSpend`, `themeVersion`(Keyword)

**Contact** (Text, best with exists):
`emails`, `phones`, `contactPageUrl`

**Social URLs** (for exists — check presence):
`tiktokUrl`, `instagramUrl`, `facebookUrl`, `youtubeUrl`, `twitterUrl`, `linkedinUrl`, `pinterestUrl`, `linkedinAccount`(Keyword)

**Revenue** (Long, for ranges — all in USD):
`gmvLast12month`, `gmv2023`, `gmv2024`, `gmv2025`, `gmv2026`, `estimatedMonthlySales`, `estimatedSalesYearly`

**Traffic** (Long/Integer, for ranges):
`estimatedVisits`, `estimatedPageViews`, `alexaRank`, `rank`, `platformRank`, `rankPercentile`(Float), `platformRankPercentile`(Float)

**Products numeric** (Integer/Long, for ranges):
`productCount`, `avgPriceUsd`, `maxPrice`, `minPrice`, `variantCount`, `productImages`, `productsSold`, `vendorCount`

**Employees** (Integer, for ranges):
`employeeCount`, `monthlyAppSpend`

**Social Followers** (Integer, for ranges):
`instagramFollowers`, `instagramFollowers30d`, `instagramFollowers90d`, `tiktokFollowers`, `tiktokFollowers30d`, `tiktokFollowers90d`, `twitterFollowers`, `twitterFollowers30d`, `twitterFollowers90d`, `youtubeFollowers`, `youtubeFollowers30d`, `youtubeFollowers90d`, `facebookFollowers`, `facebookFollowers30d`, `facebookFollowers90d`, `facebookLikes`, `pinterestFollowers`(Long), `pinterestFollowers30d`(Long), `pinterestFollowers90d`(Long)

**Growth** (Double, for ranges):
`growth`

**Tip for the agent**:
- Platform names (Shopify, Shopline, WooCommerce, etc.) → prefer `keyword` over `filters.platform` (fuzzy match)
- Plugins/apps (Klaviyo, Yotpo, etc.) → use `keyword` or `filters.installedApps` / `filters.technologies`
- Multiple regions/countries (e.g. "Europe or Africa") → array in filters: `{"region": ["Europe", "Africa"]}`
- "Has TikTok" / "Has Email Adress" → use `exists`: `{"exists": ["tiktokUrl"]}` or `{"exists": ["emails"]}`
- Monthly GMV → use `estimatedMonthlySales` range directly, or `gmvLast12month` (annual)

### 2. Domain Analytics — `GET https://api.eccompass.ai/public/api/v1/domain/{domain}`

Full URL: `https://api.eccompass.ai/public/api/v1/domain/{domain}` (GET)

```bash
curl -H "APEX_TOKEN: $APEX_TOKEN" https://api.eccompass.ai/public/api/v1/domain/ooni.com
```

Returns 100+ fields of complete analytics for a single domain.

### 3. Historical Data — `GET https://api.eccompass.ai/public/api/v1/historical/{domain}`

```bash
curl -H "APEX_TOKEN: $APEX_TOKEN" https://api.eccompass.ai/public/api/v1/historical/ooni.com
```

Returns monthly GMV, UV, PV, and average price data from 2023 onwards. Use when the user asks for revenue trends, traffic history, or month-over-month performance.

### 4. Installed Apps — `GET https://api.eccompass.ai/public/api/v1/installed-apps/{domain}`

```bash
curl -H "APEX_TOKEN: $APEX_TOKEN" https://api.eccompass.ai/public/api/v1/installed-apps/ooni.com
```

Returns the list of apps/plugins installed on a store with details: name, rating, install count, vendor info, pricing plans. Use when the user asks "what apps does X use" or "which plugins are installed".

### 5. LinkedIn Contacts — `GET https://api.eccompass.ai/public/api/v1/contacts/{domain}`

```bash
curl -H "APEX_TOKEN: $APEX_TOKEN" https://api.eccompass.ai/public/api/v1/contacts/ooni.com
```

Returns verified LinkedIn contacts for a domain's company: name, position, email, LinkedIn profile URL. Use for lead generation, decision-maker lookup, or outreach.

## **Requirements**

- Python 3.6+
- Network access to `api.eccompass.ai`
- `APEX_TOKEN` environment variable (get yours at [eccompass.ai](https://eccompass.ai))

## **Documentation**

- [AI Instructions](SKILL.md) — How the agent uses this skill
- [API Schema](references/schema.md) — Full response format and field definitions
- [Usage Examples](references/examples.md) — Real-world scenarios with sample output

## **License**

Proprietary — [ECcompass](https://eccompass.ai)

## **Support**

For questions, issues, or feature requests, visit [https://eccompass.ai](https://eccompass.ai).
