# llmfa-investor-analyser

### Investor Analysis of Indian Companies Using FY Reports and Live Market Data

You upload annual-report (FY) PDFs and pick the companies. The notebook reads the reports, checks the values against live web sources (share price, Screener.in, Yahoo Finance, NSE), and builds one table of financial variables per company, plus derived metrics, valuation, a reverse DCF and stress test, 22 answered questions, 7 comparable charts, share-price charts, 6 optional live research questions and an HTML report.

> **Not financial advice.** The output is informational. Valuations depend on assumptions you can change in Cell 1 (for example cost of equity 12% and terminal growth 5%), and web data can change between runs.

Release: **v4.0.0** · Notebook: `notebook/LLMFA_v4_final.ipynb`

## What you get

The full workflow, from your reports to a final report on the one company you pick, is in [HOW_IT_WORKS.md](HOW_IT_WORKS.md).

For each company the notebook answers 22 financial questions, draws 7 chart modules, and (optionally) runs 6 live research questions on the sector and the company's outlook. See [FEATURES.md](FEATURES.md) for what each covers and how it helps an investor.

## How it works

Each variable is filled by three routes, combined under one fixed rule:

| Route | What it is |
|-------|------------|
| **P** | Rule-based PDF parser on the statement pages |
| **L** | Llama 3.1 8B Instruct (4-bit NF4) row-picker. Llama copies a row number and a value; Python validates it and does all arithmetic |
| **O** | Web override from yfinance, Screener.in and NSE, used only after an identity check confirms the web series describes the same company as the report |

**Integration rule (frozen):**
1. P and L both exist and agree within 0.1% → use P.
2. P and L disagree → if O supports exactly one of them within 1%, use that one. Otherwise use L when P came from a flagged row; else keep P and add the cell to the REVIEW list.
3. Only one of P, L exists → accept it if O is missing or agrees within 1%. If O differs by 1% to 10%, take O. If O differs by more than 10%, keep the document value and add it to REVIEW.
4. Only O exists → use O. None → missing.

**Web source order (override route):** for every variable except EBIT the anchor is yfinance, with fallback yfinance → NSE → Screener. For EBIT the anchor is Screener, with fallback Screener → NSE → yfinance.

After the table: QA and restatement checks, derived metrics, valuation (P/E, P/B, EV/EBITDA, dividend yield, earnings yield), a reverse DCF and a stress test, fact-linked narratives, 22 questions answered by Llama from the report data, 7 comparable charts and a share-price chart per company. An optional research stage answers 6 live research questions for one selected company using Tavily web search and Gemini.

## Accuracy

Scored against values read from the annual reports. A value counts as matched if it is exact, or within 0.1% or a rounding floor of the reported value.

| | Values | Matched | Share |
|---|---|---|---|
| Seen data (prototype, 13 companies, 43 reports) | 199 | 192 | 96.48% (100% excluding HDFC Bank) |
| Unseen data (4 defence and aerospace companies, 20 reports) | 230 | 223 | 96.96% |
| Combined (17 companies, 63 reports) | 429 | 415 | 96.74% |

Full tables, sectors and companies: [ACCURACY.md](ACCURACY.md). The ten prototypes that led to this version: [PROTOTYPES.md](PROTOTYPES.md). Row-level check files are in [`checks/`](checks/); the prototype notebook that produced the 199-value result is in [`prototype/`](prototype/).

How to read these numbers:
- The percentages measure only how correctly the model extracts the five statement variables (total assets, total equity, current liabilities, inventories, EBIT). They do not measure the valuation, the narratives or the question answers.
- The seen-data figure is in-sample. The rules were developed on those 43 reports, and v4 was chosen as the best of ten prototypes.
- The unseen figure is the one to rely on for new companies. It covers 230 of the 780 values the model produced for those four companies: all flagged values plus about a third of the rest, not every value. It is a single sector.
- The remaining errors on the seen data are all HDFC Bank (a bank, with a different statement layout).

## Where your results are saved

