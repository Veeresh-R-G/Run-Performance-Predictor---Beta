# 🏃 Running Performance Predictor
> Predict marathon finish times from training data using Machine Learning  

---

## 📌 Problem Statement
Given a runner's weekly training data, can we predict their marathon finish time?  
This is the core problem companies like **Runna** and **Strava** solve at scale — personalised performance prediction.

---

## 📊 Dataset
- **Source:** [Marathon Time Predictions — Kaggle](https://www.kaggle.com/datasets/girardi69/marathon-time-predictions)
- **Size:** 87 runners, 10 raw features
- **Target:** `MarathonTime` (hours)
- ---

## 🔧 Features Used
| Feature | Description |
|---|---|
| `km4week` | Weekly kilometers run in training |
| `sp4week` | Average training speed (km/h) |
| `Wall21` | Halfway split performance indicator |
| `did_cross_train` | Binary: did runner cross-train? |
| `speed_efficiency` | Engineered: speed × weekly volume |
| `Category_encoded` | Age/gender category (M40, M45, MAM, WAM...) |

---

## 🧹 Data Cleaning Decisions
- Dropped `id`, `Name`, `Marathon` — identifiers with no predictive signal
- Dropped `CATEGORY` — duplicate of `Category`
- Removed `sp4week` outliers > 25 km/h (physically impossible values)
- `CrossTraining` had 85% missing → engineered into binary `did_cross_train` flag
- `Wall21` had `'-'` string values → replaced with 0, cast to float
- `Category` nulls filled with mode BEFORE label encoding (order matters!)

---
## 🤖 Models Trained & Results
| Model | MAE | RMSE | R² |
|---|---|---|---|
| Linear Regression | 0.1805h | 0.2303h | 0.6979 |
| Ridge Regression | 0.1717h | 0.2240h | 0.7142 |
| Random Forest | 0.1309h | 0.1779h | 0.8197 |
| **Gradient Boosting** ✅ | **0.1199h** | **0.1524h** | **0.8678** |

**Best Model:** Gradient Boosting  
**Mean Absolute Error:** 7.2 minutes  
**R² Score:** 0.8678 (model explains 86.8% of variance in finish times)

---

## 💡 Key Insights
- `km4week` is the strongest real predictor — more weekly volume = faster finish
- Tree-based models significantly outperform linear models — finish time has non-linear relationships
- `speed_efficiency` (engineered feature) added signal beyond raw speed alone
- Cross-training had minimal impact on its own, but the binary flag added marginal value

---

## 🛠️ Tech Stack
- Python, Pandas, NumPy
- Scikit-learn (LinearRegression, Ridge, RandomForest, GradientBoosting)
- Matplotlib, Seaborn
- Kaggle Notebooks

---

## 🚀 How to Run
1. Clone this repo
2. Open `notebook.ipynb` in Kaggle or Jupyter
3. Run all cells top to bottom
4. Modify the `new_runner` dict in the last cell to predict YOUR finish time

---

## 📈 Sample Prediction
```python
new_runner = {
    'km4week'        : 85,     # runs 85km/week
    'sp4week'        : 11.5,   # avg speed 11.5 km/h  
    'Category'       : 'MAM',  # Male Amateur
    'did_cross_train': 1       # cross trains
}
# → Predicted finish time: ~3h 15min
```


---

## 🔗 Author
Built by Veeresh
