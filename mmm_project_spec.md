# MMM Portfolio Project Description


## Project Brief

Build a end-to-end **Marketing Mix Model (MMM)** portfolio project in Python, structured as a well-documented Jupyter notebook. The goal is to answer one business question: **"Given a fixed marketing budget, are we spending it in the right places — and how should we reallocate it?"**

This project is intended as a public GitHub portfolio piece targeting a Business Data Scientist (Marketing) role. The emphasis is on **business storytelling and actionable recommendations**, not just modeling rigor.

---

## Dataset

Download the **Division-Level Marketing Spend Dataset** from Kaggle:
- URL: https://www.kaggle.com/datasets/yugagrawal95/sample-media-spends-data
- Weekly data (2018–2020, 113 weeks) across 8 media channels: Facebook, Google Search impressions, Email impressions, YouTube Paid, YouTube Organic, Affiliate channel views, and overall views, plus a sales column.
- If the dataset isn't available, use `pymc_marketing.mmm.utils.simulate_mmm_data()` to generate a realistic stand-in and note this in the README.

---

## Tech Stack

```
pymc-marketing    # MMM with adstock + saturation built in
pymc              # underlying Bayesian engine
pandas
numpy
matplotlib
seaborn
scipy             # budget optimizer
jupyter notebook
```

Create a `requirements.txt` and a `README.md` alongside the notebook.

---

## Project Structure

```
mmm-marketing-portfolio/
├── README.md
├── requirements.txt
├── data/
│   └── media_spends.csv
└── notebooks/
    └── mmm_analysis.ipynb
```

---

## Notebook Structure

Build the notebook in four clearly labeled phases. Each phase should have a markdown cell at the top with a plain-English explanation of what we're doing and why — written for a marketing executive, not a data scientist.

---

### Phase 1: EDA & Data Storytelling

Goal: Tell a story with the data before any modeling.

- Load and inspect the dataset; handle any nulls or formatting issues
- Plot each channel's spend over time as a multi-line chart — identify seasonality or spend spikes
- Plot weekly total sales over time
- Calculate "Naive ROAS" per channel (total sales / total spend) as a simple baseline — include a callout that this is misleading because it ignores time lags and diminishing returns (set up the motivation for MMM)
- Visualize spend share by channel as a stacked area chart over time
- End with a 3–4 sentence markdown summary: "What the data tells us before modeling"

---

### Phase 2: MMM with PyMC-Marketing

Goal: Fit a Bayesian MMM with adstock and saturation transformations.

Use `pymc_marketing.mmm.MMM` as the primary modeling class.

- Include markdown cells explaining adstock (spend has a decaying effect over time — last week's ad still influences this week's sales) and saturation (diminishing returns — doubling spend doesn't double sales) in plain English before the code
- Apply geometric adstock decay per channel
- Apply logistic or Hill saturation curves per channel
- Fit the model using NUTS sampler (use `target_accept=0.9`, `draws=1000`, `tune=1000` as a starting point — note that longer chains improve accuracy)
- Print a model summary table with posterior means and HDI intervals per channel

---

### Phase 3: Business Outputs

Goal: Translate model outputs into executive-ready insights. This is the most important phase.

**Output 1 — Channel Contribution Decomposition**
- Stacked bar/area chart showing what % of weekly sales came from each channel vs. the baseline (organic sales with zero spend)
- Markdown callout: which channel drives the most incremental sales?

**Output 2 — Response Curves**
- Plot the fitted saturation curve for each channel (spend on x-axis, incremental sales on y-axis)
- Mark the current average spend level on each curve
- Markdown callout: which channels are approaching saturation? Which have room to scale?

**Output 3 — Budget Optimizer**
- Use `scipy.optimize.minimize` to reallocate the current total budget across channels to maximize predicted incremental sales given the fitted response curves
- Present a side-by-side table: Current Allocation vs. Recommended Allocation vs. Projected Lift
- Frame the output as: "Reallocating $X from Channel A to Channel B is projected to increase incremental sales by Y%"

---

### Phase 4: Caveats & Next Steps

A short markdown-only section covering:
- Model limitations (data length, no offline channels, no competitor data)
- What would make this more robust in a production setting (geo-level data, calibration with holdout experiments, longer time series)
- One-sentence framing of how this connects to real-world marketing measurement at scale

---

## README Requirements

Write the README as an executive summary, not a technical doc. Structure:

1. **Business Question** — one sentence
2. **Approach** — 2–3 sentences on MMM and why it's better than naive attribution
3. **Key Findings** — 3 bullet points with specific numbers from the model (channel X drives Y% of incremental sales, channel Z is oversaturated, etc.)
4. **Recommendation** — the budget reallocation recommendation in plain English with projected lift
5. **How to Run** — install instructions and how to launch the notebook
6. **Caveats** — 2–3 bullets

The README should be readable by someone who has never heard of MMM.

---

## Code Quality Standards

- Every code cell should have a comment explaining what it does
- No cell should be longer than ~30 lines — break complex logic into smaller cells
- All charts must have titles, labeled axes, and a brief caption in the markdown cell below them
- Use consistent color palette across all charts (suggest a muted, professional palette — no default matplotlib colors)
- The notebook should run top-to-bottom without errors after a fresh kernel restart

---

## What Success Looks Like

A recruiter or hiring manager at Google should be able to open the README, read it in 2 minutes, and understand: what problem was solved, what was found, and what was recommended — without opening the notebook. The notebook itself should then serve as the technical proof.
