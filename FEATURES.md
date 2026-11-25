# Features

What LLMFA gives an investor, and how saving everything to Google Drive makes it more useful.

## What the analysis covers

The notebook does not stop at extracting numbers. Three question and chart files define what an investor gets for each company: where it has been (past), where it stands (present) and what could move it next (outlook).

| File | What it defines | Time focus | Data it uses |
|------|-----------------|------------|--------------|
| `questions_and_chart_details/22_questions_LLM.json` | 22 financial questions, answered per company | Past and present | Annual-report values, plus dated market data |
| `questions_and_chart_details/Graph_Charts_details.json` | 7 chart modules (plus one compact table) | Past, latest year, scenarios | Annual-report values, plus dated market data |
| `questions_and_chart_details/LLM_api_calling_questions.json` | 6 live research questions, for one company | Present and outlook | Live web search (Tavily) written up by Gemini |

## 1. The 22 questions: how the business has performed

**Objective:** answer the questions an investor would ask while reading an annual report, in a fixed order, using the verified numbers from the report.

| Area | Questions | What an investor learns |
|------|-----------|-------------------------|
| Revenue and profit growth | Q1–Q4 | Is revenue growing steadily, is profit keeping pace, is it stable, is it concentrated in one segment, region or customer |
| Profitability and capital efficiency | Q5–Q7 | Margins over five years, ROE and ROCE and what drives them, whether profit comes from operations or from other income and one-offs |
| Cash flow and earnings quality | Q8–Q9 | Whether profit turns into cash, and whether free cash flow remains after capex |
| Debt and financial safety | Q10–Q11 | How debt has changed, and whether earnings cover interest |
| Working capital and liquidity | Q12–Q13 | Whether short-term obligations are covered, and whether inventory and receivables are managed well |
| Latest year vs previous year | Q14 | What improved or got worse the most |
| Valuation and outlook | Q15–Q19 | P/E, P/B and EV/EBITDA, comparison with its own history and peers, the growth already priced in (reverse DCF), bear/base/bull scenarios, and a stress test |
| Shareholder returns and capital allocation | Q20–Q22 | How capital was used, whether shareholders gained through EPS and dividends, and governance red flags (promoter holding, pledges, related-party transactions, auditor remarks) |

Each answer carries a source label: `reported_historical`, `market_data`, `company_guidance`, `analyst_estimate` or `llm_scenario`. After Q1–Q22 the notebook produces a scorecard of strengths, weaknesses and red flags, each linked to its question number. It does not predict the share price.

The question file also defines a variant for banks and NBFCs (net interest margin, gross and net NPA, capital adequacy, CASA, liquidity coverage) that replaces the industrial-company ratios and drops the questions that don't apply to them.

## 2. The 7 charts: how the numbers look over time

**Objective:** show each question visually, with one chart module per question an investor cares about.

| # | Chart | The question it answers |
|---|-------|-------------------------|
| 1 | Revenue and profit growth | Is growth consistent, and are profits growing as fast as revenue? |
| 2 | Profitability and capital efficiency | Are margins and returns on capital improving? |
| 3 | Cash flow and earnings quality | Does profit turn into cash, and is free cash flow positive? |
| 4 | Debt and financial safety | Is the balance sheet getting safer or more fragile? |
| 5 | Working capital and liquidity | Is cash getting tied up in receivables or inventory? |
| 6 | Latest FY vs previous FY | What changed the most in the last year? |
| 7 | Valuation and future outlook | How is it valued against peers and its own history, and what do bear, base and bull cases look like? |

A compact table covers shareholder returns (EPS growth, dividends, buybacks, dilution). The chart design rules keep the charts honest:
- every figure is traceable to a source document and page;
- consolidated and standalone data are never mixed;
- a missing value stays missing, never zero;
- actuals, company guidance, analyst estimates and model scenarios are shown separately;
- market data always shows its as-of date;
- at most three observations per chart, each stating what changed, why it matters, what to investigate and its limits.

## 3. The 6 live research questions: what is happening now

**Objective:** add what an annual report cannot tell you: current news, policy and sector developments. This stage runs for one selected company and uses live web search.

| # | Topic |
|---|-------|
| 1 | Government investment and policy support for the sector (budget, schemes, incentives, and how much is announced vs approved vs actually spent) |
| 2 | Global trends and how the world sees the sector |
| 3 | Global and Indian industry outlook over the next 3–5 years |
| 4 | The company's position and its ability to benefit from those trends |
| 5 | Future outlook, valuation, catalysts and risks over 1–3 years, with optimistic, base and pessimistic cases |
| 6 | Upcoming products, launches, capacity expansions, order wins and approvals, separated into announced, under execution, proposed, rumour and inference |

The research rules require cited sources with links and dates, a clear split between verified facts, forecasts and inference, a search for negative evidence, no invented figures, and no buy or sell recommendation.

## Saved to your Google Drive

The notebook runs in Google Colab, but your work does not stay inside the Colab session. Your FY reports, the live web data and every result are kept in your own Drive folder (`BASE_DIR`), and the results are also downloaded to your computer automatically. They are small and take very little storage.

For each run it saves the full HTML report, the tables behind it (question answers, scenario table, verified facts, commentary) and the frozen rule that produced it. The full folder layout is in [DYNAMIC_DATASET.md](DYNAMIC_DATASET.md).

What this gives you:
- **Nothing is lost when Colab disconnects.** The session resets, but your results stay in Drive.
- **Your own dataset.** You upload the FY reports of the companies you care about, and the dataset keeps growing as you add more, built for your investments rather than a standard company list. No large PDF library on your computer.
- **A personal research library.** Analyse 4 companies today and come back to them next month; each run is saved with its date.
- **Faster, lighter repeat runs.** PDFs and web data are reused instead of downloaded again.
- **Private by default.** Reports and data sit in your own Drive. Your API keys stay in Colab Secrets and are not written to the saved files.

## Why this design

- **One place for past, present and outlook.** The report gives the history, live market data gives the present, and web research gives what is coming next.
- **The numbers underneath are checked.** The financial variables are cross-checked across the PDF, the Llama extractor and web sources under a frozen rule. Extraction accuracy is reported in [ACCURACY.md](ACCURACY.md).
- **Facts, estimates and scenarios are labelled.** An investor can see what is reported, what is a forecast and what is the model's own scenario.
- **Same questions for every company.** The comparison is consistent and nothing important is skipped.
- **Warnings instead of silent guesses.** Missing data, restatements, accounting-definition differences and unsuitable ratios are flagged.

## What it does not do

- It is not financial advice and never gives a buy or sell call. Valuations depend on assumptions you can change (for example cost of equity and terminal growth in Cell 1).
- The accuracy figures cover only the extraction of five statement variables. They do not measure the 22 answers, the charts or the research answers.
- The research stage depends on live web results and a language model. Check the cited sources before relying on a claim.
- Peer comparisons and scenarios are only as good as the data and assumptions behind them.
