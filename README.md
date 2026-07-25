# 🌦️ Delhi Climate Forecasting — End-to-End ML & Power BI Dashboard

A complete data science project that analyzes 4+ years of Delhi's daily weather data, engineers time-based features, builds and compares regression models to forecast mean temperature, and visualizes the results in an interactive Power BI dashboard.



---

## 📌 Project Overview

This project follows a full data science lifecycle — from raw, messy climate data to a deployed, decision-ready dashboard:

1. **Data Understanding** — merged and profiled train/test weather records
2. **Data Cleaning & Preprocessing** — detected and corrected sensor/outlier errors
3. **Feature Engineering** — derived time-based features for seasonality
4. **Model Building** — trained and compared Linear Regression vs. Random Forest
5. **Model Evaluation** — quantified error and visualized prediction accuracy
6. **Dashboard** — built an interactive Power BI report for business-friendly insights

**Goal:** Predict Delhi's daily mean temperature from historical weather indicators (humidity, wind speed, pressure, and date-derived seasonality features) and present the findings through an interactive dashboard.

---

## 🗂️ Dataset

- **Source:** Daily Delhi Climate dataset (train + test), 2013–2017
- **Records:** 1,576 daily observations after merging (1,462 train + 114 test)
- **Features:**

| Column | Description |
|---|---|
| `date` | Date of observation |
| `meantemp` | Mean temperature (°C) — target variable |
| `humidity` | Mean humidity (%) |
| `wind_speed` | Mean wind speed (km/h) |
| `meanpressure` | Mean atmospheric pressure (hPa) |

---

## 🧹 Data Cleaning & Preprocessing

- Merged the train and test files into a single time series and converted `date` to `datetime`.
- Verified there were **zero missing values and zero duplicate rows** across the merged dataset.
- Identified **8 physically invalid `meanpressure` readings** (e.g., 7679 hPa, -3 hPa, 59 hPa) using a valid-range filter (900–1100 hPa), which is impossible on Earth's surface and indicates sensor error.
- Replaced invalid values with `NaN` and applied **linear interpolation** to preserve the time-series continuity instead of dropping rows.
- Verified the fix with boxplot inspection and confirmed **zero out-of-range values remained**.

## 🛠️ Feature Engineering

Extracted seasonality and calendar signals from the `date` column to help the models capture cyclical weather patterns:

- `year`, `month`, `day`, `day_of_week`, `quarter`
- `is_weekend` (binary flag for Saturday/Sunday)

These features let tree-based models learn seasonal and weekly temperature patterns without needing an explicit time-series model.

---

## 🤖 Model Building & Evaluation

Two regression models were trained on the same feature set (`humidity`, `wind_speed`, `meanpressure`, `year`, `month`, `day`, `day_of_week`, `quarter`) to predict `meantemp`, and evaluated on a held-out 2017 test period.

| Model | MAE | RMSE | R² Score |
|---|---|---|---|
| Linear Regression | 4.77 | 5.68 | 0.196 |
| **Random Forest Regressor** | **2.60** | **3.29** | **0.730** |

**Random Forest outperformed Linear Regression by a wide margin**, improving R² from ~0.20 to ~0.73 and cutting average error (MAE) by nearly 45%. This confirms that temperature depends on **non-linear interactions** between humidity, pressure, wind speed, and seasonality — relationships a linear model cannot capture.

- Final model: `RandomForestRegressor(n_estimators=100, random_state=42)`
- Average prediction error on unseen 2017 data: **-0.92°C** (near-zero bias, per the dashboard's error gauge)
- Residuals were visualized with an error-distribution histogram to confirm the model doesn't systematically over/under-predict.

---

## 📊 Power BI Dashboard

The final predictions were exported to CSV and visualized in an interactive Power BI report for non-technical stakeholders:

- **KPI cards:** Average Temperature, Predicted Avg Temperature, Average Humidity, Wind Speed, Pressure
- **Actual vs. Predicted Temperature** trend line to visually validate model accuracy over time
- **Average Temperature by Day of Week** and **Monthly Average Temperature** to surface seasonal patterns
- **Temperature vs. Wind Speed** scatter plot to explore feature relationships
- **Average Error gauge** for at-a-glance model performance
- **Interactive slicers** (Day of Week, Month) for drill-down exploration

---

## 🧰 Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3.13 |
| Data Handling | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn, Power BI |
| Machine Learning | Scikit-learn (Linear Regression, Random Forest) |
| Environment | Jupyter Notebook |

---

## 📁 Repository Structure

```
Delhi-Climate-Forecasting/
│
├── Notebooks/
│   ├── Data Understanding.ipynb
│   ├── DataCleaning &Preprocessing.ipynb
│   ├── Feature Engineering.ipynb
│   ├── Model Building.ipynb
│   └── Model Evaluation & Comparison.ipynb
│
├── Data/
│   ├── DailyDelhiClimateTrain.csv
│   ├── DailyDelhiClimateTest.csv
│   ├── DailyDelhiClimateMerged.csv
│   ├── DailyDelhiClimateCleaned.csv
│   ├── Delhi_Climate_Feature_Engineered.csv
│   ├── Temperature_Predictions.csv
│   └── Delhi_Climate_Final_Result.csv
│
├── Dashboard/
│   ├── Dashboard.pbix
│   ├── Dashboard.png
│   └── Background Image.png
│
└── README.md
```

---

## 🔍 Key Insights

- Delhi shows a **strong seasonal temperature cycle**, rising steadily from winter (Jan) through summer (Apr), consistent with the Monthly Average Temperature trend.
- Weekday temperature averages are relatively stable, suggesting **day-of-week has limited predictive power** compared to `month` and `quarter`.
- **Pressure data required correction** before modeling — a reminder that real-world sensor data often contains silent, non-obvious errors that standard `.isnull()` checks won't catch.
- The Random Forest model generalizes well to a full unseen year (2017), making it suitable for short-term temperature forecasting support.


---

## 👤 Author

**Om**
B.E. Information Technology, C. K. Pithawalla College of Engineering & Technology (GTU)
📫 Connect on [LinkedIn](https://www.linkedin.com/in/om-maniya-07409b31a/) | [GitHub](https://github.com/om0302)
---

⭐ If you found this project useful, consider giving it a star!


[def]: #