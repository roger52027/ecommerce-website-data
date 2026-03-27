# Changelog

All notable changes to this skill will be documented in this file.

## [1.2.4] - 2026-03-26

### Fixed

- Resolved ClawHub security flag: metadata inconsistencies across files
- Aligned `version`, `author`, `requires.bins`, `requires.env` between `claw.json`, `SKILL.md`, and `query.py`
- Removed false `curl` dependency — skill only requires `python3`
- Added `requires` section to `claw.json` to explicitly declare `bins` and `env`
- Fixed data update frequency inconsistency (README said "Weekly", now all files say "Monthly")

## [1.2.3] - 2026-03-26

### Added

- **3 new API endpoints**:
  - `GET /historical/{domain}` — monthly GMV, UV, PV, and average price history from 2023 onwards
  - `GET /installed-apps/{domain}` — installed apps/plugins with ratings, install counts, vendor info, and pricing plans
  - `GET /contacts/{domain}` — verified LinkedIn contacts (name, position, email) for a domain's company
- **Exists filter** (`exists`): query fields that must be present and not empty (e.g. `["tiktokUrl", "emails"]`)
- **Multi-value OR filters**: pass comma-separated values for the same field (e.g. `region: "Europe,Africa"`)
- **Complete field reference** in SKILL.md — all available ES fields documented for filters, ranges, and exists
- Case-insensitive keyword filters for platform, country, and other Keyword-type fields
- Keyword search now covers `installed_apps` and `platform` fields
- CLI commands: `historical`, `apps`, `contacts` subcommands in `query.py`
- CLI flag `--exists` for exists filter in search

### Changed

- Search API changed from `GET /search/{keyword}` to `POST /search` with JSON body
- Search keyword is now optional (can search by filters only)
- Range queries use `gte`/`lte` JSON syntax for Elasticsearch compatibility
- `installed-apps` and `contacts` endpoints return empty data instead of 400 error when no results found

### Fixed

- Elasticsearch `[range] query does not support [from]` error — switched to `wrapperQuery` with manual JSON
- `estimatedMonthlySales` and `estimatedSalesYearly` field types corrected from String to Long
- `avgPriceUsd` field type corrected from String to Integer
- Historical data API double-wrapping issue (nested `data.data`)
- Range query `Double.toString()` scientific notation risk — now uses `BigDecimal.toPlainString()`
- Removed duplicate database queries in API key validation flow
- Removed API whitelist mechanism from rate limit aspect

## [1.2.1] - 2026-03-19

### Added

- Keyword search across 14M+ e-commerce domains via ECcompass REST API
- Full domain analytics with 100+ fields (GMV, traffic, social media, tech stack, etc.)
- Python CLI tool (`scripts/query.py`) with `search` and `domain` subcommands
- JSON export support (`--json` flag)
- Paginated search results (up to 100 per page)
- Complete API response schema documentation
- Usage examples covering real-world scenarios

