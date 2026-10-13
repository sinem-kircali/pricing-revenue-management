# Pricing and Revenue Management: Price-Response Estimation to Optimal Prices

This project starts from a price experiment and works through the main pricing decisions. It estimates how demand responds to price, then finds profit-maximising prices for four cases: a single price, market segmentation with cannibalisation, day-of-week pricing, and capacity-constrained (theatre) pricing. The optimisation models are solved with Gurobi.

Built for the *Pricing and Revenue Management* course (Industrial Engineering, Koç University), as a pair project with Ece Numanoğlu.

## Data
A 151-day online price experiment (Jan – May 2022). Each day records:
- price and sales;
- **potential demand** (visitors, from click data);
- day of the week, a competitor's price and rain.

The first 114 days are used for estimation and the last 37 days for testing.

## What's inside
| Section | Method | Key result |
|---|---|---|
| A. Price response | linear, exponential and logit models from price and sales | price alone explains about 22% of sales variation; test MAPE ≈ 23–25% |
| B. Conversion rate + potential demand | conversion rate r(p) = sales ÷ visitors; potential demand from weekend, rain and competitor price | combined forecast: **test MAPE 12.7%**, about half the price-only error |
| C. Optimal single price | profit maximisation for costs 0–30 TL | logit optimum ≈ 98 TL at 30 TL cost; linear and exponential optima lie outside the tested 95–115 TL range |
| D. Segmentation | two prices split at 60 TL willingness-to-pay, with cannibalisation α (Gurobi, non-convex QP) | +8% profit at α = 0; better than a single price only while **α < 0.16** |
| E. Day-of-week pricing | regression with significant day effects, then a joint price optimisation | small gain (+0.5%): only weekends differ, and by 6–10 TL |
| F. 400-seat theatre | single / daily / two prices / two prices with weekend-to-weekday diversion | daily prices **+49% revenue**; two prices +35%; diversion narrows the price gap |

## Notes on this version
The notebook merges two homework notebooks into one analysis and fixes several issues:
- The exponential conversion-rate model reused the predictions of a different model, which gave a meaningless MAPE of 6,500%.
- The potential-demand regression was scored against sales instead of potential demand.
- One combined forecast used the *actual* test conversion rates, which leaked the answer. It now uses the fitted conversion-rate model, chosen on training error.
- Day-of-week pricing now maximises profit with the 30 TL incremental cost, as specified; the earlier version maximised revenue.
- Segment demand curves come from the fitted model instead of hard-coded numbers.
- The optimal-price tables (previously in Excel) are reproduced in code.

## Structure
```
pricing_revenue_management.ipynb   full analysis with outputs (sections A–F)
data/                               place HW1data.csv here (not included)
requirements.txt
```
Gurobi's free size-limited license (installed with `pip install gurobipy`) is enough for these models.

## Tech stack
Python · pandas · statsmodels · Gurobi (gurobipy) · matplotlib
