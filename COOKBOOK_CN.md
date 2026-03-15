# InvestSkill Cookbook

面向当前 `v1.4.0` 技能集整理的实用安装说明、工作流模式与示例。

> 相关文档：[README](README_CN.md) | [繁體中文](COOKBOOK-zh-TW.md) | [English](COOKBOOK.md) | [Gemini CLI](GEMINI.md)

## 1. 安装

### 从市场安装

```bash
claude

/plugin marketplace add yennanliu/InvestSkill
/plugin install us-stock-analysis@invest-skill
/plugin list
```

预期结果：`us-stock-analysis` 会出现在列表中，并显示 `18` 个技能。

### 本地安装

```bash
claude

/plugin marketplace add /path/to/InvestSkill
/plugin install us-stock-analysis@invest-skill
```

### 快速验证

```bash
/us-stock-analysis:stock-eval AAPL
```

如果输出末尾有 `INVESTMENT SIGNAL` 区块，说明安装正常。

## 2. 使用心智模型

InvestSkill 最适合被当作一套研究系统，而不是一组零散命令。

### 核心分层

| 层级 | 技能 | 目的 |
| --- | --- | --- |
| 商业质量 | `fundamental-analysis`、`competitor-analysis`、`financial-report-analyst` | 了解护城河、会计质量与企业健康度 |
| 估值 | `stock-valuation`、`dcf-valuation`、`stock-eval` | 估算内在价值并与市场价格比较 |
| 市场定位 | `insider-trading`、`institutional-ownership`、`earnings-call-analysis`、`short-interest`、`options-analysis` | 观察管理层、机构与市场在传递什么信号 |
| 时机与环境 | `technical-analysis`、`economics-analysis`、`sector-analysis` | 判断什么时候做、在什么环境下做 |
| 最终整合 | `research-bundle`、`report-generator` | 将多个输出整合成统一论点或可交付报告 |

## 3. 命令模式

### 单技能命令

```bash
/us-stock-analysis:fundamental-analysis MSFT
```

### 带 flag 的命令

```bash
/us-stock-analysis:technical-analysis NVDA --chart
/us-stock-analysis:fundamental-analysis AMZN --visual
/us-stock-analysis:stock-valuation GOOGL --full
```

### 多步骤流程

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:stock-valuation AAPL --full
/us-stock-analysis:research-bundle AAPL --visual
```

## 4. 工作流配方

### 配方 A：建仓前完整尽调

适合需要产出完整长篇投资论点时使用。

```bash
/us-stock-analysis:financial-report-analyst MSFT 10-K --full
/us-stock-analysis:competitor-analysis MSFT
/us-stock-analysis:stock-valuation MSFT --full
/us-stock-analysis:research-bundle MSFT --visual
/us-stock-analysis:report-generator --type comprehensive
```

每一步的作用：

- `financial-report-analyst`：抓财报质量、会计红旗和风险因子变化
- `competitor-analysis`：确认护城河与行业结构
- `stock-valuation`：产出多方法目标价区间
- `research-bundle`：加权整合成统一论点
- `report-generator`：导出 HTML/PDF 成品

### 配方 B：快速想法筛选

适合先判断一只股票值不值得深入研究。

```bash
/us-stock-analysis:stock-eval META
/us-stock-analysis:technical-analysis META
/us-stock-analysis:insider-trading META
```

解读方式：

- `stock-eval` 回答“这家公司和估值是否有吸引力？”
- `technical-analysis` 回答“现在是不是合适的介入点？”
- `insider-trading` 回答“内部人是否支持这个论点？”

### 配方 C：收益型股票评估

```bash
/us-stock-analysis:dividend-analysis JNJ
/us-stock-analysis:fundamental-analysis JNJ
/us-stock-analysis:stock-valuation JNJ
```

适合确认：

- 股息是否可持续
- 派息覆盖率与衰退承受力
- 高股息率是否其实是价值陷阱

### 配方 D：财报事件前准备

```bash
/us-stock-analysis:earnings-call-analysis NVDA
/us-stock-analysis:options-analysis NVDA --earnings
/us-stock-analysis:technical-analysis NVDA --chart
```

适合确认：

- 指引语气与管理层可信度
- 预期波动和 IV 状态
- 财报前的关键价格区间

### 配方 E：自上而下的市场流程

```bash
/us-stock-analysis:economics-analysis
/us-stock-analysis:sector-analysis
/us-stock-analysis:stock-eval XOM
```

适合：

- 先判断宏观环境
- 再收敛到行业
- 最后才挑选个股

## 5. 最新技能如何使用

### `financial-report-analyst`

当投资论点高度依赖原始文件时，这个技能最有价值。

常见用法：

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:financial-report-analyst MSFT 10-Q --section risk-factors
/us-stock-analysis:financial-report-analyst NVDA --compare-prior
```

重点观察：

- 营收和利润率叙述是否能和数字自洽
- DSO、库存、递延收入是否异常变动
- 风险因子措辞是否新增或升级
- 非 GAAP 调整或 SBC 是否明显上升

### `stock-valuation`

当单一 DCF 过于狭窄、需要多方法交叉验证时使用。

常见用法：

```bash
/us-stock-analysis:stock-valuation AAPL
/us-stock-analysis:stock-valuation MSFT --methods dcf,cca,ev-ebitda
/us-stock-analysis:stock-valuation BRK.B --full
```

用途：

- 用多种估值方法互相验证
- 看出假设对结果有多脆弱
- 输出加权后的价值区间，而不是单一数字

## 6. 数据源纪律

最新技能集对数据质量要求更严格。

优先顺序：

1. SEC 申报、年报、8-K 财报材料、公司 IR 页面
2. 管理层指引与 investor presentation
3. 可归因的市场输入数据
4. 第三方财经网站仅作交叉验证

不要因为某个财经网站不可用就中止整个流程。应该改用可获得的一手资料，并在必要时降低信心等级。

若 peer multiple 或一致预期数据缺失：

- 扩大输出区间
- 明确说明缺口
- 不要制造虚假的精确度

## 7. 跨 AI 使用

InvestSkill 在 `prompts/` 中提供 `17` 份通用 prompt。

### Gemini 示例

```text
@prompts/stock-valuation.md Analyze AAPL using all valuation methods
@prompts/research-bundle.md Build a complete thesis for AAPL
```

### Copilot 或 Cursor 示例

```text
Use prompts/financial-report-analyst.md to review this 10-Q excerpt.
Use prompts/technical-analysis.md to analyze TSLA.
```

`report-generator` 是主要仍保留在 Claude plugin 侧的能力。

## 8. 输出标准

每个技能都应以统一的信号区块结尾：

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

这个区块是结论压缩层，不是对前面分析过程的替代。

## 9. 常见错误

- 长期投资只看 `technical-analysis`，却跳过商业质量与财报质量
- 在输入很弱的情况下，把 `stock-valuation` 当成精确定价工具
- 完全依赖第三方财经网站，而不查看原始申报文件
- 当论点与会计质量高度相关时，却没有运行 `financial-report-analyst`
- 估值、技术面和筹码信号彼此冲突时，直接生成报告而不先解决矛盾

## 10. 推荐起手式

根据你的问题选择第一个技能：

- “这家公司值得研究吗？” -> `stock-eval`
- “它的合理价值是多少？” -> `stock-valuation`
- “这份财报到底变了什么？” -> `financial-report-analyst`
- “现在适合进场吗？” -> `technical-analysis`
- “我要一份完整论点。” -> `research-bundle`
- “我要一份可交付的报告。” -> `report-generator`
