# InvestSkill for Gemini CLI

Use the prompt files in `prompts/` as the canonical Gemini entry points for InvestSkill.

## Scope

- Current plugin release: `v1.4.0`
- Claude Code skills: `18`
- Gemini prompt files: `17`
- `report-generator` is Claude-plugin-only and does not have a Gemini prompt

## Prompt Index

| Prompt file | Use for |
| --- | --- |
| `prompts/stock-eval.md` | Holistic stock evaluation |
| `prompts/financial-report-analyst.md` | 10-K, 10-Q, filing-quality, and red-flag review |
| `prompts/stock-valuation.md` | Multi-method valuation with DCF, comps, EV multiples, residual income |
| `prompts/fundamental-analysis.md` | Deep financial statement analysis |
| `prompts/technical-analysis.md` | Multi-timeframe technical review |
| `prompts/dcf-valuation.md` | Standalone intrinsic value modeling |
| `prompts/economics-analysis.md` | US macro and market context |
| `prompts/earnings-call-analysis.md` | Transcript and guidance analysis |
| `prompts/insider-trading.md` | Form 4 and insider activity review |
| `prompts/institutional-ownership.md` | 13F and smart-money positioning |
| `prompts/competitor-analysis.md` | Moat and peer positioning |
| `prompts/dividend-analysis.md` | Dividend safety and income analysis |
| `prompts/short-interest.md` | Short positioning and squeeze setup |
| `prompts/options-analysis.md` | Greeks, IV, and options strategy |
| `prompts/portfolio-review.md` | Portfolio-level review |
| `prompts/sector-analysis.md` | Sector rotation and relative strength |
| `prompts/research-bundle.md` | End-to-end investment thesis workflow |

## Quick Start

```bash
gemini
```

```text
@prompts/stock-valuation.md Analyze MSFT using DCF, comparable companies, and residual income.

@prompts/financial-report-analyst.md Review this 10-Q section, identify accounting red flags, and score filing quality.

@prompts/research-bundle.md Build a full investment thesis for AMZN and end with the standard signal block.
```

## Recommended Gemini Workflows

### Filing-led workflow

```text
@prompts/financial-report-analyst.md Analyze AAPL 10-K
@prompts/stock-valuation.md Value AAPL using the findings above
@prompts/research-bundle.md Synthesize the final thesis for AAPL
```

### Fast triage

```text
@prompts/stock-eval.md Evaluate NVDA
@prompts/technical-analysis.md Review the current technical setup for NVDA
```

### Income and risk workflow

```text
@prompts/dividend-analysis.md Review JNJ for dividend safety
@prompts/short-interest.md Assess bearish positioning in JNJ
```

## Analysis Rules

The latest valuation and research skills use a stricter source hierarchy. Follow it in Gemini:

1. Prefer SEC filings, annual reports, 8-K earnings materials, XBRL facts, and company IR pages.
2. Use management guidance and investor presentations for scenario assumptions.
3. Use attributable market inputs for price, treasury yields, and other reference data.
4. Treat third-party finance portals as cross-checks only.

If reliable peer multiples, beta, or consensus data are unavailable:

- state the limitation explicitly
- widen the valuation range
- avoid invented or padded inputs

## Output Standard

End every analysis with the shared InvestSkill signal format:

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

## Note on Reports

Gemini can generate analysis content and chart-ready tables, but InvestSkill's HTML/PDF export flow is implemented through the Claude Code `/report-generator` skill. If you need final report export, run the final synthesis in Claude Code after using Gemini for research.