Results are saved to your own Google Drive (under `BASE_DIR`) and also downloaded to your computer automatically (they take very little storage), so analysed companies stay available for later use. The folder layout and why Drive works as a dynamic dataset are in [DYNAMIC_DATASET.md](DYNAMIC_DATASET.md).

## Requirements

- A GPU runtime for the Llama cells (tested on a Tesla T4 in Google Colab, Python 3.13, CUDA 13.0 build of PyTorch).
- Python packages: see `requirements.txt` (the notebook also installs them in its setup cell).
- Optional keys: `GEMINI_API_KEY` and `TAVILY_API_KEY` (research stage only), `HF_TOKEN` (Hugging Face; optional for the open mirrors used). Keys are read from Colab Secrets, then an environment variable, then a hidden prompt. Never put a key in the notebook. See [SECRETS.md](SECRETS.md) for how to store them and how to allow Google Drive access.
- Annual-report PDFs, which are **not** included in this repository.

## Quick start

1. Open `notebook/LLMFA_v4_final.ipynb` in Google Colab (or locally with a GPU) and select a GPU runtime.
2. In the first cell, set `BASE_DIR` to a folder of your own. Put your reports in `BASE_DIR/FY_Reports/`, named `COMPANY_YEAR.pdf`, for example `HAL_2025.pdf`. YEAR is the financial-year end year (FY 2024-25 is 2025).
3. Copy `22_questions_LLM.json` (and `LLM_api_calling_questions.json` for the research stage) from the `questions_and_chart_details/` folder into `BASE_DIR`.
4. Run the setup cells, Cell 1 (settings) and Cell 2 (company picker). Pick the companies, then run Cell 2B and every cell after it up to Cell 24.
5. Cell 24 builds the full report (every question and answer for each selected company, all charts, share-price charts) and downloads it. Read it and decide which company you like most.
6. Research stage (optional, one company): run Cells 25–27. Cell 25 selects the company, Cell 26 runs the 6 live research questions, and Cell 27 builds that company's final report and downloads it.
7. Cell 28 saves a dated copy of the analysis in `BASE_DIR/Analysis/`.

To re-run, use Runtime → Restart session and run all.

The main output is the HTML report. The tables behind it (including the verified values) are saved in your Drive; see [DYNAMIC_DATASET.md](DYNAMIC_DATASET.md).

## Repository layout

```
notebook/                the model notebook (LLMFA_v4_final.ipynb)
questions_and_chart_details/ the question and chart files: 22 questions, 7 charts, 6 research questions
prototype/               the locked prototype notebook (research version, 199-value result)
README.md
ACCURACY.md              accuracy tables
PROTOTYPES.md            the ten prototypes and their results
HOW_IT_WORKS.md          the full flow, from reports to the final report
SECRETS.md               API keys, Colab Secrets and Google Drive access
FEATURES.md              what the 22 questions, 7 charts and 6 research questions cover
DYNAMIC_DATASET.md       how Google Drive acts as your own dataset
requirements.txt
LICENSE
checks/                  row-level check files for the accuracy tables
docs/                    screenshots and charts
examples/                an example report generated by the notebook
```

## Known limitations

- Prototype accuracy is in-sample; unseen accuracy covers four companies in one sector and a checked subset (230 of 780 values).
- HDFC Bank: EBIT is wrong in all four years, and PDF and Llama agree on the wrong value, so the pipeline does not flag it. Total equity is off by about 0.6–0.75%.
- The Llama row-picker takes a lease repayment line as repayment of borrowings (BEL, FY2023–FY2026).
- Under rule 3, a web value can override a correct PDF value (seen on Data Patterns depreciation and amortisation, FY2022 and FY2023).
- Web EPS can be on a restated share count and then differs from the reported EPS.
- Non-convertible debenture lines are excluded from Apollo Micro Systems proceeds from borrowings.
- Web data (Screener.in, Yahoo Finance, NSE, Tavily, Gemini) is fetched live, so results can change between runs. Responses are cached for a number of days (`WEB_CACHE_DAYS`).
- Scanned or image-only PDF pages are not supported (no OCR).
- Gemini model names in the research stage are settings; use names available on your account.

## License

See [LICENSE](LICENSE).
