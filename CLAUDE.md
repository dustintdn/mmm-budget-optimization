# MMM Budget Optimization — Project Context

## Project Purpose
End-to-end Bayesian Marketing Mix Model portfolio project. Business goal: given a fixed marketing budget, determine if it's optimally allocated across channels and produce a data-driven reallocation recommendation.

Intended audience: recruiters/hiring managers at Google/Meta/major CPG firms targeting a Business Data Scientist (Marketing) role.

## Dataset
- File: `data/media_spends.csv` (not committed to git — add manually)
- Source: Kaggle "Division-Level Marketing Spend Dataset"
- 26 divisions × 113 weeks (2018–2020) = ~3,051 rows
- Columns: Division, Calendar_Week, Facebook_Impressions, Google_Impressions, Email_Impressions, Paid_Views (YouTube), Organic_Views (YouTube), Affiliate_Impressions, Overall_Views, Sales
- Note: columns are impressions/views, NOT dollar spend. Treated as spend proxy in the model.

## Tech Stack
- `pymc-marketing` ≥ 0.9 — MMM with GeometricAdstock + LogisticSaturation
- `pymc` ≥ 5.16 — Bayesian MCMC engine (NUTS sampler)
- `scipy.optimize.minimize` (SLSQP) — budget optimizer
- `sklearn.preprocessing.MaxAbsScaler` — input normalization
- `arviz` — posterior diagnostics

## Notebook Structure (`notebooks/mmm_analysis.ipynb`)
- Phase 1: EDA — channel activity chart, sales chart, naive ROAS, stacked area share
- Phase 2: MMM fitting — normalize → build MMM → NUTS sample → posterior summary
- Phase 3: Business outputs — contribution decomposition, response curves, budget optimizer
- Phase 4: Caveats — limitations and production roadmap

## Key Modeling Decisions
- Filter to Division A as representative single market (all 26 divisions share same structure)
- MaxAbsScaler on channels, mean-normalize sales target before fitting
- GeometricAdstock(l_max=8): up to 8 weeks carry-over
- LogisticSaturation per channel
- NUTS: draws=1000, tune=1000, target_accept=0.9, chains=2, random_seed=42
- Budget optimizer uses posterior mean saturation lam parameters

## Known Caveats (important for interview context)
1. Impressions ≠ dollar spend — allocation expressed in normalized units
2. Single-division model — would need hierarchical extension for all 26 divisions
3. No competitor data or offline channels in model
4. 113 weeks is minimal — longer history improves seasonality estimates
