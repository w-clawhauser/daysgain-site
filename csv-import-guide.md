# Fintify CSV Import Guide

Fintify supports CSV import so you can bring investment and cash activity records into the app.

This guide describes the Fintify CSV v1 format. Fintify intentionally supports one clean import format that matches the app's export format.

## What You Can Import

Fintify CSV import supports:

- Buy transactions
- Sell transactions
- Deposits
- Withdrawals

Dividend and split events are not imported manually through CSV. Fintify retrieves dividend and split data from market data providers when available.

## Recommended Workflow

For the cleanest import experience:

1. Export a CSV from Fintify.
2. Use the exported file as the template.
3. Edit or append rows in the same column format.
4. Import the updated CSV back into Fintify.

If you do not have complete transaction history, you can start with one initial buy row per current holding using your current share quantity and average cost. Historical gain/loss before that initial row may not be accurate.

## Required Format

Your CSV file must include a header row and must use Fintify's activity format with the `activity_category` column.

Recommended full header:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
```

The importer requires these core columns:

- `date`
- `activity_category`
- `type`
- `account_name`
- `owner_name`
- `account_type`
- `institution`
- `currency`

## Column Reference

| Column | Required | Description |
| --- | --- | --- |
| `source_record_id` | Optional | Stable row ID used to avoid duplicate imports. Exported CSV files include this automatically. |
| `date` | Yes | Activity date. Recommended format: `YYYY-MM-DD`. |
| `activity_category` | Yes | Use `transaction` for buy/sell rows and `transfer` for deposit/withdraw rows. |
| `type` | Yes | Use `buy`, `sell`, `deposit`, or `withdraw`. |
| `account_name` | Yes | Account display name or source label. |
| `owner_name` | Yes | Owner label, such as `WD` or `My`. |
| `account_type` | Yes | Account type, such as `TFSA`, `RRSP`, `Cash`, `Non-registered`, or `A股账户`. |
| `institution` | Yes | Brokerage or institution, such as `Questrade`, `RBC`, `Wealthsimple`, or `国金证券`. |
| `ticker` | Required for buy/sell | Display ticker, such as `NVDA`, `ENB.TO`, or `600036`. Leave blank for deposit/withdraw rows. |
| `api_symbol` | Recommended for buy/sell | Market data symbol. For China A-shares, use symbols such as `600036.SS` or `000651.SZ`. |
| `security_name` | Optional | Security name, such as `NVIDIA Corp`. |
| `market` | Recommended for buy/sell | Use `us`, `canada`, or `chinaA`. |
| `currency` | Yes | Use `USD`, `CAD`, or `CNY`. |
| `quantity` | Required for buy/sell | Number of shares or units. Use positive numbers for both buy and sell rows. |
| `price` | Required for buy/sell | Price per share or unit. |
| `cash_amount` | Required for deposit/withdraw | Cash amount for transfer rows. Use positive numbers for both deposit and withdraw rows. |
| `fee` | Optional | Transaction fee or commission. |
| `notes` | Optional | Any additional notes. |
| `counterparty_account_name` | Optional | Reserved for transfer workflows. Usually blank. |

## Activity Rules

For buy and sell rows:

- `activity_category` must be `transaction`.
- `type` must be `buy` or `sell`.
- `ticker`, `quantity`, and `price` should be filled.
- `cash_amount` should be blank.

For deposit and withdraw rows:

- `activity_category` must be `transfer`.
- `type` must be `deposit` or `withdraw`.
- `cash_amount` should be filled.
- `ticker`, `quantity`, and `price` should be blank.

## Supported Markets

Fintify currently supports US, Canadian, and China A-share securities.

| Market | `market` value | Ticker examples | Notes |
| --- | --- | --- | --- |
| US stocks / ETFs | `us` | `AAPL`, `MSFT`, `NVDA`, `VGT` | Plain ticker, usually USD. |
| Canadian stocks / ETFs | `canada` | `ENB.TO`, `BNS.TO`, `VDY.TO`, `XEI.TO` | Use Yahoo-style Canadian suffixes such as `.TO`. |
| China A-shares / ETFs | `chinaA` | `600036`, `000651`, `510300` | Use plain ticker in `ticker` and provider symbol such as `600036.SS` or `000651.SZ` in `api_symbol`. |

Ticker availability and market data quality may vary by provider.

## Example CSV

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
dep-2024-01-15,2024-01-15,transfer,deposit,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,5000,,Initial deposit,
buy-2024-01-16-aapl,2024-01-16,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc,us,USD,10,185.50,,1.00,Initial position,
buy-2024-02-10-enb,2024-02-10,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,ENB.TO,ENB.TO,Enbridge Inc,canada,CAD,50,48.25,,0,Canadian dividend stock,
buy-2024-03-05-600036,2024-03-05,transaction,buy,WD-China-Guojin,WD,A股账户,国金证券,600036,600036.SS,招商银行,chinaA,CNY,1000,38.20,,5,A-share position,
sell-2024-06-20-aapl,2024-06-20,transaction,sell,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc,us,USD,2,210.00,,1.00,Partial sale,
wdr-2024-07-01,2024-07-01,transfer,withdraw,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,1000,,Cash withdrawal,
```

## Import Tips

- Use one row per activity.
- Keep dates in `YYYY-MM-DD` format when possible.
- Use positive quantities for both buy and sell rows.
- Use positive cash amounts for both deposits and withdrawals.
- Use `USD`, `CAD`, or `CNY` as currency codes.
- Keep `owner_name`, `account_type`, and `institution` consistent for the same account.
- Keep `source_record_id` stable if you plan to import the same file multiple times.
- Avoid duplicate rows unless they represent separate real activities.
- Review your portfolio after importing to confirm values look reasonable.
- Export a backup CSV before making large changes.

## Privacy and Security

CSV files may contain investment records and other financial information. Store, back up, and share exported CSV files carefully.

Fintify is designed as a local-first app. Your portfolio records are stored on your device unless you choose to export, share, or otherwise provide them outside the app.

## Data Accuracy

Fintify relies on the data you enter or import. Incomplete, duplicated, incorrectly formatted, or inconsistent CSV data may cause inaccurate portfolio values, cost basis, gain/loss, dividend summaries, account summaries, or charts.

Third-party market data, dividends, exchange rates, and corporate actions may be delayed, incomplete, unavailable, or inaccurate. Verify important financial information with your brokerage, financial institution, tax advisor, or other qualified professional.
