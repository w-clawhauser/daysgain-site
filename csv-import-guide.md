# Fintify CSV Import Guide

Fintify supports CSV import so you can bring your investment records into the app.

CSV import is designed for users who want to migrate existing transaction history, restore data from a backup, or manage records in a spreadsheet.

## 1. What You Can Import

Fintify supports investment and cash activity records such as:

- Buy transactions
- Sell transactions
- Deposits
- Withdrawals

Dividend and split data may be retrieved from market data providers when available. Users generally do not need to manually enter dividend or split records.

## 2. Recommended Use

For the most accurate portfolio tracking, cost basis, gain/loss, and dividend summaries, import your complete transaction history when possible.

If you do not have complete historical transactions, you can still start using Fintify by adding your current holdings as initial buy transactions.

For example, if you currently hold 200 shares of NVDA with an average cost of $120, you can add or import one buy transaction:

```csv
Date,Type,Ticker,Market,Account,Owner,Quantity,Price,Currency
2026-04-27,Buy,NVDA,USA,TFSA,Owner,200,120,USD
