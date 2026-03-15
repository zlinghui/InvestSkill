# InvestSkill 操作手冊

針對目前 `v1.4.0` 技能集整理的實用安裝說明、工作流程與使用範例。

> 相關文件：[README](README-zh-TW.md) | [English](COOKBOOK.md) | [Gemini CLI](GEMINI.md)

## 1. 安裝

### 從市集安裝

```bash
claude

/plugin marketplace add yennanliu/InvestSkill
/plugin install us-stock-analysis@invest-skill
/plugin list
```

預期結果：`us-stock-analysis` 出現在清單中，且顯示 `18` 個技能。

### 本地安裝

```bash
claude

/plugin marketplace add /path/to/InvestSkill
/plugin install us-stock-analysis@invest-skill
```

### 快速驗證

```bash
/us-stock-analysis:stock-eval AAPL
```

如果輸出最後有 `INVESTMENT SIGNAL` 區塊，表示安裝正常。

## 2. 使用心法

InvestSkill 最適合被當成一套研究系統，而不是零散指令集合。

### 核心分層

| 層級 | 技能 | 目的 |
| --- | --- | --- |
| 商業品質 | `fundamental-analysis`、`competitor-analysis`、`financial-report-analyst` | 了解護城河、會計品質與企業健康度 |
| 估值 | `stock-valuation`、`dcf-valuation`、`stock-eval` | 估算內在價值並與市場價格比較 |
| 市場定位 | `insider-trading`、`institutional-ownership`、`earnings-call-analysis`、`short-interest`、`options-analysis` | 觀察管理層、機構與市場在傳遞什麼訊號 |
| 時機與環境 | `technical-analysis`、`economics-analysis`、`sector-analysis` | 判斷何時做、在哪個市場環境做 |
| 最終整合 | `research-bundle`、`report-generator` | 將多個輸出整合成統一論點或正式報告 |

## 3. 指令模式

### 單一技能

```bash
/us-stock-analysis:fundamental-analysis MSFT
```

### 帶旗標的技能

```bash
/us-stock-analysis:technical-analysis NVDA --chart
/us-stock-analysis:fundamental-analysis AMZN --visual
/us-stock-analysis:stock-valuation GOOGL --full
```

### 多步驟流程

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:stock-valuation AAPL --full
/us-stock-analysis:research-bundle AAPL --visual
```

## 4. 工作流程配方

### 配方 A：開倉前完整盡調

適合要產出完整長篇投資論點時使用。

```bash
/us-stock-analysis:financial-report-analyst MSFT 10-K --full
/us-stock-analysis:competitor-analysis MSFT
/us-stock-analysis:stock-valuation MSFT --full
/us-stock-analysis:research-bundle MSFT --visual
/us-stock-analysis:report-generator --type comprehensive
```

每一步的角色：

- `financial-report-analyst`：抓財報品質、會計紅旗與風險因子變化
- `competitor-analysis`：確認護城河與產業結構
- `stock-valuation`：產出多方法目標價區間
- `research-bundle`：加權整合成一份論點
- `report-generator`：匯出 HTML/PDF 成品

### 配方 B：快速想法篩選

適合先判斷一檔股票值不值得深挖。

```bash
/us-stock-analysis:stock-eval META
/us-stock-analysis:technical-analysis META
/us-stock-analysis:insider-trading META
```

解讀方式：

- `stock-eval` 回答「這家公司與估值有沒有吸引力？」
- `technical-analysis` 回答「現在是不是合適的切入點？」
- `insider-trading` 回答「內部人是否支持這個論點？」

### 配方 C：收益型股票檢查

```bash
/us-stock-analysis:dividend-analysis JNJ
/us-stock-analysis:fundamental-analysis JNJ
/us-stock-analysis:stock-valuation JNJ
```

適合用來確認：

- 股息是否可持續
- payout coverage 與景氣衰退承受力
- 高殖利率是否其實是價值陷阱

### 配方 D：財報事件前準備

```bash
/us-stock-analysis:earnings-call-analysis NVDA
/us-stock-analysis:options-analysis NVDA --earnings
/us-stock-analysis:technical-analysis NVDA --chart
```

適合用來確認：

- 指引語氣與管理層可信度
- 預期波動與 IV 狀態
- 財報前的重要價位

### 配方 E：由上而下的市場流程

```bash
/us-stock-analysis:economics-analysis
/us-stock-analysis:sector-analysis
/us-stock-analysis:stock-eval XOM
```

適合：

- 先判斷總體環境
- 再收斂到產業
- 最後才挑個股

## 5. 最新技能怎麼用

### `financial-report-analyst`

當投資論點高度依賴原始文件時，這個技能最有價值。

常見用法：

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:financial-report-analyst MSFT 10-Q --section risk-factors
/us-stock-analysis:financial-report-analyst NVDA --compare-prior
```

