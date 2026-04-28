# Fintify CSV Import Guide

Fintify supports CSV import so you can bring your investment records into the app.

CSV import is designed for users who want to migrate existing transaction history, restore data from a backup, or manage records in a spreadsheet.

## 1. One CSV Format for Import and Export

Fintify uses one CSV format for both export and import.

The safest way to create a compatible CSV file is to export a CSV from Fintify first, then use the same column headers when editing or preparing your own file.

The standard Fintify CSV header is:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
```

## 2. What You Can Import

Fintify supports two main activity categories:

- Investment transactions
- Cash transfers

Supported activity types include:

- Buy
- Sell
- Deposit
- Withdraw

Dividend and split data may be retrieved from market data providers when available. Users generally do not need to manually enter dividend or split records.

## 3. Required Columns

Every CSV file must include these columns:

```csv
date,activity_category,type,account_name,account_type,institution,currency
```

For investment transaction rows, these additional fields should be filled:

```csv
ticker,quantity,price
```

For cash transfer rows, this field should be filled:

```csv
cash_amount
```

## 4. Column Reference

| Column | Required | Description |
|---|---:|---|
| source_record_id | Optional | A stable unique ID for the row. Helps Fintify avoid duplicate imports. If left blank, Fintify will use row details to detect duplicates. |
| date | Yes | Activity date. Recommended format: `YYYY-MM-DD`. |
| activity_category | Yes | Use `transaction` for buy/sell rows. Use `transfer` for deposit/withdraw rows. |
| type | Yes | For `transaction`: use `buy` or `sell`. For `transfer`: use `deposit` or `withdraw`. |
| account_name | Yes | Account label, such as `WD-RRSP-Questrade`, `TFSA`, `RRSP`, `Cash`, or your own account name. |
| owner_name | Optional | Owner label if you track multiple people or portfolios. If left blank, Fintify may infer an owner from the account name. |
| account_type | Yes | Account type, such as `TFSA`, `RRSP`, `Cash`, `Margin`, or another label you use. |
| institution | Yes | Brokerage or institution, such as `Questrade`, `Wealthsimple`, `RBC`, `IBKR`, or another label you use. |
| ticker | Required for buy/sell | Security ticker, such as `AAPL`, `NVDA`, `ENB`, or `600036`. Leave blank for deposit/withdraw rows. |
| api_symbol | Recommended for non-US securities | Market data symbol used for lookup, such as `ENB.TO`, `600036.SS`, or `510300.SS`. For US securities, this is usually the same as `ticker`. |
| security_name | Optional | Security name, such as `Apple Inc.` or `Enbridge Inc.` |
| market | Optional | Exported for reference. Import currently relies mainly on `api_symbol` and `currency` to infer the market. |
| currency | Yes | Activity currency: `USD`, `CAD`, or `CNY`. |
| quantity | Required for buy/sell | Number of shares or units. Leave blank for deposit/withdraw rows. |
| price | Required for buy/sell | Price per share or unit. Leave blank for deposit/withdraw rows. |
| cash_amount | Required for deposit/withdraw | Cash amount for deposit or withdrawal. Leave blank for buy/sell rows. |
| fee | Optional | Transaction fee or commission. Usually used for buy/sell rows. |
| notes | Optional | Any additional notes. |
| counterparty_account_name | Optional | Optional account name for transfer-related notes or future transfer workflows. |

## 5. Activity Categories and Types

Use the following combinations:

| activity_category | type | Meaning |
|---|---|---|
| transaction | buy | Buy a security |
| transaction | sell | Sell a security |
| transfer | deposit | Add cash to an account |
| transfer | withdraw | Remove cash from an account |

Examples:

```csv
activity_category,type
transaction,buy
transaction,sell
transfer,deposit
transfer,withdraw
```

## 6. Example CSV

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
dep-2024-01-15-tfsa,2024-01-15,transfer,deposit,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,5000,,Initial deposit,
buy-2024-01-16-aapl,2024-01-16,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc.,us,USD,10,185.50,,1.00,Initial position,
buy-2024-02-10-enb,2024-02-10,transaction,buy,WD-TFSA-Questrade,WD,TFSA,Questrade,ENB,ENB.TO,Enbridge Inc.,canada,CAD,50,48.25,,0.00,Canadian dividend stock,
buy-2024-03-05-nvda,2024-03-05,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,NVDA,NVDA,NVIDIA Corporation,us,USD,5,850.00,,1.00,Before split,
sell-2024-06-20-aapl,2024-06-20,transaction,sell,WD-TFSA-Questrade,WD,TFSA,Questrade,AAPL,AAPL,Apple Inc.,us,USD,2,210.00,,1.00,Partial sale,
wd-2024-07-01-tfsa,2024-07-01,transfer,withdraw,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,1000,,Cash withdrawal,
```

## 7. Buy and Sell Rows

For buy and sell rows:

- `activity_category` should be `transaction`
- `type` should be `buy` or `sell`
- `ticker` should be filled
- `currency` should be filled
- `quantity` should be filled
- `price` should be filled
- `cash_amount` should be blank

