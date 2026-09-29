# T2 ETF Comparison: Fees and Maximum Drawdown (SPY, TLT, GLD)

T1's project plan proposed comparing SPY, TLT and GLD from a small synthetic snapshot; this T2 report upgrades that comparison to the real daily price series of the T2 ETF Data Pack, keeping return analysis out of scope.

## Source and method

- **Input paths**: `data/t2/daily_prices.csv` (7,536 daily observations, 2,512 per ETF, common window **2016-09-01 through 2026-08-31**), `data/t2/fund_info.csv` (fee records); `artifacts/t1/project-plan.md` used only as context. All numerical claims use the T2 pack.
- **Preparation date**: 2026-09-25 (pack). Historical source recorded by the preparation team: Yahoo Finance chart API with daily data and adjusted closes enabled; retained responses downloaded 2026-09-20. Adjustment basis: provider's `adjusted_close` (split and dividend-distribution adjustments), in USD.
- **Fees** (disclosed annual expense ratios, source URLs in `fund_info.csv`, all rechecked/accessed on 2026-09-25):
  - SPY: **0.0945** — Fund-information as-of date 2026-09-10; fee-specific effective date **not stated** (label is gross of waivers/reimbursements).
  - TLT: **0.15** — Current prospectus; specific date **not stated** in the fee panel.
  - GLD: **0.4** — Effective date **not stated** in the selected field.
- **Script**: `artifacts/t2/calculate_drawdown.py`; command actually executed from the repository root: `python3 artifacts/t2/calculate_drawdown.py` (Python 3.9.6, standard library only).
- **Input-check result**: all checks passed before calculating — exactly the tickers SPY/TLT/GLD; 7,536 total rows; 2,512 rows per ETF; unique ticker/date pairs; dates ascending within each ticker; all `close` and `adjusted_close` values positive and finite; first date 2016-09-01 and last date 2026-08-31 for every ETF; date sets identical across tickers. Nothing was dropped or filled.
- **Calculation method**: for each ticker, use all `adjusted_close` observations in ascending date order; `high_t` = largest adjusted_close from the window start through day *t*; `drawdown_t% = (adjusted_close_t / high_t − 1) × 100`; maximum drawdown = minimum across the full window. Tie rule: earliest trough with its earliest corresponding peak; no-loss rule: first observation for both dates and zero drawdown. Full precision kept internally; only the displayed table values are rounded to two decimals.

## 1. Comparison

| Ticker | Annual expense ratio (%) | Maximum drawdown (%) | Peak date | Trough date |
|---|---|---|---|---|
| SPY | 0.0945 | -33.72 | 2020-02-19 | 2020-03-23 |
| TLT | 0.15 | -48.35 | 2020-08-04 | 2023-10-19 |
| GLD | 0.4 | -26.40 | 2026-01-29 | 2026-07-16 |

Fees preserve the disclosed precision of `fund_info.csv`. In every row the peak is on or before the trough. Full-precision drawdowns retained for the check: SPY −33.7172572108114%, TLT −48.3511260736481%, GLD −26.4045178570297%; the corresponding peak/trough adjusted_close values are recorded in the Agent Check section and in the script output.

## 2. Observation

GLD has the smallest drawdown loss in this period (closest to zero): **−26.40%**, versus SPY −33.72% and TLT −48.35%. The three displayed two-decimal results (−33.72, −48.35, −26.40) are all distinct, so there is no tie. Drawdowns are non-positive; closer to zero means a smaller loss. All drawdowns were calculated from the `adjusted_close` column of `daily_prices.csv` using the cumulative-high method above — they were not read from any precomputed snapshot, and the supplied inputs deliberately contain no such snapshot. One limitation: these are daily-close drawdowns within the fixed 2016-09-01–2026-08-31 window only; they are not intraday or all-time measures, and the disclosed fees are current issuer disclosure snapshots (as of the 2026-09-25 recheck), not ten-year averages, and they exclude trading costs such as bid/ask spreads.

## 3. Agent Check

This is my own self-check performed by the Agent with local tools; it is **not** independent verification, and the student has not checked the data.

**Chosen ETF: SPY.** Re-read the reported peak/trough rows from `data/t2/daily_prices.csv` (source rows and adjusted prices):
- Row 872: `2020-02-19,SPY,338.3399963378906,307.6394958496094` — peak, adjusted_close = 307.6394958496094
- Row 895: `2020-03-23,SPY,222.9499969482422,203.91189575195312` — trough, adjusted_close = 203.91189575195312

**Check command**: an inline `python3 -c` script (standard library) that independently re-reads the CSV, recomputes drawdown for all dates and all three tickers (brute-force cumulative high per date, earliest-trough/earliest-peak tie rule), asserts every `high_t` equals `max(adjusted_close[0..t])`, asserts peak date ≤ trough date, and recomputes the SPY pair arithmetically as `(trough / peak − 1) × 100`.

**Actual output**:
```
SPY (trough/peak-1)*100 = -33.7172572108114%
SPY: max_dd=-33.7172572108114 peak=2020-02-19 adj=307.639495849609 trough=2020-03-23 adj=203.911895751953  peak_before_trough=True
TLT: max_dd=-48.3511260736481 peak=2020-08-04 adj=141.584442138672 trough=2023-10-19 adj=73.1267700195312  peak_before_trough=True
GLD: max_dd=-26.4045178570297 peak=2026-01-29 adj=495.899993896484 trough=2026-07-16 adj=364.959991455078  peak_before_trough=True
```

**Comparison**: the two-point SPY recomputation (−33.7172572108114%) exactly matches the report's full-precision SPY drawdown (displayed −33.72%). The independent full-series recheck reproduces all three tickers' peak/trough dates, adjusted_close values, and drawdowns exactly, with peak before trough in every case; the observation (GLD closest to zero) holds against all three results. **No correction was needed and no rerun was required.**
