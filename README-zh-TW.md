# InvestSkill

適用於 Claude Code、Gemini CLI、GitHub Copilot、Cursor 與其他 AI 助手的美股分析技能集。

> 目前版本：`v1.4.0`
>
> 文件入口：[操作手冊](COOKBOOK-zh-TW.md) | [English](README.md) | [網站](https://yennanliu.github.io/InvestSkill/)

[![Deploy](https://github.com/yennanliu/InvestSkill/actions/workflows/deploy-pages.yml/badge.svg)](https://github.com/yennanliu/InvestSkill/actions/workflows/deploy-pages.yml)
[![Validate](https://github.com/yennanliu/InvestSkill/actions/workflows/validate.yml/badge.svg)](https://github.com/yennanliu/InvestSkill/actions/workflows/validate.yml)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 概述

InvestSkill 把完整的美股研究流程封裝成可重複使用的分析技能：

- `18` 個 Claude Code 技能，位於 `plugins/us-stock-analysis/skills/`
- `17` 份跨 AI 可重用提示檔，位於 `prompts/`
- 統一的 `INVESTMENT SIGNAL` 訊號區塊
- 涵蓋基本面、技術面、估值、財報文件、資金流向與投資組合檢視

目前技能集裡最新加入的核心分析能力是：

- `/financial-report-analyst`：10-K、10-Q、委託書與財報品質分析
- `/stock-valuation`：DCF、同業比較、EV 倍數、Residual Income 多方法估值

`/report-generator` 仍屬於 Claude Code 外掛技能，沒有對應的獨立 prompt 檔。

## 安裝

### 從市集安裝

```bash
claude

/plugin marketplace add yennanliu/InvestSkill
/plugin install us-stock-analysis@invest-skill
/plugin list
```

### 本地開發安裝

```bash
claude

/plugin marketplace add /path/to/InvestSkill
/plugin install us-stock-analysis@invest-skill
```

安裝後，`plugin list` 應顯示 `us-stock-analysis`，且包含 `18` 個技能。

## 快速開始

```bash
# 股票全方位評估
/us-stock-analysis:stock-eval AAPL

# 財報文件分析
/us-stock-analysis:financial-report-analyst MSFT 10-K

# 多方法估值
/us-stock-analysis:stock-valuation NVDA --full

# 完整研究流程
/us-stock-analysis:research-bundle AMZN
```

部分 Claude Code 環境也支援 `/stock-eval` 這類短指令，但文件統一以 namespaced 形式為準。

## 技能地圖

### 核心分析

| 技能 | 功能 |
| --- | --- |
| `/stock-eval` | 從商業品質、估值、護城河與風險角度做整體評估 |
| `/economics-analysis` | 美國總體經濟、利率、信用條件與市場含意 |
| `/fundamental-analysis` | 財務報表、商業品質與可選的視覺化輸出 |
| `/technical-analysis` | 多時間框架、技術指標、支撐壓力與圖表模式 |
| `/portfolio-review` | 資產配置、分散程度、集中風險與組合診斷 |
| `/sector-analysis` | 產業輪動、景氣位置與相對機會 |

### 市場資料與報告

| 技能 | 功能 |
| --- | --- |
| `/report-generator` | 產生 HTML/PDF 投資報告與圖表 |
| `/earnings-call-analysis` | 法說會逐字稿情緒、指引品質與管理層語氣 |
| `/insider-trading` | SEC Form 4 內部人交易與訊號解讀 |
| `/institutional-ownership` | 13F 機構持股變化與聰明錢動向 |

### 進階研究

| 技能 | 功能 |
| --- | --- |
| `/dividend-analysis` | 股息安全性、可持續性與收益型投資分析 |
| `/short-interest` | 放空部位、軋空條件與空方壓力 |
| `/options-analysis` | Greeks、隱含波動率與策略選擇 |
| `/research-bundle` | 串接多個技能，輸出統一投資論點 |
| `/dcf-valuation` | 情境式 DCF 內在價值估算與敏感度表 |
| `/competitor-analysis` | 護城河、產業結構與同業競爭定位 |
| `/financial-report-analyst` | 深讀 10-K/10-Q、會計品質與紅旗訊號 |
| `/stock-valuation` | DCF + comps + EV 倍數 + residual income 估值整合 |

## 跨 AI 使用方式

InvestSkill 在 [prompts/](prompts) 中提供 `17` 份分析 prompt，可用於 Claude Code 以外的 AI 工具。

### Gemini CLI

Gemini 會自動讀取 [GEMINI.md](GEMINI.md)。範例：

```bash
gemini
```

```text
@prompts/stock-valuation.md Analyze AAPL using DCF, comps, and residual income
@prompts/financial-report-analyst.md Review this 10-Q excerpt for red flags
@prompts/research-bundle.md Build a full thesis for AMZN
```

### GitHub Copilot 與 Cursor

- GitHub Copilot 使用 `.github/copilot-instructions.md`
- Cursor 使用 `.cursor/rules/invest-skill.mdc`

也可以直接在對話中引用 prompt：

```text
Use prompts/fundamental-analysis.md to analyze MSFT
Use prompts/technical-analysis.md to review TSLA
```

### Prompt 覆蓋範圍

`prompts/` 目前包含以下分析提示：

- `competitor-analysis`
- `dcf-valuation`
- `dividend-analysis`
- `earnings-call-analysis`
- `economics-analysis`
- `financial-report-analyst`
- `fundamental-analysis`
- `insider-trading`
- `institutional-ownership`
- `options-analysis`
- `portfolio-review`
- `research-bundle`
- `sector-analysis`
- `short-interest`
- `stock-eval`
- `stock-valuation`
- `technical-analysis`

刻意沒有 `report-generator.md`，因為報告生成由 Claude 外掛技能處理。

## 建議工作流程

### 完整盡職調查

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:stock-valuation AAPL --full
/us-stock-analysis:research-bundle AAPL --visual
/us-stock-analysis:report-generator --type comprehensive
```

### 快速基本面流程

```bash
/us-stock-analysis:fundamental-analysis MSFT --visual
/us-stock-analysis:dcf-valuation MSFT
/us-stock-analysis:competitor-analysis MSFT
```

### 事件驅動流程

```bash
/us-stock-analysis:earnings-call-analysis NVDA
/us-stock-analysis:insider-trading NVDA
/us-stock-analysis:options-analysis NVDA --earnings
```

## 分析標準

最新技能集採用更嚴格的資料來源規則：

- 優先使用 SEC 申報、公司 IR 資料、年報與可歸因的一手來源
- 第三方財經網站只用來交叉驗證，不作為硬性依賴
- 若關鍵資料缺失，應擴大估值區間並明確揭露限制
- 在 peer 或共識數據不足時，不要製造虛假的精確度

每次分析都應以標準化的 `INVESTMENT SIGNAL` 區塊結尾：

```text
Signal:      BULLISH / NEUTRAL / BEARISH
Confidence:  HIGH / MEDIUM / LOW
Horizon:     SHORT / MEDIUM / LONG-TERM
Score:       0.0 - 10.0
Action:      BUY / HOLD / SELL
Conviction:  STRONG / MODERATE / WEAK
```

## 專案結構

```text
InvestSkill/
├── .claude-plugin/
├── .cursor/
├── .github/
├── plugins/us-stock-analysis/
│   ├── .claude-plugin/plugin.json
│   └── skills/
├── prompts/
├── COOKBOOK.md
├── COOKBOOK-zh-TW.md
├── GEMINI.md
├── README.md
└── README-zh-TW.md
```

## 免責聲明

本專案僅供教育與研究流程使用，不構成投資建議。做出資本配置決策前，請自行驗證所有資料、假設與結論。
