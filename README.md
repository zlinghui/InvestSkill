# InvestSkill

Professional US stock analysis skills for Claude Code, Gemini CLI, GitHub Copilot, Cursor, and other AI assistants.

> Current release: `v1.4.0`
>
> Docs: [Cookbook](COOKBOOK.md) | [Traditional Chinese](README-zh-TW.md) | [Website](https://yennanliu.github.io/InvestSkill/)

[![Deploy](https://github.com/yennanliu/InvestSkill/actions/workflows/deploy-pages.yml/badge.svg)](https://github.com/yennanliu/InvestSkill/actions/workflows/deploy-pages.yml)
[![Validate](https://github.com/yennanliu/InvestSkill/actions/workflows/validate.yml/badge.svg)](https://github.com/yennanliu/InvestSkill/actions/workflows/validate.yml)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## Overview

InvestSkill packages a complete US equity research workflow into reusable skills:

- `18` Claude Code skills in `plugins/us-stock-analysis/skills/`
- `17` cross-AI prompt files in `prompts/`
- A shared `INVESTMENT SIGNAL` block for standardized outputs
- Coverage across fundamentals, technicals, valuation, filings, market positioning, and portfolio review

The newest analysis capabilities in the current skill set are:

- `/financial-report-analyst` for 10-K, 10-Q, proxy, and filing-quality analysis
- `/stock-valuation` for multi-method valuation using DCF, comps, EV multiples, and residual income

`/report-generator` remains a Claude Code plugin skill and is not shipped as a standalone prompt file.

## Install

### Marketplace install

```bash
claude

/plugin marketplace add yennanliu/InvestSkill
/plugin install us-stock-analysis@invest-skill
/plugin list
```

### Local development install

```bash
claude

/plugin marketplace add /path/to/InvestSkill
/plugin install us-stock-analysis@invest-skill
```

After installation, you should see `us-stock-analysis` with `18` skills.

## Quick Start

```bash
# Holistic stock review
/us-stock-analysis:stock-eval AAPL

# Filing analysis
/us-stock-analysis:financial-report-analyst MSFT 10-K

# Multi-method valuation
/us-stock-analysis:stock-valuation NVDA --full

# Full research workflow
/us-stock-analysis:research-bundle AMZN
```

Some Claude Code setups also expose short aliases like `/stock-eval`; the namespaced form above is the safest documented form.

## Skill Map

### Core analysis

| Skill | What it covers |
| --- | --- |
| `/stock-eval` | Holistic stock evaluation with quality, valuation, moat, and risk framing |
| `/economics-analysis` | US macro indicators, rates, credit conditions, and market implications |
| `/fundamental-analysis` | Financial statement analysis, business quality, and optional visual output |
| `/technical-analysis` | Multi-timeframe technical setup, indicators, support/resistance, and chart mode |
| `/portfolio-review` | Allocation, diversification, concentration, and portfolio diagnostics |
| `/sector-analysis` | Sector rotation, cycle positioning, and relative opportunities |

### Market data and reporting

| Skill | What it covers |
| --- | --- |
| `/report-generator` | HTML/PDF investment reports with charts and signal blocks |
| `/earnings-call-analysis` | Transcript sentiment, guidance quality, and management tone |
| `/insider-trading` | SEC Form 4 activity, transaction patterns, and insider signaling |
| `/institutional-ownership` | 13F ownership changes and smart-money positioning |

### Advanced research

| Skill | What it covers |
| --- | --- |
| `/dividend-analysis` | Dividend safety, payout durability, and income investing |
| `/short-interest` | Short positioning, squeeze setup, and bearish pressure |
| `/options-analysis` | Greeks, IV regime, strategy selection, and earnings setup |
| `/research-bundle` | Multi-skill orchestration into one investment thesis |
| `/dcf-valuation` | Intrinsic value modeling with scenarios and sensitivity tables |
| `/competitor-analysis` | Moat strength, market structure, and peer benchmarking |
| `/financial-report-analyst` | 10-K/10-Q deep reads, accounting quality, and filing red flags |
| `/stock-valuation` | DCF + comps + EV multiples + residual income triangulation |

## Cross-AI Usage

InvestSkill ships `17` prompt files in [prompts/](prompts) for AI tools outside Claude Code.

### Gemini CLI

Gemini reads [GEMINI.md](GEMINI.md) automatically. Example:

```bash
gemini
```

```text
@prompts/stock-valuation.md Analyze AAPL using DCF, comps, and residual income
@prompts/financial-report-analyst.md Review this 10-Q excerpt for red flags
@prompts/research-bundle.md Build a full thesis for AMZN
```

### GitHub Copilot and Cursor

- GitHub Copilot uses `.github/copilot-instructions.md`
- Cursor uses `.cursor/rules/invest-skill.mdc`

You can also reference prompt files directly in chat:

```text
Use prompts/fundamental-analysis.md to analyze MSFT
Use prompts/technical-analysis.md to review TSLA
```

### Prompt coverage

The `prompts/` directory contains analysis prompts for:

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

There is intentionally no `report-generator.md`; report generation is handled by the Claude plugin skill.

## Recommended Workflows

### Full diligence workflow

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:stock-valuation AAPL --full
/us-stock-analysis:research-bundle AAPL --visual
/us-stock-analysis:report-generator --type comprehensive
```

### Fast fundamental workflow

```bash
/us-stock-analysis:fundamental-analysis MSFT --visual
/us-stock-analysis:dcf-valuation MSFT
/us-stock-analysis:competitor-analysis MSFT
```

### Event-driven workflow

```bash
/us-stock-analysis:earnings-call-analysis NVDA
/us-stock-analysis:insider-trading NVDA
/us-stock-analysis:options-analysis NVDA --earnings
```

## Analysis Standards

The latest skills follow a stricter data-source protocol:

- Prefer SEC filings, company IR material, annual reports, and attributable primary sources
- Use third-party finance portals only as cross-checks, not hard dependencies
- If key data is missing, widen the valuation range and state the limitation explicitly
- Avoid false precision when peer or consensus data is unavailable

Every analysis should end with the standardized `INVESTMENT SIGNAL` block:

```text
Signal:      BULLISH / NEUTRAL / BEARISH
Confidence:  HIGH / MEDIUM / LOW
Horizon:     SHORT / MEDIUM / LONG-TERM
Score:       0.0 - 10.0
Action:      BUY / HOLD / SELL
Conviction:  STRONG / MODERATE / WEAK
```

## Repository Layout

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

## Disclaimer

This project is for education and research workflow support only. It is not investment advice. Validate all inputs, assumptions, and conclusions before making capital allocation decisions.
