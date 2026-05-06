---
title: DaysGain CSV Import Guide
permalink: /csv-import-guide/
lang: en
current_language: English
alternate_label: 中文
alternate_lang: zh-Hans
alternate_url: ../zh/csv-import-guide
home_url: ../
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

## One CSV Format

DaysGain uses one CSV format for both import and export.

Your CSV file must include a header row.

The standard DaysGain CSV header is:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
```

## What You Can Import

DaysGain CSV import supports:

- Buy transactions
- Sell transactions
- Deposits
- Withdrawals

Dividend and split events are not imported manually through CSV. DaysGain retrieves dividend and split data from market data providers when available.

## Required Core Columns

Every CSV file must include these core columns:

| Column | Required | Description |
|---|---:|---|
| `date` | Yes | Activity date. Recommended format: `YYYY-MM-DD`. |
| `activity_category` | Yes | Use `transaction` for buy/sell rows. Use `transfer` for deposit/withdraw rows. |
| `type` | Yes | Use `buy`, `sell`, `deposit`, or `withdraw`. |
| `account_name` | Yes | Account display name or source label. Keep it consistent for the same account. |
| `owner_name` | Yes | Owner label, such as initials or name. The column is required, but the value can be left blank if needed. |
| `account_type` | Yes | Account type, such as `TFSA`, `RRSP`, `Cash`, `Margin`, or `A股账户`. |
| `institution` | Yes | Brokerage or institution, such as `Questrade`, `Wealthsimple`, `RBC`, or `国金证券`. |
| `currency` | Yes | Use `USD`, `CAD`, or `CNY`. |

## Full Column Reference

| Column | Required | Description |
|---|---:|---|
| `source_record_id` | Optional | Stable row ID used to avoid duplicate imports. Exported CSV files include this automatically. |
| `date` | Yes | Activity date. Recommended format: `YYYY-MM-DD`. |
| `activity_category` | Yes | Use `transaction` for buy/sell rows. Use `transfer` for deposit/withdraw rows. |
| `type` | Yes | Use `buy`, `sell`, `deposit`, or `withdraw`. |
| `account_name` | Yes | Account display name or source label. Keep this aligned with `owner_name`, `account_type`, and `institution`. |
| `owner_name` | Yes | Owner label, such as `WD`, `My`, or another label you use. The column is required, but the value can be left blank if needed. |
| `account_type` | Yes | Account type, such as `TFSA`, `RRSP`, `Cash`, `Margin`, `Non-registered`, or `A股账户`. |
| `institution` | Yes | Brokerage or institution, such as `Questrade`, `Wealthsimple`, `RBC`, `IBKR`, or `国金证券`. |
| `ticker` | Required for buy/sell | Display ticker, such as `AAPL`, `ENB.TO`, or `600036`. Leave blank for deposit/withdraw rows. |
| `api_symbol` | Recommended for buy/sell | Market data symbol. For China A-shares, use symbols such as `600036.SS` or `000651.SZ`. |
| `security_name` | Optional | Security name, such as `Apple Inc.` or `招商银行`. |
| `market` | Recommended for buy/sell | Market hint used when `api_symbol` is blank. Use `us`, `canada`, or `chinaA`. |
| `currency` | Yes | Use `USD`, `CAD`, or `CNY`. |
| `quantity` | Required for buy/sell | Number of shares or units. Use positive numbers for both buy and sell rows. |
| `price` | Required for buy/sell | Price per share or unit. |
| `cash_amount` | Required for deposit/withdraw | Cash amount for transfer rows. Use positive numbers for both deposit and withdraw rows. |
| `fee` | Optional | Transaction fee or commission. Usually used for buy/sell rows. |
| `notes` | Optional | Any additional notes. |
| `counterparty_account_name` | Optional | Reserved for transfer workflows. Usually blank. |

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
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
buy-2024-01-16-aapl,2024-01-16,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc.,us,USD,10,185.50,,1.00,Initial position,
```

### Deposit and Withdraw Rows

For deposit and withdraw rows:

- `activity_category` must be `transfer`.
- `type` must be `deposit` or `withdraw`.
- `currency` must be filled.
- `cash_amount` must be filled.
- `ticker`, `api_symbol`, `security_name`, `market`, `quantity`, `price`, and `fee` should usually be blank.