Example:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
buy-nvda-initial,2026-04-27,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,NVDA,NVDA,NVIDIA Corporation,us,USD,200,120,,0,Initial holding based on average cost,
```

## 8. Deposit and Withdraw Rows

For deposit and withdraw rows:

- `activity_category` should be `transfer`
- `type` should be `deposit` or `withdraw`
- `currency` should be filled
- `cash_amount` should be filled
- `ticker`, `api_symbol`, `security_name`, `market`, `quantity`, `price`, and `fee` should usually be blank

Example:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
deposit-example,2026-04-27,transfer,deposit,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,5000,,Initial cash deposit,
withdraw-example,2026-04-28,transfer,withdraw,WD-TFSA-Questrade,WD,TFSA,Questrade,,,,,CAD,,,1000,,Cash withdrawal,
```

## 9. Ticker and API Symbol

Fintify separates the display ticker from the market data symbol.

| Field | Purpose | Example |
|---|---|---|
| ticker | The ticker shown in the app | `ENB` |
| api_symbol | The symbol used for market data lookup | `ENB.TO` |

For US securities, `ticker` and `api_symbol` are usually the same:

```csv
AAPL,AAPL
NVDA,NVDA
MSFT,MSFT
```

For Canadian securities, `api_symbol` usually includes `.TO`, `.V`, or `.NE`:

```csv
ENB,ENB.TO
BNS,BNS.TO
VDY,VDY.TO
```

For China A-share securities, `api_symbol` usually includes `.SS` or `.SZ`:

```csv
600036,600036.SS
601398,601398.SS
510300,510300.SS
159819,159819.SZ
```

## 10. Market Inference

The `market` column is included in exported CSV files for reference.

During import, Fintify primarily determines the market using `api_symbol` and `currency`.

Recommended approach:

- For US securities, use `currency = USD` and `api_symbol = AAPL`
- For Canadian securities, use `currency = CAD` and `api_symbol = ENB.TO`
- For China A-share securities, use `currency = CNY` and `api_symbol = 600036.SS` or `159819.SZ`

If `api_symbol` is blank, Fintify may infer the market from `currency`:

| Currency | Inferred Market |
|---|---|
| USD | United States |
| CAD | Canada |
| CNY | China A-shares |

For the most accurate results, especially outside the US market, fill in `api_symbol`.

## 11. Starting Without Complete History

You do not need perfect historical records to start using Fintify.

If you already hold a stock or ETF but do not want to enter every historical trade, you can add your current holding as one initial buy transaction.

Example:

```csv
source_record_id,date,activity_category,type,account_name,owner_name,account_type,institution,ticker,api_symbol,security_name,market,currency,quantity,price,cash_amount,fee,notes,counterparty_account_name
initial-msft,2026-04-27,transaction,buy,WD-RRSP-Questrade,WD,RRSP,Questrade,MSFT,MSFT,Microsoft Corporation,us,USD,50,320,,0,Initial holding based on average cost,
```

Use your total current shares and average cost if available.

If you do not know your average cost, you may use an estimated cost or current market price. In that case, gain/loss and historical performance may not reflect your true historical results.

## 12. Import Tips

- Use one row per activity.
- Keep dates in `YYYY-MM-DD` format when possible.
- Use lowercase or uppercase consistently for `activity_category` and `type`; Fintify accepts common casing such as `buy` or `Buy`.
- Use positive quantities for buy and sell transactions.
- Use positive cash amounts for deposits and withdrawals.
- Make sure currency codes are consistent: `USD`, `CAD`, or `CNY`.
- Keep `account_name`, `account_type`, and `institution` consistent across rows for the same account.
- Use `source_record_id` when possible to help prevent duplicate imports.
- Avoid duplicate rows unless they represent separate real activities.
- Review your portfolio after importing to confirm values look reasonable.
- Export a backup CSV before making large changes to your data.

## 13. Data Accuracy

Fintify relies on the data you enter or import.

Imported data that is incomplete, duplicated, incorrectly formatted, or inconsistent may cause inaccurate portfolio values, cost basis, gain/loss, dividend summaries, account summaries, or charts.

Third-party market data, dividends, exchange rates, and corporate actions may be delayed, incomplete, unavailable, or inaccurate.

You should verify important financial information with your brokerage, financial institution, tax advisor, or other qualified professional.

## 14. Privacy and Security

CSV files may contain investment records and other financial information.

Please store, back up, and share CSV files securely.

Fintify is designed as a local-first app. Your portfolio records are stored on your device unless you choose to export, share, or otherwise provide them outside the app.

## 15. Need Help?

For questions, feedback, or bug reports, contact:

**fintify.support@gmail.com**

When reporting an issue, please include:

- App version
- iOS version
- Device model
- A brief description of the issue
- Steps to reproduce the issue
- Screenshots, if helpful

Please do not include sensitive personal or financial information unless you choose to do so.
