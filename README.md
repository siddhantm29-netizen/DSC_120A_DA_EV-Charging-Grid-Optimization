# EV Charging Grid Optimization

A data analysis project looking at what really drives EV charging outcomes — and how that could feed into smarter, grid-aware charging decisions. We dug into 8,354 real charging sessions across 20 stations, built a couple of predictive models, ran a clustering pass, and pulled it all together into a report and a presentation deck.

**Course:** DSC 120A
**Status:** Complete — analysis, modeling, and reporting all wrapped up

---

## Team & Roles

| Name | Role |
|---|---|
| Siddhant Santosh Mathapati | Team Lead |
| Sharath Chandra Revu | Data Engineer |
| Kanishk Akula Damodar | ML Engineer |
| Valluri Venkata Praveen | Documentation & Data Analyst |

---

## What's in here

| File | What it is |
|---|---|
| `ev_charging_grid_optimization_eda.ipynb` | The full analysis notebook — EDA, modeling, tuning, clustering, and extended feature engineering, all runnable end to end |
| `EV_Charging_Grid_Optimization_Report.docx` | The final written report, with findings and recommendations |
| `EV_Charging_Grid_Optimization_Deck.pptx` | A slide deck for presenting the project |
| `cleaned_ev_charging_data.csv` | The dataset — 8,354 sessions, already cleaned and ready to use |
| `optimization_reward_predictions_final.csv` | Predictions from the recommended model (Linear Regression + engineered features), with Random Forest predictions alongside for comparison |
| `figures/` | Every chart from the notebook, saved as a standalone PNG |

---

## The dataset

8,354 charging sessions spread across 20 stations and 10 chargers, covering a full week (Monday through Sunday, Peak and Off-Peak time slots). Each row captures things like vehicle type, wait time, station load, queue length, electricity price, renewable energy ratio, weather, traffic, and a composite `optimization_reward` score meant to reflect how "good" that session was from a grid-optimization standpoint.

It showed up clean — no missing values, no duplicate sessions — so we didn't need a separate cleaning step before diving in.

---

## What we actually did

**Exploratory analysis.** We looked at how load, wait times, and pricing vary by station, by time of day, and by conditions like weather and traffic. A few stations run consistently hotter than others, and peak hours bring longer waits and higher prices — pretty much what you'd expect, but it's good to see it confirmed in the data.

**Predictive modeling.** We tried to predict the optimization reward using only information you'd know before a session starts — things like station load, queue length, price, and renewable ratio. We compared a plain linear regression against a tuned Random Forest, and honestly, the linear model came out slightly ahead. That's actually a useful finding: it means the relationships in this data are mostly straightforward rather than deeply non-linear, so a simpler model does the job just as well (and it's easier to explain to stakeholders).

**Clustering.** Beyond prediction, we used K-Means to check whether sessions naturally fall into distinct groups. They do — two clusters, split mainly by station load. One cluster runs quieter with better renewable access, the other runs busier with less clean power behind it. That's a handy way to think about network conditions instead of treating everything as one average case.

**Extended feature engineering.** The project proposal called for richer, session-level features on top of the raw columns — rolling load averages, session intensity, and station utilization. We built those out using the real timestamps already in the dataset, then ran the fair comparison: both models (Linear Regression and Random Forest), both feature sets (original and original-plus-engineered). The result is a clean answer — **Linear Regression with the engineered features wins outright** (R² 0.727, the best MAE and RMSE of the four combinations), narrowly beating plain Linear Regression (0.725) and clearly beating both Random Forest variants (0.708 and 0.716). The engineered features genuinely help — just not enough to change which model family wins. Updated recommendation: Linear Regression on the original plus engineered feature set is the model to put into production.

---

## Key findings

- **Congestion is the biggest lever.** Queue length and station load matter more than price or weather when it comes to session outcomes.
- **The relationships are mostly linear.** A tuned Random Forest barely beats plain linear regression, so complexity isn't buying much here.
- **Two clear session types exist.** A lower-load, higher-renewable group and a higher-load, lower-renewable group — worth planning around separately.
- **Peak hours hit on every front.** Higher load, longer waits, and higher prices all show up together during peak times.
- **Renewable availability doesn't track demand.** It seems to be driven more by generation conditions than by how much charging is happening.

---

## How to run it

1. Clone this repo.
2. Open `ev_charging_grid_optimization_eda.ipynb` in Jupyter or upload it to Kaggle.
3. Make sure `cleaned_ev_charging_data.csv` is in the same directory (or update the file path in the first cell).
4. Run all cells — the notebook installs what it needs and walks through EDA, modeling, tuning, and clustering in order.

Main dependencies: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`.

---

## Recommendations

- Focus on congestion management (load balancing, smarter scheduling) before leaning on pricing changes.
- Use the two clusters to plan for "quiet" and "busy" scenarios separately rather than one blended strategy.
- Treat the high-load, low-renewable cluster as the top priority for investment — it's the worst combination on both fronts.
- Put Linear Regression with the engineered features into production as the scoring model — it's the most accurate option we found, and still simple and easy to explain.

---

## Next steps

- Build a lightweight dashboard for station-level monitoring.
- Extend data collection to include more granular weather and grid-supply data.
- Present findings and recommendations to stakeholders.

---

*Part of the DSC 120A EV Charging Grid Optimization project.*