Example:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
dep-2024-01-15,2024-01-15,transfer,deposit,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,5000,,Initial deposit,
wdr-2024-07-01,2024-07-01,transfer,withdraw,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,1000,,Cash withdrawal,
```

## Supported Activity Combinations

| `activity_category` | `type` | Meaning |
|---|---|---|
| `transaction` | `buy` | Buy a security |
| `transaction` | `sell` | Sell a security |
| `transfer` | `deposit` | Add cash to an account |
| `transfer` | `withdraw` | Remove cash from an account |

## Ticker, API Symbol, and Market Detection

DaysGain separates the display ticker from the market data symbol.

| Field | Purpose | Example |
|---|---|---|
| `ticker` | The ticker shown in the app | `ENB` |
| `api_symbol` | The symbol used for market data lookup | `ENB.TO` |
| `market` | Optional market hint | `canada` |

DaysGain determines the market using this priority:

1. `api_symbol`, if provided.
2. `market`, if `api_symbol` is blank.
3. `currency`, if both `api_symbol` and `market` are blank.

Recommended format:

| Market | `market` value | `ticker` example | `api_symbol` example | Currency |
|---|---|---|---|---|
| US stocks / ETFs | `us` | `AAPL` | `AAPL` | `USD` |
| Canadian stocks / ETFs | `canada` | `ENB.TO` or `ENB` | `ENB.TO` | `CAD` |
| China A-shares / ETFs | `chinaA` | `600036` | `600036.SS` | `CNY` |
| China Shenzhen-listed securities | `chinaA` | `000651` | `000651.SZ` | `CNY` |

For the most accurate market data lookup:

- Use `api_symbol` for Canadian and China A-share securities.
- For Shanghai-listed China A-shares, use `.SS`, such as `600036.SS`.
- For Shenzhen-listed China A-shares, use `.SZ`, such as `000651.SZ`.
- US securities usually use the same value for `ticker` and `api_symbol`.

Ticker availability and market data quality may vary by provider.

## Example CSV

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
dep-2024-01-15,2024-01-15,transfer,deposit,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,5000,,Initial deposit,
buy-2024-01-16-aapl,2024-01-16,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc.,us,USD,10,185.50,,1.00,Initial position,
buy-2024-02-10-enb,2024-02-10,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,ENB.TO,ENB.TO,Enbridge Inc.,canada,CAD,50,48.25,,0.00,Canadian dividend stock,
buy-2024-03-05-nvda,2024-03-05,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,NVDA,NVDA,NVIDIA Corp.,us,USD,5,850.00,,1.00,Before split,
buy-2024-03-10-600036,2024-03-10,transaction,buy,WD-China-Guojin,WD,A股账户,国金证券,600036,600036.SS,招商银行,chinaA,CNY,1000,38.20,,5.00,A-share position,
sell-2024-06-20-aapl,2024-06-20,transaction,sell,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc.,us,USD,2,210.00,,1.00,Partial sale,
wdr-2024-07-01,2024-07-01,transfer,withdraw,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,1000,,Cash withdrawal,
```

## Starting Without Complete History

You do not need every historical trade to start using DaysGain.

If you already hold a position but do not have the full transaction history, add one initial buy row using your current share count and average cost.

Example:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
initial-msft,2026-04-28,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,MSFT,MSFT,Microsoft Corp.,us,USD,50,320.00,,0,Initial holding based on average cost,
```

DaysGain will track your position from this date forward. Gain/loss before this date will not reflect your actual history.

If you do not know your exact average cost, you can use an estimate or current market price. Gain/loss figures will be based on whichever cost you enter.

## Import Tips

- Use one row per activity.
- Keep `account_name`, `owner_name`, `account_type`, and `institution` consistent for the same account across all rows.
- Use `YYYY-MM-DD` date format when possible. Other formats such as `MM/DD/YYYY` may also be accepted.
- Use positive numbers for both buy and sell quantities.
- Use positive numbers for both deposit and withdraw cash amounts.
- Use `USD`, `CAD`, or `CNY` as currency codes.
- Use `source_record_id` when possible if you plan to import the same file multiple times.
- Avoid duplicate rows unless they represent separate real activities.
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

**daysgain.app@gmail.com**

When reporting an import issue, please include:

- App version
- iOS version
- Device model
- A description of what went wrong
- A sample of the rows that failed, with sensitive information removed

Please do not include sensitive personal or financial information unless you choose to do so.
