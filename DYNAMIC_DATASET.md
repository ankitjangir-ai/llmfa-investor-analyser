# Dynamic dataset: your Google Drive is the dataset

LLMFA does not ship with a fixed set of companies. You upload the FY reports of the companies you want to analyse to your own Google Drive, and the dataset grows with every company you add. It becomes a dataset built for you and your investments, not a standard list chosen by someone else.

## Why not a standard dataset?

| A fixed, ready-made dataset | LLMFA's dynamic dataset |
|-----------------------------|-------------------------|
| A fixed list of companies chosen by someone else | Covers any listed company whose FY reports you add |
| Goes stale when a new annual report or a new share price arrives | Reports and live market data are brought in at run time |
| Hundreds of PDFs would have to be downloaded to your computer | Reports stay in your Drive; your computer stores nothing extra |
| One layout for everyone, so numbers cannot be traced to your own documents | Every number traces back to the report you supplied |
| Cannot show how today's analysis was produced | Each run saves its tables, answers and the frozen rule that produced them |

In short: you analyse the companies you care about, on today's data, without keeping a library of large PDFs on your laptop.

## A dataset built by you, for your investments

A standard company dataset has the same fixed list for everyone, and most of it will never be useful to you. In LLMFA the investor decides what goes in:

1. You upload the FY reports of a company you are considering.
2. The notebook analyses it and saves the results next to the reports.
3. The next time you analyse another company, you add its reports too, and the dataset is larger. Nothing you added earlier needs to be redone.

Over time the Drive folder becomes a personal research library of exactly the companies on your watchlist or in your portfolio, with their reports, verified numbers and past analyses in one place.

- **Relevant:** every company in it is one you chose.
- **Growing:** it expands as your interests do, and you never wait for a dataset provider to add a company.
- **Specific to your decisions:** the same questions, charts and checks are applied to each company, so your companies can be compared on equal terms.
- **Owned by you:** it stays in your Drive and can be kept, shared or deleted by you.

## What the dataset contains

Everything sits under one folder in your Drive (`BASE_DIR`, for example `Model_1`):

```
Model_1/
├── FY_Reports/                  your annual-report PDFs, kept for reuse
├── _cache/                      live web data, reused for 7 days (WEB_CACHE_DAYS)
├── Final_Output/
│   └── <companies analysed>/    one folder per run, named after the companies
│       ├── Report_<companies>.html   the full report (charts and answers)
│       ├── frozen_v4_rule.json       the exact rule used for this run
│       ├── tables/                   the data behind the report
│       │   ├── question_answers.json / .md / question_answers_llm.json
│       │   ├── scenario_table.csv    bear / base / bull and stress cases
│       │   ├── facts.json            the verified figures
│       │   └── narratives.json       written commentary
│       ├── charts/                   reserved for chart files (see note)
│       └── answers/                  reserved for answer files (see note)
└── Analysis/
    ├── Analysis_<date>.html     dated report, one per run
    └── fetched_values_<date>.txt   values fetched for manual checking
```

Note: the charts and answers are already inside the HTML report, so the `charts/` and `answers/` folders are currently empty. They are kept for later use.

## How it helps you

- **Choose your own companies.** Add the FY reports of any company and analyse it.
- **No space used on your computer.** The report PDFs and every output stay in Drive; results are downloaded only if you want a copy.
- **Reuse instead of repeat.** A report you added once is available for every later analysis, and web data is reused for 7 days.
- **A history you can reopen.** Each run is saved with the date, so you can look at an old analysis next month or compare it with a new one.
- **Traceable and repeatable.** The saved `frozen_v4_rule.json` and `facts.json` show how each number was decided.
- **Private.** The dataset lives in your own Drive. API keys stay in Colab Secrets and are not written into these files.

## What the dataset is not

- It is not a shared or public dataset, so you build your own.
- It only holds what you have analysed. A company you haven't run has nothing in it.
- Live web data is as current as the last fetch, within the 7-day cache window.
