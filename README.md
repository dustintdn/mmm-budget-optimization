# Marketing Mix Model: Budget Optimization

## Business Question

**Given a fixed marketing budget, are we spending it in the right places, and how should we reallocate it?**

---

## Approach

Traditional attribution (last-click, or "naive ROAS") systematically misleads budget decisions: it ignores the delayed effect of advertising (you see an ad Monday, you buy Friday) and the diminishing returns from overinvesting in a single channel (doubling Facebook spend doesn't double Facebook sales).

Marketing Mix Modeling (MMM) solves both problems. By fitting a Bayesian model with **adstock** (time-lagged carry-over effects) and **saturation** (S-curve diminishing returns) transformations, we can estimate the true, causally-correct contribution of each channel to sales and use that to mathematically optimize how the budget should be split.

---

## Data

Kaggle "Division-Level Marketing Spend Dataset" (`data/media_spends.csv`):
weekly marketing activity and sales for 26 retail divisions over 113 weeks
(Jan 2018 – Feb 2020). The model only fits on Division A — a single
representative market of 113 weekly observations; the same structure applies
to any division.

Six media channels are used as predictors, with weekly Sales as the target:

| Model channel | Source column |
|---|---|
| Facebook | Facebook_Impressions |
| Google Search | Google_Impressions |
| Email | Email_Impressions |
| YouTube Paid | Paid_Views |
| YouTube Organic | Organic_Views |
| Affiliate | Affiliate_Impressions |

Note: these are impression/view counts, not dollar spend, so they're treated
as a spend proxy and all allocations are expressed in normalized activity
units (see Caveats).

---

## Key Findings

- **Email drives ~47% of incremental sales on ~25% of activity** — the largest single contributor, though its high saturation rate (lam ≈ 2.7) means it has limited room to grow.
- **Google Search and Facebook have the highest effectiveness coefficients** (beta ≈ 1.1 and 0.7) and the slowest-saturating response curves, making them the best candidates for additional investment.
- **YouTube Paid, YouTube Organic, and Affiliate contribute little incrementally** (under 8% combined) — their effectiveness coefficients are near zero, despite Affiliate receiving ~30% of current activity.

---

## Recommendation

The optimizer shifts budget toward Google Search (+37pp, to 51% of total) and Facebook (+18pp), funded by cutting Affiliate (−30pp) and both YouTube channels (−22pp combined), with Email roughly held (−3pp). Projected incremental sales lift: **+84%** at constant total spend.

Two caveats on that number. It comes from static response curves at mean weekly spend and ignores adstock timing, so it is an upper bound rather than a forecast. And fully zeroing three channels leans on point estimates of near-zero coefficients that carry wide uncertainty intervals — in practice this would be tested as a staged reallocation, not a hard cut.

---

## Repository Structure

```
mmm-budget-optimization/
├── data/
│   └── media_spends.csv          # Kaggle dataset
├── notebooks/
│   └── mmm_analysis.ipynb        # End-to-end analysis: EDA → MMM fit → optimization
├── requirements.txt
└── README.md
```

---

## How to Run

### 1. Install dependencies

```bash
# Create a virtual environment (recommended)
python -m venv .venv
source .venv/bin/activate   # macOS / Linux
# .venv\Scripts\activate    # Windows

# Install packages
pip install -r requirements.txt
```

### 2. Launch the notebook

```bash
jupyter notebook notebooks/mmm_analysis.ipynb
```

Run all cells top-to-bottom (**Kernel → Restart & Run All**). Model fitting (Phase 2) takes approximately 5–15 minutes.

---

## Caveats

- **Single market:** The model is fit on Division A (one of 26). Results are directionally representative but should be validated across divisions before acting on them.
- **Impressions as spend proxy:** The dataset contains impression/view counts rather than dollar spend, so budget allocation is expressed in normalized activity units. A production version would use actual media spend in dollars.
- **No external controls:** Seasonality, competitor activity, and macro-economic factors are not modeled. In a production MMM, these would be included as control variables to isolate true media contribution.
