---
title: DaysGain CSV Import Guide
permalink: /csv-import-guide/
lang: en
current_language: English
alternate_label: 中文
alternate_lang: zh-Hans
alternate_url: https://daysgain.com/zh/csv-import-guide/
home_url: https://daysgain.com/
asset_prefix: ../
---

# DaysGain CSV Import Guide

DaysGain supports CSV import so you can bring investment and cash activity records into the app.

CSV import is designed for users who want to migrate existing transaction history, restore data from a backup, or manage records in a spreadsheet.

## The Easiest Way to Start

The safest way to create a correctly formatted CSV file is to export your data from DaysGain first, then use the exported file as your template.

Recommended workflow:

1. Go to **Settings → Export CSV** in DaysGain.
2. Open the exported file in a spreadsheet app.
3. Add, edit, or append rows using the same column format.
4. Save the file as CSV.
5. Import the updated CSV back into DaysGain.

If you are starting fresh with no existing data, follow the format described below.

## CSV Format

DaysGain uses one CSV format for both import and export.

Your CSV file must include a header row. Column order does not matter. Column names are case-insensitive.

The standard DaysGain CSV header is:

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
```

## What You Can Import

DaysGain CSV import supports:

- Buy transactions
- Sell transactions
- Deposits
- Withdrawals

Dividend and split events are not imported manually through CSV. DaysGain retrieves dividend and split data from market data providers when available.

## Required Columns

Every CSV file must include these columns:

| Column | Example | Description |
|---|---|---|
| `date` | `2026-01-15` | Activity date. Recommended format: `YYYY-MM-DD`. Also accepts `YYYY/MM/DD` or `MM/DD/YYYY`. |
| `activity_category` | `transaction` | Use `transaction` for buy/sell rows. Use `transfer` for deposit/withdraw rows. Case-insensitive. |
| `type` | `buy` | Use `buy`, `sell`, `deposit`, or `withdraw`. Case-insensitive. |
| `account_name` | `Donald-TFSA-TD` | Account display name. Keep it consistent across rows for the same account. Max ~30 chars. |
| `owner_name` | `Donald` | Owner label. Max ~10 chars. |
| `account_type` | `TFSA` | Account type, such as `TFSA`, `RRSP`, `IRA`, or `Taxable`. Max ~10 chars. |
| `institution` | `TD` | Brokerage or institution. Max ~10 chars. |
| `currency` | `USD` | Must match exactly. Accepted values: `USD`, `CAD`, `CNY`, `HKD`, `AUD`, `GBP`, `SGD`, `TWD`. Case-insensitive. |

## Optional Columns

| Column | Description |
|---|---|
| `ticker` | Stock symbol. For international stocks, use the full symbol including suffix — e.g. `601398.SS` (China A), `0700.HK` (HK), `RY.TO` (Canada), `HSBA.L` (UK), `CBA.AX` (Australia), `D05.SI` (Singapore), `2330.TW` (Taiwan). |
| `security_name` | Display name, e.g. `Apple Inc.` Max ~50 chars. |
| `market` | `USA` / `CAN` / `CHN` / `HKG` / `AUS` / `GBR` / `SGX` / `TWS`. Case-insensitive. Inferred from ticker if omitted. |
| `quantity` | Positive whole number (≥ 1). Required for buy/sell. |
| `price` | Positive number. Required for buy/sell. Commas OK, e.g. `1,234.56`. |
| `cash_amount` | Positive number. Required for deposit/withdraw. Commas OK. |
| `fee` | Positive number. Commas OK. Default 0 if omitted. |
| `notes` | Any notes. |

## Activity Rules

### Buy and Sell Rows

For buy and sell rows:

- `activity_category` must be `transaction`.
- `type` must be `buy` or `sell`.
- `ticker`, `quantity`, and `price` should be filled.
- `cash_amount` should be blank.
- `fee` can be filled if there is a commission or transaction fee.

Example:

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-01-15,transaction,buy,Donald-TFSA-TD,Donald,TFSA,TD,USD,AAPL,Apple Inc.,USA,5,210.50,,9.99,
```

### Deposit and Withdraw Rows

For deposit and withdraw rows:

- `activity_category` must be `transfer`.
- `type` must be `deposit` or `withdraw`.
- `currency` must be filled.
- `cash_amount` must be filled.
- `ticker`, `security_name`, `market`, `quantity`, `price`, and `fee` should usually be blank.

