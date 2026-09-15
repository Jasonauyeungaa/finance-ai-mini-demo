# T1 Project Plan

## Project Goal

Develop a clear and reproducible research workflow for comparing three familiar asset classes:

- `SPY` — US equities
- `TLT` — long-term US Treasury bonds
- `GLD` — gold

The workflow will define a bounded analysis task and an organized, verifiable agent process, starting with this written plan.

## Available Data

The repository contains a fixed, illustrative ETF snapshot dataset in `data/etf_snapshot.csv`. It holds one row per ETF with the following fields (see `data/data_dictionary.md`):

| Column | Meaning |
|---|---|
| `ticker` | Short identifier for the illustrative ETF |
| `asset_class` | Broad type of asset represented by the ETF |
| `expected_return_pct` | Illustrative annual return assumption (percent per year) |
| `volatility_pct` | Illustrative annual variability assumption (percent per year) |
| `max_drawdown_pct` | Illustrative largest peak-to-trough loss (percent) |
| `expense_ratio_pct` | Illustrative annual fund fee (percent per year) |

The three rows are `SPY` (8.2% return, 18% volatility, -24% max drawdown, 0.09% fee), `TLT` (4%, 14%, -18%, 0.15%), and `GLD` (5.5%, 15.5%, -16%, 0.4%).

**This dataset is synthetic teaching data**: all numeric values are illustrative teaching assumptions, not live or historical market observations, and must not be used as investment advice or for a real investment decision.

## Expected Final Deliverable

The final deliverable will be a concise, reproducible comparison of the three ETFs based on the snapshot data, covering risk, return, and cost differences across the asset classes, together with the scripts and documentation needed to reproduce the analysis.

## Three Project Milestones

1. **Project setup (T1)**: Create this initial project plan describing the goal, data, and next steps.
2. **Bounded analysis task (T2)**: Define a specific, answerable analysis question (e.g., risk-return and fee comparison) and implement a script that computes the relevant summary metrics from the snapshot data.
3. **Verifiable agent workflow (T3)**: Organize the analysis into a repeatable agent workflow and verify that results can be reproduced from the repository inputs.

## One Data Limitation

The dataset is small and synthetic: all numeric values are teaching assumptions rather than real market data, and the snapshot omits correlations, taxes, transaction costs, liquidity, currency exposure, and investor-specific constraints. Conclusions drawn from it will be illustrative only.

## Next Action

Proceed to design the bounded analysis task (Milestone 2): choose the specific comparison question, then implement and run the analysis script. This plan describes the planned analysis; no analysis has been completed yet.