重點觀察：

- 營收與利潤率敘述是否能和數字自洽
- DSO、存貨、遞延收入是否異常變化
- 風險因子措辭是否新增或升級
- 非 GAAP 調整或 SBC 是否明顯抬升

### `stock-valuation`

當單純 DCF 太窄、需要多方法交叉驗證時使用。

常見用法：

```bash
/us-stock-analysis:stock-valuation AAPL
/us-stock-analysis:stock-valuation MSFT --methods dcf,cca,ev-ebitda
/us-stock-analysis:stock-valuation BRK.B --full
```

用途：

- 用多種估值方法互相驗證
- 看出假設對結果有多脆弱
- 輸出加權後的價值區間，而不是單一數字

## 6. 資料來源紀律

最新技能集對資料品質要求更嚴格。

優先順序：

1. SEC 申報、年報、8-K 財報材料、公司 IR 頁面
2. 管理層指引與 investor presentation
3. 可歸因的市場輸入資料
4. 第三方財經網站僅作交叉驗證

不要因為某個財經網站打不開就中止整個流程。應改用可取得的一手資料，必要時降低信心等級。

若 peer multiple 或市場共識資料缺失：

- 擴大輸出區間
- 明確說明缺口
- 不要製造虛假的精確度

## 7. 跨 AI 使用

InvestSkill 在 `prompts/` 中提供 `17` 份通用 prompt。

### Gemini 範例

```text
@prompts/stock-valuation.md Analyze AAPL using all valuation methods
@prompts/research-bundle.md Build a complete thesis for AAPL
```

### Copilot 或 Cursor 範例

```text
Use prompts/financial-report-analyst.md to review this 10-Q excerpt.
Use prompts/technical-analysis.md to analyze TSLA.
```

`report-generator` 是主要仍保留在 Claude plugin 端的能力。

## 8. 輸出標準

每個技能都應以統一的訊號區塊結尾：

```text
╔══════════════════════════════════════════════╗
║              INVESTMENT SIGNAL               ║
╠══════════════════════════════════════════════╣
║ Signal:      BULLISH / NEUTRAL / BEARISH     ║
║ Confidence:  HIGH / MEDIUM / LOW             ║
║ Horizon:     SHORT / MEDIUM / LONG-TERM      ║
║ Score:       X.X / 10                        ║
╠══════════════════════════════════════════════╣
║ Action:      BUY / HOLD / SELL               ║
║ Conviction:  STRONG / MODERATE / WEAK        ║
╚══════════════════════════════════════════════╝
```

這個區塊是壓縮結論，不是取代前面分析過程。

## 9. 常見錯誤

- 長期投資只看 `technical-analysis`，卻跳過商業品質與財報品質
- 在輸入很弱的情況下，把 `stock-valuation` 當成精準定價工具
- 完全依賴第三方財經網站，而不看原始申報文件
- 論點與會計品質高度相關時，卻沒有跑 `financial-report-analyst`
- 估值、技術面、籌碼訊號彼此衝突時，直接生成報告而不先解決矛盾

## 10. 建議起手式

依照你的問題選第一個技能：

- 「這家公司值得研究嗎？」 -> `stock-eval`
- 「它合理價值是多少？」 -> `stock-valuation`
- 「這份財報到底變了什麼？」 -> `financial-report-analyst`
- 「現在適合進場嗎？」 -> `technical-analysis`
- 「我要一份完整論點。」 -> `research-bundle`
- 「我要一份可以交付的報告。」 -> `report-generator`
