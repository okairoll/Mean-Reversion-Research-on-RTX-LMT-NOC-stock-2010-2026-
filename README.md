# Mean-Reversion-Research-on-RTX-LMT-NOC-stock-2010-2026-
This research aims to test the hypothesis if there is a mean reversion in a small sample of defense stocks (RTX, LMT, NOC). 


## Overview
This project tests whether large single-day price drops in defense-sector stocks (RTX, LMT, NOC) 
are followed by statistically abnormal recovery over the following days — a short-term 
mean-reversion effect. The analysis emphasizes methodological rigor over headline results: 
proper train/test separation, sample-size awareness, and correction for multiple comparisons, 
rather than reporting the first favorable-looking number found.

## Hypothesis
After a large one-day price drop, a stock's return over the following N days is higher than 
would be expected on a typical day — motivated by the idea that sharp drops are often driven 
by short-term liquidity effects (forced selling, overreaction) rather than a genuine change in 
fundamental value, and partially reverse once selling pressure passes.

## Data & Methodology
- **Universe:** RTX, LMT, NOC — three large-cap US defense/aerospace primes, chosen for long 
  price history and comparable business/liquidity profiles.
- **Period:** Daily price data, split chronologically into a training window (2010–2020) and 
  an out-of-sample test window (2020–2026). The test period was never used to select or tune 
  parameters.
- **Signal definition:** A "signal day" is any day where the stock's return falls at or below a 
  given drop threshold (2%, 5%, 7%, 10%, 12%, 15% tested).
- **Outcome:** Forward return over a fixed holding period after the signal day (1, 2, 3, 4, 5, 
  7, 10, or 15 trading days tested), computed per ticker to avoid holding periods spanning 
  across different stocks.
- **Statistical test:** For each (threshold, holding period) combination, forward returns 
  following signal days are compared against forward returns on all other days using an 
  independent two-sample t-test (Welch's, unequal variance).
- **Multiple-comparisons correction:** With 48 combinations tested per period, raw p-values 
  understate the true false-positive risk. A Bonferroni correction is applied independently 
  within each period, after excluding cells with fewer than 20 observations (insufficient 
  statistical power for reliable inference, e.g. very large drop thresholds that occur only a 
  handful of times in the sample).

## Key Findings
1. **Initial grid search (uncorrected) suggested a promising cell** (4% threshold, 5-day hold, 
   ~72% win rate) in the training period — but this did not replicate out-of-sample, 
   consistent with overfitting from scanning ~48 parameter combinations on one dataset.
2. **After Bonferroni correction, no combination reached significance in the training period.**
3. **In the test period, the 2% threshold remained significant after correction** at several 
   holding periods (4, 10, 15 days), with large, stable sample sizes (n≈342–344).
4. **This test-period result is not treated as a confirmed, generalizable effect**, since it 
   has no counterpart in the training period. It may instead reflect conditions specific to 
   2020–2026 (COVID recovery, elevated defense-sector demand following 2022) rather than a 
   persistent market phenomenon.
5. A secondary hypothesis — that abnormally high trading volume on signal days strengthens the 
   effect — was tested and found no consistent difference between high- and low-volume 
   subgroups.

## Limitations
- Small sample sizes at extreme thresholds (10%+) limit what can be concluded there, even 
  with correction applied.
- Overlapping holding-period windows (e.g. 15-day forward returns computed from nearly every 
  day) mean observations are not fully independent, which likely inflates apparent 
  significance somewhat; block bootstrap or Newey-West standard errors would address this in 
  future work.
- Transaction costs and realistic execution assumptions are not yet incorporated — the current 
  scope is signal detection, not strategy profitability.
- Results are specific to three defense-sector tickers and may not generalize to other 
  sectors.

## Next Steps
- Test the 2% threshold finding on additional, unrelated tickers/sectors to check whether it 
  reflects a general market phenomenon or a defense-sector/period-specific pattern.
- Incorporate transaction costs and turnover into a full backtest of any surviving signal.
- Apply block bootstrap or overlapping-window-robust standard errors to address 
  non-independence across holding periods.


## Reproducing the Results
1. Clone the repository and ensure `data/RTX.csv`, `data/LMT.csv`, `data/NOC.csv` are present 
   (historical daily price data with Date, Price, Open, High, Low, Vol., Change % columns).
2. Install dependencies: pip install pandas numpy scipy statsmodels
3. Open and run `mean_reversion_analysis.ipynb` top to bottom in Jupyter.

## Repo Structure
mean-reversion-research/
├── README.md
├── mean_reversion_analysis.ipynb
└── data/
├── RTX.csv
├── LMT.csv
└── NOC.csv
