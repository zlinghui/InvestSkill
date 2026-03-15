# InvestSkill

适用于 Claude Code、Gemini CLI、GitHub Copilot、Cursor 及其他 AI 助手的美股分析技能集。

> 当前版本：`v1.4.0`
>
> 文档入口：[Cookbook](COOKBOOK_CN.md) | [繁體中文](README-zh-TW.md) | [English](README.md) | [网站](https://yennanliu.github.io/InvestSkill/)

[![Deploy](https://github.com/yennanliu/InvestSkill/actions/workflows/deploy-pages.yml/badge.svg)](https://github.com/yennanliu/InvestSkill/actions/workflows/deploy-pages.yml)
[![Validate](https://github.com/yennanliu/InvestSkill/actions/workflows/validate.yml/badge.svg)](https://github.com/yennanliu/InvestSkill/actions/workflows/validate.yml)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 概览

InvestSkill 将完整的美股研究流程封装为可复用的分析技能：

- `18` 个 Claude Code 技能，位于 `plugins/us-stock-analysis/skills/`
- `17` 份跨 AI 通用 prompt，位于 `prompts/`
- 统一的 `INVESTMENT SIGNAL` 信号区块
- 覆盖基本面、技术面、估值、财务文件、市场定位和投资组合分析

当前技能集里最新加入的核心分析能力是：

- `/financial-report-analyst`：10-K、10-Q、委托书及财报质量分析
- `/stock-valuation`：DCF、可比公司、EV 倍数、Residual Income 多方法估值

`/report-generator` 仍然是 Claude Code 插件技能，没有对应的独立 prompt 文件。

## 安装

### 从市场安装

```bash
claude

/plugin marketplace add yennanliu/InvestSkill
/plugin install us-stock-analysis@invest-skill
/plugin list
```

### 本地开发安装

```bash
claude

/plugin marketplace add /path/to/InvestSkill
/plugin install us-stock-analysis@invest-skill
```

安装后，你应该能在列表中看到带有 `18` 个技能的 `us-stock-analysis`。

## 快速开始

```bash
# 股票全景评估
/us-stock-analysis:stock-eval AAPL

# 财务文件分析
/us-stock-analysis:financial-report-analyst MSFT 10-K

# 多方法估值
/us-stock-analysis:stock-valuation NVDA --full

# 完整研究流程
/us-stock-analysis:research-bundle AMZN
```

部分 Claude Code 环境也支持 `/stock-eval` 这样的短命令，但文档统一以 namespaced 形式为准。

## 技能地图

### 核心分析

| 技能 | 覆盖内容 |
| --- | --- |
| `/stock-eval` | 从质量、估值、护城河和风险角度做整体评估 |
| `/economics-analysis` | 美国宏观指标、利率、信用环境和市场含义 |
| `/fundamental-analysis` | 财务报表分析、商业质量及可选的可视化输出 |
| `/technical-analysis` | 多时间框架技术结构、指标、支撑阻力与图表模式 |
| `/portfolio-review` | 配置、分散度、集中风险和组合诊断 |
| `/sector-analysis` | 行业轮动、周期位置和相对机会 |

### 市场数据与报告

| 技能 | 覆盖内容 |
| --- | --- |
| `/report-generator` | 生成带图表与信号区块的 HTML/PDF 投资报告 |
| `/earnings-call-analysis` | 财报电话会逐字稿情绪、指引质量和管理层语气 |
| `/insider-trading` | SEC Form 4 内部人交易与信号解读 |
| `/institutional-ownership` | 13F 持仓变化与聪明钱定位 |

### 进阶研究

| 技能 | 覆盖内容 |
| --- | --- |
| `/dividend-analysis` | 股息安全性、派息可持续性和收益型投资 |
| `/short-interest` | 空头仓位、逼空条件和看空压力 |
| `/options-analysis` | Greeks、隐含波动率和策略选择 |
| `/research-bundle` | 串联多项技能并输出统一投资论点 |
| `/dcf-valuation` | 带情景与敏感度表的内在价值建模 |
| `/competitor-analysis` | 护城河强度、市场结构和同业对比 |
| `/financial-report-analyst` | 深读 10-K/10-Q、会计质量与文件红旗 |
| `/stock-valuation` | DCF + 可比公司 + EV 倍数 + residual income 综合估值 |

## 跨 AI 使用方式

InvestSkill 在 [prompts/](prompts) 中提供 `17` 份 prompt，可用于 Claude Code 之外的 AI 工具。

### Gemini CLI

Gemini 会自动读取 [GEMINI.md](GEMINI.md)。示例：

```bash
gemini
```

```text
@prompts/stock-valuation.md Analyze AAPL using DCF, comps, and residual income
@prompts/financial-report-analyst.md Review this 10-Q excerpt for red flags
@prompts/research-bundle.md Build a full thesis for AMZN
```

### GitHub Copilot 与 Cursor

- GitHub Copilot 使用 `.github/copilot-instructions.md`
- Cursor 使用 `.cursor/rules/invest-skill.mdc`

你也可以在聊天中直接引用 prompt 文件：

```text
Use prompts/fundamental-analysis.md to analyze MSFT
Use prompts/technical-analysis.md to review TSLA
```

### Prompt 覆盖范围

`prompts/` 目录当前包含以下分析提示：

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

有意不提供 `report-generator.md`，因为报告生成由 Claude 插件技能处理。

## 推荐工作流

### 完整尽职调查流程

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

### 事件驱动流程

```bash
/us-stock-analysis:earnings-call-analysis NVDA
/us-stock-analysis:insider-trading NVDA
/us-stock-analysis:options-analysis NVDA --earnings
```

## 分析标准

最新技能集采用了更严格的数据源规则：

- 优先使用 SEC 申报、公司 IR 材料、年报和可归因的一手来源
- 第三方财经网站仅作为交叉验证，不作为硬依赖
- 若关键数据缺失，应扩大估值区间并明确披露限制
- 当 peer 或一致预期数据不足时，不要制造虚假的精确度

每次分析都应以标准化的 `INVESTMENT SIGNAL` 区块结尾：

```text
Signal:      BULLISH / NEUTRAL / BEARISH
Confidence:  HIGH / MEDIUM / LOW
Horizon:     SHORT / MEDIUM / LONG-TERM
Score:       0.0 - 10.0
Action:      BUY / HOLD / SELL
Conviction:  STRONG / MODERATE / WEAK
```

## 仓库结构

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
├── README_CN.md
└── README-zh-TW.md
```

## 免责声明

本项目仅用于教育与研究流程支持，不构成投资建议。在进行资本配置决策前，请自行验证所有输入、假设与结论。
