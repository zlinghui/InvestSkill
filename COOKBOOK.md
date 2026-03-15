# InvestSkill Cookbook

Practical setup notes, workflow patterns, and examples for the current `v1.4.0` skill set.

> Related docs: [README](README.md) | [Traditional Chinese](COOKBOOK-zh-TW.md) | [Gemini CLI](GEMINI.md)

## 1. Setup

### Install from marketplace

```bash
claude

/plugin marketplace add yennanliu/InvestSkill
/plugin install us-stock-analysis@invest-skill
/plugin list
```

Expected result: `us-stock-analysis` appears with `18` available skills.

### Local install

```bash
claude

/plugin marketplace add /path/to/InvestSkill
/plugin install us-stock-analysis@invest-skill
```

### Quick verification

```bash
/us-stock-analysis:stock-eval AAPL
```

If the output ends with an `INVESTMENT SIGNAL` block, the install is working.

## 2. Mental Model

InvestSkill is best used as a research system, not as isolated commands.

### Core layers

| Layer | Skills | Purpose |
| --- | --- | --- |
| Business quality | `fundamental-analysis`, `competitor-analysis`, `financial-report-analyst` | Understand durability, accounting quality, and business health |
| Valuation | `stock-valuation`, `dcf-valuation`, `stock-eval` | Estimate intrinsic value and compare with market pricing |
| Market positioning | `insider-trading`, `institutional-ownership`, `earnings-call-analysis`, `short-interest`, `options-analysis` | See what management, funds, and the market are signaling |
| Timing and context | `technical-analysis`, `economics-analysis`, `sector-analysis` | Decide when and where to act |
| Final synthesis | `research-bundle`, `report-generator` | Turn multiple outputs into one thesis or exportable report |

## 3. Command Patterns

### Single-skill command

```bash
/us-stock-analysis:fundamental-analysis MSFT
```

### Skill with flags

```bash
/us-stock-analysis:technical-analysis NVDA --chart
/us-stock-analysis:fundamental-analysis AMZN --visual
/us-stock-analysis:stock-valuation GOOGL --full
```

### Multi-step workflow

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:stock-valuation AAPL --full
/us-stock-analysis:research-bundle AAPL --visual
```

## 4. Workflow Recipes

### Recipe A: Full diligence before opening a position

Use when you want a complete long-form thesis.

```bash
/us-stock-analysis:financial-report-analyst MSFT 10-K --full
/us-stock-analysis:competitor-analysis MSFT
/us-stock-analysis:stock-valuation MSFT --full
/us-stock-analysis:research-bundle MSFT --visual
/us-stock-analysis:report-generator --type comprehensive
```

What each step adds:

- `financial-report-analyst`: filing quality, accounting red flags, and risk factor changes
- `competitor-analysis`: moat durability and industry structure
- `stock-valuation`: price target range from multiple methods
- `research-bundle`: weighted synthesis into one thesis
- `report-generator`: exportable HTML/PDF output

### Recipe B: Fast idea triage

Use when you need a quick pass on a ticker.

```bash
/us-stock-analysis:stock-eval META
/us-stock-analysis:technical-analysis META
/us-stock-analysis:insider-trading META
```

Interpretation:

- `stock-eval` answers "is this business and valuation attractive?"
- `technical-analysis` answers "is the setup supportive right now?"
- `insider-trading` answers "are insiders confirming or contradicting the thesis?"

### Recipe C: Income stock review

```bash
/us-stock-analysis:dividend-analysis JNJ
/us-stock-analysis:fundamental-analysis JNJ
/us-stock-analysis:stock-valuation JNJ
```

Best for:

- dividend sustainability
- payout coverage and recession durability
- checking whether a high yield is actually a value trap

### Recipe D: Earnings event prep

```bash
/us-stock-analysis:earnings-call-analysis NVDA
/us-stock-analysis:options-analysis NVDA --earnings
/us-stock-analysis:technical-analysis NVDA --chart
```

Best for:

- guidance tone and management credibility
- expected move and IV regime
- key price levels into the event

### Recipe E: Top-down market workflow

```bash
/us-stock-analysis:economics-analysis
/us-stock-analysis:sector-analysis
/us-stock-analysis:stock-eval XOM
```

Best for:

- starting from macro regime
- narrowing into sectors
- then selecting a stock with context already established

## 5. Newest Skills in Practice

### `financial-report-analyst`

Best when source documents matter more than screeners.

Typical uses:

```bash
/us-stock-analysis:financial-report-analyst AAPL 10-K
/us-stock-analysis:financial-report-analyst MSFT 10-Q --section risk-factors
/us-stock-analysis:financial-report-analyst NVDA --compare-prior
```

Look for:

- revenue and margin explanations that do not reconcile cleanly
- DSO, inventory, and deferred revenue shifts
- new or intensified risk-factor language
- rising reliance on non-GAAP adjustments or SBC

### `stock-valuation`

Best when a single-method DCF is too narrow.

Typical uses:

```bash
/us-stock-analysis:stock-valuation AAPL
/us-stock-analysis:stock-valuation MSFT --methods dcf,cca,ev-ebitda
/us-stock-analysis:stock-valuation BRK.B --full
```

Use it to:

- cross-check DCF against market multiples
- expose assumption fragility
- produce a probability-weighted value range instead of one number

## 6. Data-Source Discipline

The latest skills are stricter about source quality.

Preferred order:

1. SEC filings, annual reports, 8-K earnings materials, and company IR pages
2. Management guidance and investor presentations
3. Clearly attributable market inputs
4. Third-party finance portals only as cross-checks

Do not stop the workflow just because a finance portal is unavailable. Use accessible primary materials and lower confidence if needed.

If peer multiples or consensus estimates are missing:

- widen the output range
- explain the missing data
- avoid fake precision

## 7. Cross-AI Usage

InvestSkill includes `17` universal prompt files in `prompts/`.

### Gemini example

```text
@prompts/stock-valuation.md Analyze AAPL using all valuation methods
@prompts/research-bundle.md Build a complete thesis for AAPL
```

### Copilot or Cursor example

```text
Use prompts/financial-report-analyst.md to review this 10-Q excerpt.
Use prompts/technical-analysis.md to analyze TSLA.
```

`report-generator` is the one major capability that stays on the Claude plugin side.

## 8. Output Standard

Every skill should end with the shared signal block:

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

Use this block as a compression layer after the detailed analysis, not as a substitute for the underlying reasoning.

## 9. Common Mistakes

- Running only `technical-analysis` and skipping business quality work for long-term positions
- Treating `stock-valuation` output as precise when the inputs are weak
- Using third-party portals as the only source for filings or capital structure
- Skipping `financial-report-analyst` when the thesis depends on accounting quality
- Exporting reports before resolving contradictions between valuation, technicals, and positioning data

## 10. Suggested Starting Points

Choose one based on the question you are trying to answer:

- "Is this business worth owning?" -> `stock-eval`
- "What is it worth?" -> `stock-valuation`
- "What changed in the filing?" -> `financial-report-analyst`
- "Is this a good entry point?" -> `technical-analysis`
- "What is the final thesis?" -> `research-bundle`
- "I need a deliverable." -> `report-generator`
