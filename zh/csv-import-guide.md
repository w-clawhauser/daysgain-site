---
title: Fintify CSV 导入指南
permalink: /zh/csv-import-guide/
lang: zh-Hans
current_language: 中文
alternate_label: English
alternate_lang: en
alternate_url: /fintify-site/csv-import-guide/
home_url: /fintify-site/zh/
---

# Fintify CSV 导入指南

Fintify 支持 CSV 导入，方便你把历史交易记录导入应用。

CSV 导入适合用于迁移已有交易历史、从备份恢复数据，或通过电子表格管理记录。

## 1. 可以导入哪些记录？

Fintify 支持导入以下投资和现金活动记录：

- 买入
- 卖出
- 入金
- 出金

分红和拆股数据通常会在可用时由市场数据源提供，用户一般不需要手动录入分红或拆股记录。

## 2. 推荐用法

为了更准确地追踪投资组合、市值、成本、收益和分红，建议在可能的情况下导入完整交易历史。

如果你没有完整历史记录，也可以从当前持仓开始使用 Fintify。做法是用当前总股数和平均成本添加一笔初始买入记录。

例如，如果你当前持有 200 股 NVDA，平均成本为 120 美元，可以导入一行：

```csv
Date,Type,Ticker,AccountName,AccountType,Institution,OwnerName,Quantity,Price,Currency,Fees,Notes
2026-04-27,Buy,NVDA,RRSP,RRSP,Questrade,WD,200,120,USD,0,Initial holding based on average cost
```

这样 Fintify 可以从该日期开始追踪当前持仓。该日期之前的历史表现可能无法完全准确。

## 3. CSV 格式

CSV 文件应包含表头行。

完整列格式：

```csv
Date,Type,Ticker,AccountName,AccountType,Institution,OwnerName,Quantity,Price,Currency,Fees,Notes
```

## 4. 字段说明

| 字段 | 是否必填 | 说明 |
| --- | --- | --- |
| Date | 必填 | 交易日期，推荐格式：`YYYY-MM-DD` |
| Type | 必填 | 交易类型：`Buy`、`Sell`、`Deposit`、`Withdraw` |
| Ticker | 买入/卖出必填 | 股票或 ETF 代码，例如 `AAPL`、`NVDA`、`ENB.TO`、`600036.SS`。入金/出金可留空 |
| AccountName | 必填 | 账户名称，例如 `TFSA`、`RRSP`，或你自己的账户名称 |
| AccountType | 必填 | 账户类型，例如 `TFSA`、`RRSP`、`Cash`、`Non-registered` |
| Institution | 必填 | 券商或机构名称，例如 `Questrade`、`Wealthsimple`、`RBC` |
| OwnerName | 可选 | 所有者标签，适合多个人或多个投资组合 |
| Quantity | 买入/卖出必填 | 股数或份额数量 |
| Price | 买入/卖出必填 | 每股或每份价格 |
| Currency | 必填 | 交易货币：`USD`、`CAD` 或 `CNY` |
| Fees | 可选 | 手续费或佣金 |
| Notes | 可选 | 备注 |

## 5. 市场和代码格式

Fintify 会根据代码格式自动判断市场，不需要额外提供 market 字段。

| 市场 | 示例代码 | 格式 |
| --- | --- | --- |
| 美国股票/ETF | `AAPL`、`MSFT`、`NVDA`、`VOO` | 普通代码，无后缀 |
| 加拿大股票/ETF | `ENB.TO`、`RY.TO`、`VDY.TO`、`XEI.TO` | 使用 `.TO` 后缀 |
| 中国 A 股/ETF | `600036.SS`、`601398.SS`、`510300.SS`、`000651.SZ` | 使用 `.SS` 或 `.SZ` 后缀 |

不同市场和代码的数据可用性可能因数据提供方而异。

## 6. 示例 CSV

```csv
Date,Type,Ticker,AccountName,AccountType,Institution,OwnerName,Quantity,Price,Currency,Fees,Notes
2024-01-15,Deposit,,,TFSA,Questrade,WD,,,CAD,0,Initial deposit
2024-01-16,Buy,AAPL,TFSA,TFSA,Questrade,WD,10,185.50,USD,1.00,Initial position
2024-02-10,Buy,ENB.TO,TFSA,TFSA,Questrade,WD,50,48.25,CAD,0,Canadian dividend stock
2024-06-20,Sell,AAPL,TFSA,TFSA,Questrade,WD,2,210.00,USD,1.00,Partial sale
2024-07-01,Withdraw,,,TFSA,Questrade,WD,,,CAD,0,Cash withdrawal
```

## 7. 导入建议

- 每一行代表一笔真实记录
- 日期尽量使用 `YYYY-MM-DD`
- 买入和卖出数量使用正数
- 入金和出金金额使用正数
- 货币代码保持一致：`USD`、`CAD` 或 `CNY`
- 同一个账户的 `AccountName`、`AccountType` 和 `Institution` 应保持一致
- 避免重复导入相同记录
- 大规模导入前，建议先导出现有数据作为备份
- 导入后，请检查组合数值是否合理

## 8. 数据准确性

Fintify 的计算依赖你输入或导入的数据。

不完整、重复、格式错误或不一致的数据可能导致组合市值、成本、收益、分红汇总、账户汇总或图表不准确。

第三方市场数据、分红、汇率和公司事件也可能存在延迟、不完整、不可用或不准确。

重要财务信息请以你的券商、金融机构、税务顾问或其他合格专业人士提供的信息为准。

## 9. 隐私与安全

CSV 文件可能包含投资记录和其他财务信息。

请安全地保存、备份和分享 CSV 文件。

Fintify 采用本地优先设计。你的投资记录默认保存在设备本地，除非你主动导出、分享或通过其他方式提供到应用之外。

## 10. 需要帮助？

如有问题、反馈或错误报告，请联系：

**fintify.support@gmail.com**
