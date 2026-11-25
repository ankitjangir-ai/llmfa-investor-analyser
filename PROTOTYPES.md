# LLMFA prototype history

The 10 prototypes built before the locked release (v4.0.0), in build order. All were scored against the same annual-report ground truth: 13 companies, 43 reports (FY2023–FY2026), 5 variables (total assets, total equity, current liabilities, inventories, EBIT).

Exact = identical value. Exact + tolerance = exact, plus gap ≤ 0.1% or ≤ the rounding floor (0.5 × precision of source A + 0.5 × precision of source B). Percentages are of the scored cells in that row's scope.

| # | Prototype | Method | Scope (scored cells) | Exact | Exact + tolerance | Outcome |
|---|-----------|--------|----------------------|-------|-------------------|---------|
| 1 | yfinance v/s Ground Truth | yfinance alone vs ground truth | 171 (11 companies; Infosys and HDFC excluded) | 134 (78.4%) | 154 (90.1%) | Kept as the primary web source. Balance-sheet fields only (136 cells): 115 exact (84.6%), 132 with tolerance (97.1%). EBIT weak: 19/35 exact, 22/35 with tolerance. |
| 2 | Screener, BSE and Moneycontrol v/s Ground Truth | Three more websites tested alone | Screener 199; BSE 0/199 and Moneycontrol 0/199 (every download failed) | Screener 37 (18.6%) | Screener 102 (51.3%) | All three dropped. Screener: equity excludes non-controlling interest, no current liabilities. BSE and Moneycontrol never returned data, so their 0% says nothing about data quality. |
| 3 | yfinance, Screener and NSE v/s Ground Truth | Side-by-side comparison of the three remaining sources | yfinance 171, Screener 148, NSE 56 (not like-for-like) | yfinance 134 (78.4%); Screener 37 (25.0%); NSE 40 (71.4%) | yfinance 154 (90.1%); Screener 98 (66.2%); NSE 47 (83.9%) | yfinance primary, NSE verifier. NSE had FY2023–24 only and no EBIT. |
| 4 | Override method | Rule-based pick of one value per cell from yfinance, Screener and NSE (rules frozen before ground truth was read) | 199 | 137 (68.8%) | 170 (85.4%) | 4 cells not covered. Without HDFC (187): 137 exact (73.3%), 170 with tolerance (90.9%). |
| 5 | PDF v/s Ground Truth | PDF text route alone (v1.3): finds balance sheet and P&L pages, parses rows | 199 | 159 (79.9%) | 160 (80.4%) | Right when it returns a value, but 29 cells not covered (mostly no row found). Without HDFC (187): 155 exact (82.9%), 156 with tolerance (83.4%). |
| 6 | Hybrid A | PDF value first, override as fallback | 199 | 178 (89.4%) | 180 (90.5%) | Full coverage. Without HDFC (187): 174 exact (93.0%), 176 with tolerance (94.1%). Errors: HDFC fallbacks, MSIL inventories parsed wrongly. |
| 7 | Hybrid B | Override value first, PDF only as fallback | 199 | 141 (70.9%) | 174 (87.4%) | Full coverage, but it discards correct PDF values (total assets fell to 69.8% exact). Without HDFC (187): 140 exact (74.9%), 173 with tolerance (92.5%). |
| 8 | Hybrid A Voting — 4 Source | PDF + yfinance + Screener + NSE grouped into agreement clusters, with tie-breaking | 199 | 159 (79.9%) | 187 (94.0%) | Full coverage, highest tolerance accuracy of the web/PDF hybrids, but exact stays at the PDF-only level. Without HDFC (187): 155 exact (82.9%), 183 with tolerance (97.9%). |
| 9 | Hybrid B Voting — 4 Source | Same four sources, voting weighted toward the websites | 199 | 150 (75.4%) | 184 (92.5%) | Full coverage. Without HDFC (187): 146 exact (78.1%), 180 with tolerance (96.3%). |
| 10 | **Final v4 (locked)** | PDF route + Llama 3.1 8B extractor + web override under the frozen integration rule | 199 | **192 (96.5%)** | **192 (96.5%)** | **Locked as v4.0.0.** 7 mismatches, all HDFC (4 EBIT, 3 total equity); without HDFC 187/187. |

## Reading the table

- Exact accuracy rose from 68.8% (override alone) and 79.9% (PDF alone) to 96.5% in the final version. The voting prototypes reached 92–94% with tolerance but stayed at 75–80% exact; adding the Llama extractor is what lifted exact matches.
- Rows 1–3 test sources on their own and are not comparable with rows 4–10, which are full pipelines scored on all 199 cells. Rows 1 and 3 use 171 cells because Infosys and HDFC were excluded.
- Row 2 is not a real accuracy result for BSE and Moneycontrol: every request failed.

## Caveats

- All ten prototypes are in-sample. Rules were designed and revised while looking at failures on the same 43 reports, and the final version was chosen as the highest-accuracy one. The 96.5% is therefore not a held-out result; the unseen test (4 defence and aerospace companies, 20 reports) is reported in `ACCURACY.md`.
- Early rows used a stricter tolerance variant (0.1% with a 1 crore cap, 128 of 136 cells in row 1) before the rounding-floor rule was adopted. The table uses the final rule throughout.
- Ground-truth basis for HDFC and Infosys was an open question during the prototypes (Infosys was corrected from standalone to consolidated before row 4).
- The "near" class (gap ≤ 1%) existed in the prototype reports and is not shown here.

## Notebooks

The locked prototype (row 10) is included as `prototype/LLMFA_prototype_v4_locked.ipynb`. Its last cells write `accuracy_check_199.csv` and `accuracy_summary_199.csv`, the row-level check for the 199 values (both files are in `checks/`). The other nine prototypes are not included; their numbers come from the results reports written at the time of each run. The release notebook, `notebook/LLMFA_v4_final.ipynb`, is the cleaned implementation of row 10.
