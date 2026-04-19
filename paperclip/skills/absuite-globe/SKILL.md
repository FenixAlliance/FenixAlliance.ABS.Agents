---
name: absuite-globe
description: >
  Look up countries, states, cities, currencies, languages, timezones, calling codes,
  and top-level domains using the Alliance Business Suite (ABS) `absuite` CLI.
  This is a reference data service — all data is read-only. Requires an authenticated
  CLI session.
---

# Alliance Business Suite — Globe Skill

Query global reference data through the `absuite` CLI's `globe` service. This service provides countries, currencies, languages, timezones, and related lookup data.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Discover commands**: `absuite globe list-commands`

## Countries

```bash
# List all countries
absuite globe list all-countries

# Count countries
absuite globe count countries

# Get country by ID (ISO 3166 alpha-3, e.g., USA, COL, GBR)
absuite globe get country-by-id --CountryId USA

# Search countries by name
absuite globe search-countries-by-name --Name "United"
```

### Country States/Provinces

```bash
absuite globe list country-states --CountryId USA
absuite globe get country-state-by-id --CountryStateId <state-guid>
```

### Cities in a State

```bash
absuite globe get cities-by-country-state-id --CountryStateId <state-guid>
```

### Country-Specific Data

```bash
# Calling codes (e.g., +1 for USA)
absuite globe get calling-codes-by-country-id --CountryId USA

# Top-level domains (e.g., .us)
absuite globe get top-level-domains-by-country-id --CountryId USA

# Timezones for a country
absuite globe get time-zones-by-country-id --CountryId USA

# Currencies enabled for a country
absuite globe get enabled-currencies-by-country-id --CountryId USA
```

## Currencies

```bash
# List all enabled currencies
absuite globe list enabled-currencies

# Count currencies
absuite globe count currencies

# Get currency by ID (format: CODE.COUNTRY, e.g., USD.USA)
absuite globe get currency-by-id --CurrencyId USD.USA
```

## Languages

```bash
# List all languages
absuite globe list languages

# Count languages
absuite globe count languages

# Get language by ID
absuite globe get language-by-id --LanguageId <language-guid>
```

## Timezones

```bash
# List all timezones
absuite globe list time-zones

# Count timezones
absuite globe count timezones

# Get timezone by ID
absuite globe get time-zone-by-id --TimezoneId <timezone-guid>
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List countries | `absuite globe list all-countries` |
| Search countries | `absuite globe search-countries-by-name --Name "Colombia"` |
| Get country | `absuite globe get country-by-id --CountryId COL` |
| List states | `absuite globe list country-states --CountryId COL` |
| Get cities | `absuite globe get cities-by-country-state-id --CountryStateId <guid>` |
| List currencies | `absuite globe list enabled-currencies` |
| Get currency | `absuite globe get currency-by-id --CurrencyId COP.COL` |
| List languages | `absuite globe list languages` |
| List timezones | `absuite globe list time-zones` |

## Critical Rules

- **This is a read-only service.** No create/update/delete operations.
- **Country IDs use ISO 3166 alpha-3 codes** (e.g., `USA`, `COL`, `GBR`, `DEU`).
- **Currency IDs use `CODE.COUNTRY` format** (e.g., `USD.USA`, `EUR.EU`, `COP.COL`).
- **Use this service for foreign key lookups** when creating entities that reference countries, currencies, languages, or timezones.
