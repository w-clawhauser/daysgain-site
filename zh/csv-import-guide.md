---
title: DaysGain CSV 导入指南
permalink: /zh/csv-import-guide/
lang: zh-Hans
current_language: 中文
alternate_label: English
alternate_lang: en
alternate_url: https://daysgain.com/csv-import-guide/
home_url: https://daysgain.com/zh/
asset_prefix: ../../
---

# DaysGain CSV 导入指南

DaysGain 支持 CSV 导入，方便你把历史交易和现金记录批量导入应用。

CSV 导入适合用于迁移已有交易历史、从备份恢复数据，或通过电子表格管理记录。

## 最简单的开始方式

最安全的做法是先从 DaysGain 导出数据，再以导出文件作为模板编辑导入。

推荐流程：

1. 在 DaysGain 中前往 **设置 → 导出 CSV**。
2. 用电子表格软件打开导出文件。
3. 按同样的列格式添加、编辑或追加记录。
4. 另存为 CSV 格式。
5. 将更新后的 CSV 重新导入 DaysGain。

如果你还没有任何数据，请按照下方格式从头创建。

## CSV 格式

DaysGain 导入和导出使用同一种 CSV 格式。

文件必须包含表头行。列顺序不限，列名不区分大小写。

标准表头格式：

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
```

## 可以导入哪些记录

DaysGain CSV 导入支持：

- 买入
- 卖出
- 入金
- 出金

分红和拆股数据通常由市场数据源提供，不需要手动导入。

## 必填列

每个 CSV 文件必须包含以下列：

| 列名 | 示例 | 说明 |
|---|---|---|
| `date` | `2026-01-15` | 日期。推荐格式：`YYYY-MM-DD`。也接受 `YYYY/MM/DD` 或 `MM/DD/YYYY`。 |
| `activity_category` | `transaction` | 买入/卖出填 `transaction`，入金/出金填 `transfer`。不区分大小写。 |
| `type` | `buy` | 填 `buy`、`sell`、`deposit` 或 `withdraw`。不区分大小写。 |
| `account_name` | `Donald-TFSA-TD` | 账户名称。同一账户保持一致。最多约 30 个字符。 |
| `owner_name` | `Donald` | 所有者标签。最多约 10 个字符。 |
| `account_type` | `TFSA` | 账户类型，如 `TFSA`、`RRSP`、`IRA`、`Taxable`。最多约 10 个字符。 |
| `institution` | `TD` | 券商或机构名称。最多约 10 个字符。 |
| `currency` | `USD` | 必须完全匹配。支持：`USD`、`CAD`、`CNY`、`HKD`、`AUD`、`GBP`、`SGD`、`TWD`。不区分大小写。 |

## 可选列

| 列名 | 说明 |
|---|---|
| `ticker` | 股票代码。国际股票请使用完整代码，如 `601398.SS`（中国A股）、`0700.HK`（港股）、`RY.TO`（加拿大）、`HSBA.L`（英国）、`CBA.AX`（澳大利亚）、`D05.SI`（新加坡）、`2330.TW`（台湾）。 |
| `security_name` | 股票名称，如 `苹果公司`。最多约 50 个字符。 |
| `market` | `USA` / `CAN` / `CHN` / `HKG` / `AUS` / `GBR` / `SGX` / `TWS`。不区分大小写。若不填则由股票代码自动推断。 |
| `quantity` | 正整数（≥ 1）。买入/卖出时必填。 |
| `price` | 正数。买入/卖出时必填。可含逗号，如 `1,234.56`。 |
| `cash_amount` | 正数。入金/出金时必填。可含逗号。 |
| `fee` | 正数。可含逗号。默认为 0。 |
| `notes` | 备注。 |

## 记录规则

### 买入和卖出

买入/卖出行：

- `activity_category` 填 `transaction`。
- `type` 填 `buy` 或 `sell`。
- `ticker`、`quantity`、`price` 应填写。
- `cash_amount` 留空。
- `fee` 可填写手续费。

示例：

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-01-15,transaction,buy,Donald-TFSA-TD,Donald,TFSA,TD,USD,AAPL,Apple Inc.,USA,5,210.50,,9.99,
```

### 入金和出金

入金/出金行：

- `activity_category` 填 `transfer`。
- `type` 填 `deposit` 或 `withdraw`。
- `currency` 和 `cash_amount` 必须填写。
- `ticker`、`security_name`、`market`、`quantity`、`price`、`fee` 通常留空。

