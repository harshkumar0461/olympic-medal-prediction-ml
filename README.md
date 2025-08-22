# Olympic Medal Prediction (Scikit-Learn)

> My first end-to-end machine learning project — predicting how many medals a national team will win at an Olympics using a simple linear regression model.

<img width="490" height="489" alt="download" src="https://github.com/user-attachments/assets/47665375-8271-4caf-81f1-5727fe2d64b5" />

Medals vs Athletes

<img width="489" height="489" alt="download (1)" src="https://github.com/user-attachments/assets/6b203a8f-53ff-4979-ab9c-7e649baa4c1c" />

Medals vs Age

## Why this project?

I wanted an approachable, *learn-by-doing* project that touches the full ML workflow: loading data, exploring it, training a model, evaluating results, and sharing everything in a clean, reproducible notebook. This repo is the result.

---

## Problem statement

Given basic information about a country's Olympic **team** in a particular year, predict the **number of medals** that team will win.

- **Type:** supervised regression  
- **Target:** `medals` (count of medals)  
- **Features used in v1:** `athletes` (team size) and `prev_medals` (medals won in the previous Olympics)

> ⚠️ This is intentionally a minimalist baseline. It’s not meant to be a leaderboard model — it’s a learning project.

---

## Dataset

The dataset lives in `teams.csv` (included in this repo). Each row is a country’s team at a specific Summer Olympics.

**Columns (subset used in v1):**
- `team` – team/country code  
- `country` – country name  
- `year` – Olympic year  
- `athletes` – number of athletes sent  
- `age` – average age of the team  
- `prev_medals` – medals from the previous Olympics  
- `medals` – medals in the current Olympics (the target)

**Quick stats on this copy:**
- Rows: **2014**  
- Years covered: **1964–2016**  
- Unique teams: **214**  
- Unique countries: **230**

> If you know the original source of this exact file, open an issue — I’ll add proper attribution.

---

## Approach (v1)

1. **Select columns**: `["team", "country", "year", "athletes", "age", "prev_medals", "medals"]`
2. **Clean**: drop rows with missing values.
3. **Train/test split** by time to avoid leakage:  
   - **Train:** rows with `year < 2012`  
   - **Test:** rows with `year ≥ 2012` (i.e., 2012 and 2016 cycles)
4. **Model:** `sklearn.linear_model.LinearRegression`
5. **Prediction post-processing:** round predictions to the nearest integer and clip negatives to 0.
6. **Metrics:** Mean Absolute Error (MAE) and R² on the test set.

---

## Results

- **Test MAE:** **3.30 medals**  
- **Test R²:** **0.921**

For context, a very simple baseline that just predicts `prev_medals` achieves:
- **Baseline MAE:** **3.45**  
- **Baseline R²:** **0.897**

So the linear model provides a **modest improvement** over the naive baseline.

### Sample predictions

First ten rows from the held-out test period:

| year | team | country         | athletes | prev_medals | medals | predictions |
|----:|:-----|:-----------------|---------:|------------:|-------:|------------:|
| 2012 | AFG | Afghanistan      | 6   | 1 | 1 | 0 |
| 2012 | ALB | Albania          | 10  | 0 | 0 | 0 |
| 2012 | ALG | Algeria          | 39  | 2 | 1 | 4 |
| 2012 | AND | Andorra          | 6   | 0 | 0 | 0 |
| 2012 | ANG | Angola           | 34  | 0 | 0 | 0 |
| 2012 | ANT | Antigua          | 5   | 0 | 1 | 0 |
| 2012 | ARG | Argentina        | 137 | 6 | 4 | 7 |
| 2012 | ARM | Armenia          | 25  | 6 | 3 | 3 |
| 2012 | ARU | Aruba            | 4   | 0 | 0 | 0 |
| 2012 | ASA | American Samoa   | 5   | 0 | 0 | 0 |

A couple of specific teams for quick intuition:

**USA**

| year | team | medals | predictions | athletes | prev_medals |
|----:|:-----|-------:|------------:|---------:|------------:|
| 2012 | USA | 248 | 285 | 689 | 317 |
| 2016 | USA | 264 | 236 | 719 | 248 |

**India (IND)**

| year | team | medals | predictions | athletes | prev_medals |
|----:|:-----|-------:|------------:|---------:|------------:|
| 2012 | IND | 6 | 7  | 95  | 3 |
| 2016 | IND | 2 | 12 | 130 | 6 |

> You’ll notice some big errors for certain teams/years (e.g., **IND 2016**). With only two features, the model can’t capture effects like home advantage, sport mix, funding, new programs, etc. That’s expected — and it’s a great opportunity for iteration.

What I learned / next steps

✅ How to structure a small ML repo (data → explore → model → evaluate → share)

✅ Why time-based splits matter for forecasting-style tasks

✅ Interpreting MAE and R²

## 🤝 Contributing

Suggestions and pull requests are welcome — especially for:
- Better features  
- Clearer visualizations  
- Links to the original data source  
