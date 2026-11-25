# How LLMFA works: from reports to an investment decision

LLMFA is used in two rounds. The first round compares several companies so you can find the one you like most. The second round goes deep on that single company.

```
Run starts: allow Google Drive access when Colab asks (add your keys for Round 2)
        │
        ▼
Your FY reports in Google Drive
        │
        ▼
1. Scan: the notebook reads the reports in your Drive folder
        │
        ▼
2. Selector: you pick the companies to analyse
        │
        ▼
3. Round 1: 22 questions + 7 comparable charts + live stock charts, for every selected company
        │
        ▼
4. Report downloaded: one report comparing all the companies
        │
        ▼
5. You read the report and decide which company looks best
        │
        ▼
6. Final selector: you pick that one company
        │
        ▼
7. Round 2: 6 live research questions (news, policy, sector outlook, launches)
        │
        ▼
8. Final report downloaded: the full picture of your chosen company
```

## When the run starts

- **Allow Google Drive access.** After the code starts running, Colab asks to connect to your Google Drive. Click Connect, choose your account and click Allow. Without this the notebook cannot read your reports or save results.
- **Add your keys (for Round 2), ideally before you run.** The live research questions need `GEMINI_API_KEY` and `TAVILY_API_KEY`, stored in Colab Secrets. Round 1 works without them. Details are in [SECRETS.md](SECRETS.md).

## Step by step

**1. Scan the reports in Drive.** You keep the FY report PDFs of the companies you are interested in in your Drive folder (see [DYNAMIC_DATASET.md](DYNAMIC_DATASET.md)). The notebook scans the folder and finds which companies and years are available.

**2. Pick the companies.** The selector lists what it found. You choose the companies to analyse, for example four companies in the same sector.

**3. The 22 questions, 7 charts and live stock charts run.** For each selected company the notebook extracts and cross-checks the financial numbers, answers the 22 questions and builds the 7 comparable chart modules. These cover growth, profitability, cash flow, debt, liquidity, the latest year against the previous year, and valuation. It also draws a live stock chart for each company, where you can pick a time window and see the share price and its change over that window. See [FEATURES.md](FEATURES.md) for what each covers.

**4. The report is saved and downloaded.** The report includes the answers, the charts and the share-price charts. The results are written to a single report in your Drive and downloaded to your computer. It is a dated record you can reopen later.

**5. You read the report and shortlist.** The answers and charts put the companies side by side. Reading them, you might notice that company X has strong returns on capital and steady growth, while company Y carries more debt. This is the point where you decide which company deserves a closer look. LLMFA gives you the evidence; the decision is yours.

**6. Choose one company in the final selector.** You pick the single company you liked most.

**7. The 6 live research questions run.** For that company the notebook searches the live web and answers six questions about government policy, global trends, the industry outlook, the company's position, its future outlook and risks, and its upcoming products and launches. Every answer carries sources and dates.

**8. The final report is saved and downloaded.** You get the final report for your chosen company: its past and present from the annual reports, and what is happening now from live news and announcements. This is what you use to decide whether to invest.

## Why two rounds?

- **Round 1 is cheap and broad.** It uses your reports and market data, so you can screen several companies on equal terms.
- **Round 2 is focused.** The live research uses web search and a language model, so it runs only for the company you have already shortlisted.
- **You stay in control.** You choose the companies at both selectors, and nothing is bought or sold for you.

LLMFA is not financial advice. It gives no buy or sell recommendation, and the research stage depends on live web results, so check the cited sources before relying on a claim.
