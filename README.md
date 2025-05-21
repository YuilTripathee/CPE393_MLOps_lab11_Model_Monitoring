# ✈️ Flight Delay Regression Report

This project evaluates the model performance for predicting flight arrival delays (`ArrDelay`) using the [DelayedFlights dataset](https://www.kaggle.com/datasets/giovamata/airlinedelaycauses) and simulates prediction quality analysis using [Evidently AI](https://www.evidentlyai.com/).

---

## 📦 Dataset Summary

- **Source**: Bureau of Transportation Statistics (via Kaggle)
- **File**: `DelayedFlights.csv`
- **Target Variable**: `ArrDelay` (renamed to `target`)
- **Features Used**:
  - `DepDelay`: Departure delay
  - `Distance`: Flight distance
  - `AirTime`: Time in the air
- **Prediction Simulation**: Created by adding Gaussian noise (`N(0, 10)`) to the true target.

---

## 🧪 Simulation Setup

- **Reference Dataset**: 5,000 random samples from historical data
- **Current Dataset**: 5,000 random samples from recent data
- **Use Case**: Simulate model quality monitoring over time

---

## 📊 Key Findings from Evidently Report

### ✅ Model Performance Overview

| Metric                   | Reference | Current | Interpretation                                             |
| ------------------------ | --------- | ------- | ---------------------------------------------------------- |
| **Mean Error**           | -0.23     | -0.08   | Model slightly under-predicts on average in both datasets. |
| **MAE (Mean Abs Error)** | 8.11      | 8.06    | Consistent performance between past and recent data.       |
| **RMSE**                 | 10.126    | 10.106  | Low variation in prediction error; stable model behavior.  |
| **R² Score**             | 0.968     | 0.969   | Very high — model explains nearly all delay variance.      |
| **Abs Max Error**        | 35.44     | 36.98   | A few outliers exist with large error, but not excessive.  |

### ⚠️ Dummy Model Comparison

| Metric         | Dummy Model | Actual Model | Interpretation                                   |
| -------------- | ----------- | ------------ | ------------------------------------------------ |
| **Dummy MAE**  | 8.065       | 8.06         | Model performs nearly the same as naive average. |
| **Dummy RMSE** | 10.106      | 10.106       | Same as model — suggests low added value.        |

### ❌ MAPE Issues (Unreliable)

- **MAPE (Current)**: ~3.7e+17%
- **MAPE (Reference)**: ~3.5e+17%
- These values are **absurdly high** due to division by near-zero actual values.
- **Recommendation**: Avoid MAPE on delay datasets — use SMAPE or median absolute error instead.

---

## 🧠 Interpretation & Recommendations

- ✅ The simulated regression model shows **very high R²** and **stable performance**, indicating strong predictive potential.
- ⚖️ **Minimal change in metrics** between reference and current = **no significant performance drift**.
- ⚠️ Model performs **similarly to a dummy predictor**, meaning:
  - Feature engineering may need improvement.
  - Simulated prediction method (random noise) is not modeling actual structure — replace with a trained model for real evaluation.
- 🧹 **MAPE is unreliable** due to extreme values and should be excluded from future reports.

---

## 📁 Files

- `simulate_regression.py` – Regression setup and report generation script
- `flight_delay_regression_report.html` – Full Evidently regression report
- `archive/DelayedFlights.csv` – Source dataset

---

## ✅ Conclusion

This analysis demonstrates how Evidently can monitor the regression performance of an arrival delay prediction model. While the current setup simulates predictions, a production model could integrate this report for **live model health checks**, **drift detection**, and **early warning on prediction quality drops**.
