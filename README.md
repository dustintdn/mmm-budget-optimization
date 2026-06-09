# Marketing Mix Model: Budget Optimization

## Business Question

**Given a fixed marketing budget, are we spending it in the right places — and how should we reallocate it?**

---

## Approach

Traditional attribution (last-click, or "naive ROAS") systematically misleads budget decisions: it ignores the delayed effect of advertising (you see an ad Monday, you buy Thursday) and the diminishing returns from overinvesting in a single channel (doubling Facebook spend doesn't double Facebook sales).

Marketing Mix Modeling (MMM) solves both problems. By fitting a Bayesian model with **adstock** (time-lagged carry-over effects) and **saturation** (S-curve diminishing returns) transformations, we can estimate the true, causally-correct contribution of each channel to sales — and use that to mathematically optimize how the budget should be split.

---

## Key Findings

> **Note:** The numbers below are placeholders. Run the notebook (`notebooks/mmm_analysis.ipynb`) to populate with actual model results.

- **[Channel X] drives ~[Y]% of total incremental sales**, despite representing only [Z]% of total spend — the highest efficiency of any paid channel.
- **Email and Affiliate channels are approaching saturation**: the model's response curves show these channels are operating past the point of diminishing returns, meaning additional spend yields minimal incremental lift.
- **YouTube Paid shows significant headroom**: the current spend level sits on the steep part of the saturation curve, indicating strong marginal returns from additional investment here.

---

## Recommendation

Reallocating **~[X]% of budget from Email and Affiliate to YouTube Paid and Google Search** is projected to increase incremental sales by approximately **[Y]%** with no change to total spend. The key reallocation is shifting dollars away from over-saturated channels and into channels with remaining capacity on their response curves.

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

> **Note on PyMC:** PyMC uses a C++ compiler for model compilation. If you hit install issues, follow the [PyMC installation guide](https://www.pymc.io/projects/docs/en/stable/installation.html). Conda is the easiest path: `conda install -c conda-forge pymc pymc-marketing`.

### 2. Launch the notebook

```bash
jupyter notebook notebooks/mmm_analysis.ipynb
```

Run all cells top-to-bottom (**Kernel → Restart & Run All**). Model fitting (Phase 2) takes approximately 5–15 minutes depending on your machine.

---

## Caveats

- **Single market:** The model is fit on Division A (one of 26). Results are directionally representative but should be validated across divisions before acting on them.
- **Impressions as spend proxy:** The dataset contains impression/view counts rather than dollar spend, so budget allocation is expressed in normalized activity units. A production version would use actual media spend in dollars.
- **No external controls:** Seasonality, competitor activity, and macro-economic factors are not modeled. In a production MMM, these would be included as control variables to isolate true media contribution.
