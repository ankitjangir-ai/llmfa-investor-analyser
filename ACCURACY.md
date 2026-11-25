# Accuracy

Tolerance: a value counts as matched if the gap to the annual report is within 0.1% of the value, or within a rounding floor (half the precision of the model value plus half the precision of the reported value).

## Table 1: Seen data (prototype)

**FY reports used: 43**

| Result | Values | Share |
|---|---|---|
| Total values checked | 199 | 100% |
| Exact match | 192 | 96.48% |
| Within tolerance | 0 | 0.00% |
| **Total matched (exact + tolerance)** | **192** | **96.48%** |
| Mismatched | 7 | 3.52% |
| Matched, excluding banking (HDFC Bank) | 187 of 187 | 100% |

Companies: 13 · Reports: 43 (FY2023–FY2026) · Numerical variables: total assets, total equity, current liabilities, inventories, EBIT

Mismatches: all 7 are HDFC Bank (4 EBIT, 3 total equity).

Sectors used: 9

Sector details:
- Banking: HDFC Bank
- Telecom: Bharti Airtel
- IT services: Infosys, TCS
- Automobiles: Maruti Suzuki
- Pharmaceuticals: Sun Pharma
- Conglomerate: Reliance Industries
- Metals and cement: Tata Steel, UltraTech Cement
- Consumer: Titan
- Internet platforms and fintech: Paytm, Swiggy, Zomato (Eternal)

Different sectors and several years (FY2023–FY2026) were used so the model is tested on varied report layouts, including banking, and not on a single industry or year.

---

## Table 2: Unseen data (current model, v4)

**FY reports used: 20** (4 companies × FY2022–FY2026)

| Result | Values | Share |
|---|---|---|
| Total values checked | 230 | 100% |
| Exact match | 214 | 93.04% |
| Within tolerance | 9 | 3.91% |
| **Total matched (exact + tolerance)** | **223** | **96.96%** |
| Mismatched | 7 | 3.04% |

Companies: 4 · Reports: FY2022–FY2026 · Values checked: 230 of 780 model output values

Mismatches: 7 (3 EPS, 4 BEL repayment of borrowings).

The model was run once and not changed or re-run. Its output values were then checked against the annual reports. The 230 values checked are the flagged values plus a sample (about a third of each company's values), not every value.

Sectors used: 1

Sector details:
- Defence and aerospace: Apollo Micro Systems, BEL, Data Patterns, HAL

This is a completely different sector, which the prototype was not tested on.

---

## Table 3: Combined (seen + unseen)

**FY reports used: 63** (43 + 20)

| Result | Values | Share |
|---|---|---|
| Total values checked | 429 | 100% |
| Exact match | 406 | 94.64% |
| Within tolerance | 9 | 2.10% |
| **Total matched (exact + tolerance)** | **415** | **96.74%** |
| Mismatched | 14 | 3.26% |

Companies: 17 · Reports: 63 · Values checked: 429

Sectors used: 10

Sector details:
- Banking: HDFC Bank
- Telecom: Bharti Airtel
- IT services: Infosys, TCS
- Automobiles: Maruti Suzuki
- Pharmaceuticals: Sun Pharma
- Conglomerate: Reliance Industries
- Metals and cement: Tata Steel, UltraTech Cement
- Consumer: Titan
- Internet platforms and fintech: Paytm, Swiggy, Zomato (Eternal)
- Defence and aerospace: Apollo Micro Systems, BEL, Data Patterns, HAL
