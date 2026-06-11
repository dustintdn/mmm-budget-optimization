# Marketing Mix Model: Budget Optimization

## Business Question

**Given a fixed marketing budget, are we spending it in the right places, and how should we reallocate it?**

---

## Approach

Traditional attribution (last-click, or "naive ROAS") systematically misleads budget decisions: it ignores the delayed effect of advertising (you see an ad Monday, you buy Friday) and the diminishing returns from overinvesting in a single channel (doubling Facebook spend doesn't double Facebook sales).

Marketing Mix Modeling (MMM) solves both problems. By fitting a Bayesian model with **adstock** (time-lagged carry-over effects) and **saturation** (S-curve diminishing returns) transformations, we can estimate the true, causally-correct contribution of each channel to sales and use that to mathematically optimize how the budget should be split.

---

## Key Findings

- **Email drives ~47% of total incremental sales, despite representing only 25% of total spend**: the highest absolute contributor of any paid channel.
- **Facebook and Google Search are over-invested relative to their saturation curves**: both channels are operating in the diminishing-returns zone, where additional spend yields minimal incremental lift. The optimizer eliminates both from the recommended allocation.
- **YouTube Paid and YouTube Organic show significant headroom**: current spend sits on the steep part of both saturation curves, indicating strong marginal returns from additional investment.

---

## Recommendation

Reallocating **~23% of budget away from Facebook and Google Search toward YouTube Paid and YouTube Organic** is projected to increase incremental sales by approximately **1.2%** with no change to total spend. The key reallocation is shifting dollars away from over-saturated channels (Facebook: -9pp, Google Search: −14pp) and into channels with remaining capacity on their response curves (YouTube Paid: +11pp, YouTube Organic: +10pp).

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