Example:

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-01-05,transfer,deposit,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,5000.00,,Initial deposit
2026-04-01,transfer,withdraw,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,1000.00,,
```

## Supported Activity Combinations

| `activity_category` | `type` | Meaning |
|---|---|---|
| `transaction` | `buy` | Buy a security |
| `transaction` | `sell` | Sell a security |
| `transfer` | `deposit` | Add cash to an account |
| `transfer` | `withdraw` | Remove cash from an account |

## Ticker and Market Detection

Use the full market symbol for the `ticker` field, including any exchange suffix.

| Market | `market` value | `ticker` example | Currency |
|---|---|---|---|
| US stocks / ETFs | `USA` | `AAPL` | `USD` |
| Canadian stocks / ETFs | `CAN` | `RY.TO` | `CAD` |
| China A-shares (Shanghai) | `CHN` | `601398.SS` | `CNY` |
| China A-shares (Shenzhen) | `CHN` | `000651.SZ` | `CNY` |
| Hong Kong stocks | `HKG` | `0700.HK` | `HKD` |
| Australian stocks | `AUS` | `CBA.AX` | `AUD` |
| UK stocks | `GBR` | `HSBA.L` | `GBP` |
| Singapore stocks | `SGX` | `D05.SI` | `SGD` |
| Taiwan stocks | `TWS` | `2330.TW` | `TWD` |

DaysGain determines the market using this priority:

1. `market`, if provided.
2. Inferred from the ticker suffix if `market` is blank.
3. `currency`, if both `market` and ticker suffix are ambiguous.

Ticker availability and market data quality may vary by provider.

## Example CSV

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-01-05,transfer,deposit,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,5000.00,,Initial deposit
2026-01-15,transaction,buy,Donald-TFSA-TD,Donald,TFSA,TD,USD,AAPL,Apple Inc.,USA,5,210.50,,9.99,
2026-01-20,transaction,buy,Donald-RRSP-TD,Donald,RRSP,TD,CAD,RY.TO,Royal Bank of Canada,CAN,10,132.80,,0,
2026-02-10,transaction,buy,Donald-Taxable-IBKR,Donald,Taxable,IBKR,HKD,0700.HK,Tencent Holdings,HKG,100,380.00,,0,
2026-02-20,transaction,buy,Donald-Taxable-IBKR,Donald,Taxable,IBKR,CNY,601398.SS,ICBC,CHN,1000,6.52,,0,
2026-03-15,transaction,sell,Donald-TFSA-TD,Donald,TFSA,TD,USD,AAPL,Apple Inc.,USA,2,245.00,,9.99,Partial sell
2026-04-01,transfer,withdraw,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,1000.00,,
```

## Starting Without Complete History

You do not need every historical trade to start using DaysGain.

If you already hold a position but do not have the full transaction history, add one initial buy row using your current share count and average cost.

Example:

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-04-28,transaction,buy,Donald-RRSP-TD,Donald,RRSP,TD,USD,MSFT,Microsoft Corp.,USA,50,320.00,,0,Initial holding based on average cost
```

DaysGain will track your position from this date forward. Gain/loss before this date will not reflect your actual history.

If you do not know your exact average cost, you can use an estimate or current market price. Gain/loss figures will be based on whichever cost you enter.

## Import Tips

- Use one row per activity.
- Keep `account_name`, `owner_name`, `account_type`, and `institution` consistent for the same account across all rows.
- Use `YYYY-MM-DD` date format when possible. Other formats such as `MM/DD/YYYY` may also be accepted.
- Use positive numbers for both buy and sell quantities.
- Use positive numbers for both deposit and withdraw cash amounts.
- Export a backup from DaysGain before importing large changes.
- Review your portfolio after importing to confirm holdings and values look correct.

## Data Accuracy

DaysGain relies on the data you enter or import.

Incomplete, duplicated, incorrectly formatted, or inconsistent CSV data may cause inaccurate portfolio values, cost basis, gain/loss figures, dividend summaries, account summaries, or charts.

Market data, dividends, exchange rates, and corporate actions are sourced from third-party providers and may be delayed, incomplete, inaccurate, or unavailable.

You should verify important financial figures with your brokerage, financial institution, tax advisor, or other qualified professional.

## Privacy and Security

CSV files may contain investment records and financial information.

Store, back up, and share exported CSV files carefully.

DaysGain is designed as a local-first app. Your portfolio records are stored on your device unless you choose to export, share, or otherwise provide them outside the app.

## Need Help?

If you run into issues or have questions, contact us at:

**hello@daysgain.com**

When reporting an import issue, please include:

- App version
- iOS version
- Device model
- A description of what went wrong
- A sample of the rows that failed, with sensitive information removed

Please do not include sensitive personal or financial information unless you choose to do so.
