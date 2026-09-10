# Option Pricing Under Time-Varying Volatility: Black-Scholes vs. GARCH(1,1)

## Summary
Black-Scholes assumes constant volatility, but real market returns violate this assumption. 
This project quantifies the practical impact: comparing option prices using naive historical 
volatility vs. GARCH(1,1)-implied volatility on SPY shows a **33.4% pricing difference** 
($9.26 vs. $12.35) for a 30-day at-the-money call, driven by GARCH detecting a recent uptick 
in volatility that a trailing historical average smooths over.

## Approach
1. Validated a Monte Carlo option pricing simulator against the Black-Scholes closed-form 
   solution to confirm correctness before building on top of it.
2. Tested Black-Scholes' core assumptions against ~10 years of real SPY return data — found 
   excess kurtosis of ~13.6 (fat tails) and clear volatility clustering, both violating the 
   constant-volatility, normally-distributed-returns assumption.
3. Fit a GARCH(1,1) model via maximum likelihood estimation to capture time-varying volatility, 
   and validated it by confirming the conditional volatility tracks realized volatility.
4. Repriced a 30-day at-the-money SPY call under both naive historical volatility and 
   GARCH-implied volatility to quantify the mispricing.

## Key Result
Using GARCH(1,1)-implied volatility instead of naive trailing-252-day historical volatility changes the price of a 30-day at-the-money SPY call by 33.4% ($9.26 → $12.35), driven by GARCH detecting a recent volatility uptick that the historical average smooths over.

## Tools
Python, NumPy, SciPy, pandas, matplotlib, `arch` (GARCH estimation), `yfinance` (data)

## Setup
```bash
pip install -r requirements.txt
jupyter notebook notebooks/option_pricing_garch.ipynb
```

## Structure
- `src/pricing.py` — Black-Scholes and Monte Carlo pricing functions
- `notebooks/option_pricing_garch.ipynb` — full analysis and results
- `data/` — cached SPY price data
