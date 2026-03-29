# 🎙️ Podcast "Gemischtes Hack" — Episode Length Analysis

> *Are episodes of the podcast "Gemischtes Hack" getting longer over time?*

A simple regression analysis using episode data retrieved from the Spotify API to test the hypothesis made by the hosts in episode *"#333 DEUTSCHE WATERGATE"*.

---

## Background

The hosts of the German podcast **Gemischtes Hack** speculated that their episodes have been getting longer over time. This project puts that claim to the test using real episode data and a linear regression model with time as the independent variable and episode duration as the dependent variable.

---

## Dataset

- **Source**: Spotify API
- **Storage**: SQLite database (`episodes.db`)
- **Size**: 353 episodes
- **Features used**:

| Column | Description |
|---|---|
| `release_date` | Date the episode was published |
| `duration_ms` | Episode length in milliseconds |
| `description` | Episode description text (used for outlier verification) |

---

## Methodology

### 1. Data Preparation
- Duration converted from milliseconds to minutes
- Time encoded as *weeks since first episode* for interpretability
- No missing values

### 2. Outlier Handling
Four episodes under 20 minutes were identified as irregular. Their descriptions were verified using German keyword search (`krank`, `gesundheitliche Gründe`, `fällt aus`, `ausfallen`) — all confirmed as "no show" announcements due to illness. These were removed before modelling.

### 3. Regression Assumptions Checked

| Assumption | Method | Result |
|---|---|---|
| Linear relationship | Scatter plot | ✅ Satisfied |
| Normal residuals | Histogram | ✅ Approximately normal |
| Homoskedasticity | Visual + Breusch-Pagan test | ✅ No significant heteroskedasticity |
| No autocorrelation | Durbin-Watson test | ✅ Value near 2 |

### 4. Models
Two OLS models were compared:
- **Model A**: All episodes ≥ 20 minutes (n=349)
- **Model B**: All episodes ≥ 40 minutes — sensitivity check excluding early short episodes

---

## Results

| | Model A (≥20 min) | Model B (≥40 min) |
|---|---|---|
| Intercept | 65.97 min | 67.48 min |
| Slope (per week) | +0.044 min | +0.039 min |
| R² | 0.201 | 0.176 |
| p-value (slope) | ~1.1e-18 | ~3.5e-16 |

**The hypothesis is confirmed**: episode length has increased significantly over time. Each additional week is associated with roughly **2.6 minutes of extra length per year** (Model A).

However, with R² ≈ 0.20, time alone explains only about 20% of the variation in episode length — other factors likely play a role too.


---

## Requirements

```bash
pip install -r requirements.txt
```

---

## Limitations

- **Low R²**: The model explains ~20% of variance. Episode length is influenced by many factors beyond time.
- **Single predictor**: A multivariate model could improve fit.
- **No causal inference**: The analysis shows correlation between time and length, not a causal mechanism.

---

## Personal disclaimer

The project idea was mine and I also executed the project myself (I wrote all code). The readme was however written by Claude and adapted by me.