示例：

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-01-05,transfer,deposit,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,5000.00,,初始入金
2026-04-01,transfer,withdraw,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,1000.00,,
```

## 支持的记录类型组合

| `activity_category` | `type` | 含义 |
|---|---|---|
| `transaction` | `buy` | 买入证券 |
| `transaction` | `sell` | 卖出证券 |
| `transfer` | `deposit` | 账户入金 |
| `transfer` | `withdraw` | 账户出金 |

## 股票代码与市场

`ticker` 字段填写完整代码，包括交易所后缀。

| 市场 | `market` 值 | `ticker` 示例 | 货币 |
|---|---|---|---|
| 美股 / ETF | `USA` | `AAPL` | `USD` |
| 加拿大股票 / ETF | `CAN` | `RY.TO` | `CAD` |
| 中国A股（上交所） | `CHN` | `601398.SS` | `CNY` |
| 中国A股（深交所） | `CHN` | `000651.SZ` | `CNY` |
| 港股 | `HKG` | `0700.HK` | `HKD` |
| 澳大利亚股票 | `AUS` | `CBA.AX` | `AUD` |
| 英国股票 | `GBR` | `HSBA.L` | `GBP` |
| 新加坡股票 | `SGX` | `D05.SI` | `SGD` |
| 台湾股票 | `TWS` | `2330.TW` | `TWD` |

市场推断优先级：

1. `market` 字段（如已填写）。
2. 根据股票代码后缀自动推断。
3. 根据 `currency` 推断（兜底）。

不同市场和代码的数据可用性可能因数据提供方而异。

## 示例 CSV

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-01-05,transfer,deposit,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,5000.00,,初始入金
2026-01-15,transaction,buy,Donald-TFSA-TD,Donald,TFSA,TD,USD,AAPL,Apple Inc.,USA,5,210.50,,9.99,
2026-01-20,transaction,buy,Donald-RRSP-TD,Donald,RRSP,TD,CAD,RY.TO,Royal Bank of Canada,CAN,10,132.80,,0,
2026-02-10,transaction,buy,Donald-Taxable-IBKR,Donald,Taxable,IBKR,HKD,0700.HK,Tencent Holdings,HKG,100,380.00,,0,
2026-02-20,transaction,buy,Donald-Taxable-IBKR,Donald,Taxable,IBKR,CNY,601398.SS,ICBC,CHN,1000,6.52,,0,
2026-03-15,transaction,sell,Donald-TFSA-TD,Donald,TFSA,TD,USD,AAPL,Apple Inc.,USA,2,245.00,,9.99,部分卖出
2026-04-01,transfer,withdraw,Donald-TFSA-TD,Donald,TFSA,TD,CAD,,,,,,1000.00,,
```

## 没有完整历史记录怎么办

你不需要提供全部历史交易记录，才能开始使用 DaysGain。

如果你已有持仓但没有完整历史，可以用当前总股数和平均成本添加一笔初始买入记录。

示例：

```csv
date,activity_category,type,account_name,owner_name,account_type,institution,currency,ticker,security_name,market,quantity,price,cash_amount,fee,notes
2026-04-28,transaction,buy,Donald-RRSP-TD,Donald,RRSP,TD,USD,MSFT,Microsoft Corp.,USA,50,320.00,,0,以平均成本录入的初始持仓
```

DaysGain 将从该日期开始追踪你的持仓。该日期之前的历史表现不会被反映。

如果不知道精确平均成本，可以使用估算值或当前市价。收益计算将以你录入的成本为准。

## 导入建议

- 每一行代表一笔真实记录。
- 同一账户的 `account_name`、`owner_name`、`account_type`、`institution` 应在所有行中保持一致。
- 日期尽量使用 `YYYY-MM-DD` 格式。
- 买入和卖出数量使用正数。
- 入金和出金金额使用正数。
- 大规模导入前，建议先导出现有数据作为备份。
- 导入后检查持仓和数值是否正确。

## 数据准确性

DaysGain 的计算依赖你输入或导入的数据。

不完整、重复、格式错误或不一致的数据可能导致组合市值、成本、收益、分红汇总、账户汇总或图表不准确。

第三方市场数据、分红、汇率和公司事件可能存在延迟、不完整、不可用或不准确的情况。

重要财务信息请以你的券商、金融机构、税务顾问或其他合格专业人士提供的信息为准。

## 隐私与安全

CSV 文件可能包含投资记录和其他财务信息。

请安全地保存、备份和分享 CSV 文件。

DaysGain 采用本地优先设计。你的投资记录默认保存在设备本地，除非你主动导出、分享或通过其他方式提供到应用之外。

## 需要帮助？

如有问题、反馈或错误报告，请联系：

**hello@daysgain.com**

报告导入问题时，请提供：

- App 版本
- iOS 版本
- 设备型号
- 问题描述
- 导入失败的示例行（请删除敏感信息）

请勿包含敏感的个人或财务信息，除非你主动选择提供。
