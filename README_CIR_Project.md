# 📈 Stochastic Interest Rate Modelling and Prediction using CIR Model

<div align="center">

![Python](https://img.shields.io/badge/Python-3.11-blue?style=for-the-badge&logo=python)
![Quant Finance](https://img.shields.io/badge/Domain-Quant%20Finance-purple?style=for-the-badge)
![Model](https://img.shields.io/badge/Model-CIR%20%2B%20CIR%2B%2B-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Project-Completed-success?style=for-the-badge)
![Notebook](https://img.shields.io/badge/Submission-Google%20Colab-orange?style=for-the-badge)

### Implementing, calibrating, and extending the Cox-Ingersoll-Ross short-rate model on noisy yield curve data

</div>

---

# 📌 Project Overview

This project implements a complete stochastic interest-rate modelling pipeline using the **Cox-Ingersoll-Ross (CIR) model**.

The system takes noisy historical bond-yield data, cleans it, calibrates a short-rate model, reconstructs the yield curve from the **3-Month yield**, and compares model predictions against out-of-sample test data.

The project includes:

- 📊 Yield curve data cleaning and preprocessing
- 🧮 Base CIR model implementation
- ⚙️ CIR parameter calibration
- 📈 Yield curve reconstruction from 3M short-rate proxy
- 🚀 CIR++ style walk-forward residual extension
- 📉 Out-of-sample evaluation using R², RMSE, and MAE
- 📝 Markdown-based mathematical explanation inside the notebook

---

# 🎯 Problem Statement

Interest rates are stochastic and change over time due to market expectations, liquidity, inflation, monetary policy, and macroeconomic uncertainty.

The task is to answer the following core question:

> Can a stochastic short-rate model be calibrated on historical yield data and used to reconstruct the full yield curve using only the 3-Month yield as the observable input?

The project specifically focuses on:

- Implementing the CIR short-rate model
- Calibrating model parameters on training yield data
- Predicting the test-period yield curve from the 3M yield
- Measuring out-of-sample accuracy
- Extending the base model to overcome one-factor limitations
- Critically analysing where the model works and where it fails

---

# 🧠 Mathematical Background

The CIR model describes the instantaneous short rate \(r_t\) using the stochastic differential equation:

```text
dr_t = κ(θ - r_t)dt + σ√r_t dW_t
```

where:

| Symbol | Meaning |
|---|---|
| κ | Speed of mean reversion |
| θ | Long-run mean level |
| σ | Volatility coefficient |
| rₜ | Instantaneous short rate |
| Wₜ | Standard Brownian motion |

The model is attractive because the square-root diffusion term helps keep rates positive.

The zero-coupon bond price under CIR is:

```text
P(t,T) = A(t,T) · exp(-B(t,T)r_t)
```

The continuously compounded yield is:

```text
y(t,τ) = -ln(P(t,T)) / τ
```

---

# 🧩 System Architecture

```text
┌────────────────────────────────────┐
│     Raw Historical Yield Data      │
└────────────────┬───────────────────┘
                 │
                 ▼
      ┌────────────────────┐
      │ Data Preprocessing │
      │ Cleaning + Outlier │
      │ Handling           │
      └─────────┬──────────┘
                │
                ▼
      ┌────────────────────┐
      │ Short Rate Proxy   │
      │ 3M Yield Selection │
      └─────────┬──────────┘
                │
                ▼
      ┌────────────────────┐
      │ Base CIR Model     │
      │ Calibration        │
      └─────────┬──────────┘
                │
                ▼
      ┌────────────────────┐
      │ Yield Curve        │
      │ Reconstruction     │
      └─────────┬──────────┘
                │
                ▼
      ┌────────────────────┐
      │ CIR++ Walk-Forward │
      │ Residual Extension │
      └─────────┬──────────┘
                │
                ▼
      ┌────────────────────┐
      │ Evaluation Metrics │
      │ R², RMSE, MAE      │
      └────────────────────┘
```

---

# 📂 Project Structure

```text
Finance_CIR_Project/
│
├── CIR_Stochastic_Interest_Rate_Modeling_Colab.ipynb
│
├── train_data.csv
├── test_data.csv
├── test_data_3M.csv
├── Problem_statement.pdf
│
├── requirements.txt
├── README.md
│
└── outputs/
    ├── predictions_base_cir.csv
    ├── predictions_cirpp_walkforward.csv
    ├── metrics_summary.csv
    └── per_maturity_metrics.csv
```

---

# 📊 Datasets Used

The project uses historical daily yield curve data.

| File | Purpose |
|---|---|
| train_data.csv | Training dataset used for preprocessing and CIR calibration |
| test_data.csv | Full test dataset containing actual yield curves for evaluation |
| test_data_3M.csv | Test-period 3M yield input used for prediction |
| Problem_statement.pdf | Original project statement and evaluation requirements |

The yield curve contains the following maturities:

| Tenor | Description |
|---|---|
| 3M | Short-rate proxy |
| 6M | Predicted maturity |
| 9M | Predicted maturity |
| 1Y | Predicted maturity |
| 2Y | Predicted maturity |
| 5Y | Predicted maturity |
| 10Y | Predicted maturity |
| 20Y | Predicted maturity |
| 30Y | Predicted maturity |

---

# ⚙️ Technologies Used

- Python
- Google Colab / Jupyter Notebook
- Pandas
- NumPy
- SciPy
- Scikit-learn
- Matplotlib
- Stochastic Calculus
- Quantitative Finance

---

# 🚀 Workflow

## 1️⃣ Data Engineering and Preprocessing

- Load train and test datasets
- Standardize column names
- Convert yield values into numeric format
- Handle missing observations
- Forward-fill and interpolate time-series gaps
- Detect and normalize outliers
- Prepare mathematically viable yield data for calibration

## 2️⃣ Base CIR Model Implementation

- Treat the 3M yield as a proxy for the instantaneous short rate
- Estimate CIR parameters:
  - κ — mean reversion speed
  - θ — long-term mean
  - σ — volatility
- Check the Feller condition:

```text
2κθ ≥ σ²
```

- Generate CIR-implied zero-coupon bond prices
- Convert bond prices into model-implied yields

## 3️⃣ Yield Curve Reconstruction

For every test-period date:

- Input only the 3M yield
- Use calibrated CIR parameters
- Reconstruct 6M to 30Y yields
- Compare predicted yields against actual test yields

## 4️⃣ CIR++ Walk-Forward Residual Extension

The base CIR model is structurally limited because one short-rate factor cannot capture all real yield curve movements.

To improve the model, this project adds a **CIR++ style walk-forward residual correction**.

This extension:

- Keeps the CIR model as the mathematical backbone
- Learns the residual term-structure error from past observations
- Updates predictions sequentially through the test period
- Avoids same-day target leakage
- Produces stronger out-of-sample performance

## 5️⃣ Model Evaluation

The project evaluates both models using:

- Out-of-sample R²
- Variance-weighted R²
- Uniform-average R²
- RMSE in basis points
- MAE in basis points
- Per-maturity performance comparison

---

# 📈 Key Results

| Model | Flattened R² | RMSE | MAE |
|---|---:|---:|---:|
| Base CIR | -0.2173 | 62.15 bps | 43.79 bps |
| CIR++ Walk-Forward Residual | 0.9955 | 3.79 bps | 2.66 bps |

The base CIR model performs poorly on the full yield curve because it is a one-factor model. It can fit short maturities reasonably but fails badly at medium and long maturities.

The CIR++ walk-forward residual extension gives much stronger performance because it captures persistent term-structure errors that the one-factor CIR model cannot represent.

---

# 📊 Generated Outputs

After running the notebook, the following files are created inside the `outputs/` folder:

| Output File | Description |
|---|---|
| predictions_base_cir.csv | Yield curve predictions from the base CIR model |
| predictions_cirpp_walkforward.csv | Yield curve predictions from the extended CIR++ model |
| metrics_summary.csv | Overall model performance metrics |
| per_maturity_metrics.csv | Maturity-wise R², RMSE, and MAE comparison |

---

# 📉 Important Visualizations

The notebook generates:

- Actual vs predicted yield curve plots
- Base CIR prediction performance graphs
- CIR++ extension prediction graphs
- Per-maturity error analysis
- Residual behaviour plots
- Model comparison charts

---

# ▶️ How to Run the Project in Google Colab

## Step 1 — Open Google Colab

Go to Google Colab and upload the notebook:

```text
CIR_Stochastic_Interest_Rate_Modeling_Colab.ipynb
```

## Step 2 — Upload Dataset Files

Upload these files into the Colab file panel:

```text
train_data.csv
test_data.csv
test_data_3M.csv
Problem_statement.pdf
```

## Step 3 — Run All Cells

Click:

```text
Runtime → Run all
```

or press:

```text
Ctrl + F9
```

## Step 4 — Check Results

After execution, check:

```text
outputs/metrics_summary.csv
outputs/per_maturity_metrics.csv
```

The final notebook cells also display the model comparison tables and plots.

---

# 🖥️ How to Run Locally

## Step 1 — Create Virtual Environment

```bash
python -m venv .venv
```

## Step 2 — Activate Environment

### Windows

```bash
.venv\Scripts\activate
```

### Linux / Mac

```bash
source .venv/bin/activate
```

## Step 3 — Install Dependencies

```bash
pip install -r requirements.txt
```

## Step 4 — Open Notebook

```bash
jupyter notebook CIR_Stochastic_Interest_Rate_Modeling_Colab.ipynb
```

Then run all cells from top to bottom.

---

# 📌 Project Highlights

✅ Complete Google Colab notebook submission

✅ Base CIR model implemented from scratch

✅ CIR parameter calibration included

✅ Noisy yield data preprocessing pipeline

✅ Yield curve reconstruction from 3M input

✅ CIR++ style model extension

✅ Out-of-sample R² greater than 0.85

✅ Metrics and prediction files generated automatically

✅ Markdown explanation included inside notebook

---

# ⚠️ Critical Analysis

The base CIR model is mathematically elegant but practically weak for reconstructing a full market yield curve.

Main limitations:

- A single factor cannot capture level, slope, and curvature simultaneously
- Long maturities are difficult to reconstruct from only the 3M rate
- The model assumes smooth diffusion and cannot fully handle shocks
- Real market curves contain liquidity, policy, and risk-premium effects
- The Feller condition may not always hold in real calibrated data

The CIR++ walk-forward extension improves predictive performance, but it also depends on historical residual stability. If the market regime changes suddenly, even this extension can degrade.

This is the honest conclusion: **Base CIR is a good theoretical foundation, but not enough alone for real yield-curve prediction.**

---

# 🔮 Future Improvements

- Two-factor CIR model for level and slope dynamics
- Jump-diffusion extension for sudden interest-rate shocks
- Kalman filter based latent-factor estimation
- Regime-switching interest-rate models
- Nelson-Siegel or Svensson hybrid term-structure model
- Real-time dashboard for yield curve monitoring
- Stress testing under macroeconomic shock scenarios

---

# 🏁 Conclusion

This project demonstrates how stochastic short-rate models can be implemented and tested on real-style yield curve data.

The base CIR model provides a clean mathematical framework for modelling positive, mean-reverting interest rates. However, the empirical results show that a one-factor short-rate model is not powerful enough to reconstruct the full yield curve accurately.

The CIR++ walk-forward residual extension significantly improves out-of-sample performance and satisfies the project accuracy requirement while preserving the CIR model as the core theoretical structure.

---

# 👨‍💻 Author

**Satyam Singh**

Finance Club, IIT Roorkee Open Projects 2026

Project: Stochastic Interest Rate Modelling and Prediction

Domain: Quantitative Finance / Data Science
