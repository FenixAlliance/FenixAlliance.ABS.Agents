---
name: absuite-forex
description: >
  Perform currency exchange calculations and retrieve foreign exchange rates using
  the Alliance Business Suite (ABS) `absuite` CLI. Covers latest and historical
  rate lookups, currency amount conversions, and rate model retrieval. Requires an
  authenticated CLI session.
---

# Alliance Business Suite — Forex Skill

Perform currency exchange operations through the `absuite` CLI's `forex` service.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Discover commands**: `absuite forex list-commands`

## Exchange Currency

### Exchange at Latest Rates

```bash
absuite forex exchange-amount --Amount 1000 --SourceCurrencyId USD.USA --TargetCurrencyId EUR.EU
```

Returns a `Money` object with the converted amount and target currency.

### Exchange at Latest Rates (v3)

```bash
absuite forex exchange-amount-v3 --Amount 1000 --SourceCurrencyId USD.USA --TargetCurrencyId COP.COL
```

### Exchange at Historical Rates

```bash
absuite forex exchange-amount-historical --Amount 1000 --SourceCurrencyId USD.USA --TargetCurrencyId GBP.GBR --Date 2026-01-15T00:00:00Z
```

### Exchange at Historical Rates (v3)

```bash
absuite forex exchange-amount-historical-v3 --Amount 1000 --SourceCurrencyId USD.USA --TargetCurrencyId JPY.JPN --Date 2026-01-15T00:00:00Z
```

## Get Rates

### Latest Rate for a Currency

```bash
absuite forex get latest-currency-rate --CurrencyId EUR.EU
```

### Historical Rate for a Currency

```bash
absuite forex get historical-currency-rate --CurrencyId EUR.EU --Date 2026-01-15T00:00:00Z
```

### All Latest Currency Rates

```bash
absuite forex get latest-currency-rates-model
```

### Historical Currency Rates List

```bash
absuite forex list historical-currency-rates --CurrencyId EUR.EU
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| Convert currency (latest) | `absuite forex exchange-amount --Amount 100 --SourceCurrencyId USD.USA --TargetCurrencyId EUR.EU` |
| Convert currency (historical) | `absuite forex exchange-amount-historical --Amount 100 --SourceCurrencyId USD.USA --TargetCurrencyId EUR.EU --Date <ISO-date>` |
| Get latest rate | `absuite forex get latest-currency-rate --CurrencyId EUR.EU` |
| Get historical rate | `absuite forex get historical-currency-rate --CurrencyId EUR.EU --Date <ISO-date>` |
| Get all rates | `absuite forex get latest-currency-rates-model` |

## Critical Rules

- **Currency IDs follow the `CODE.COUNTRY` format** (e.g., `USD.USA`, `EUR.EU`, `COP.COL`, `GBP.GBR`).
- **Historical dates use ISO 8601 format** (e.g., `2026-01-15T00:00:00Z`).
- **Use `absuite globe list enabled-currencies`** to discover valid currency IDs.
