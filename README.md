# 📈 PPP Analysis & ARIMA Exchange Rate Forecasting — GBP/JPY (2010–2020)

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Statsmodels](https://img.shields.io/badge/Statsmodels-0.14-orange)
![ARIMA](https://img.shields.io/badge/ARIMA-Time%20Series-purple)
![UCC](https://img.shields.io/badge/UCC-MSc%20Business%20Analytics-darkgreen)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> **Skills:** Python · Statsmodels · ARIMA · Time Series Analysis · ADF/KPSS Testing · Engle-Granger Cointegration · Matplotlib · Pandas  
> **Module:** EC6011 Business Forecasting — University College Cork (UCC)  
> **Dataset:** 132 monthly observations · GBP/JPY · Jan 2010 – Dec 2020

---

## 📌 Project Overview

This project analyses the bilateral **GBP/JPY exchange rate** across a decade spanning the Bank of Japan's QQE expansion, the 2016 Brexit referendum, and the COVID-19 shock — pursuing two goals:

1. Test whether **Purchasing Power Parity (PPP)** holds using formal unit-root diagnostics and Engle-Granger cointegration
2. Construct an optimal **Box-Jenkins ARIMA** model for 12-month out-of-sample forecasting

**Central finding:** GBP/JPY behaved as a near-random walk. Both Absolute and Relative PPP are rejected. ARIMA(0,1,0) is selected — validating the **Meese-Rogoff (1983) puzzle** in a contemporary out-of-sample setting.

---

## 🗂️ Repository Structure & How Files Connect

```
📁 PPP-ARIMA-Exchange-Rate-Forecasting/
│
├── 📓 PPP_ARIMA_Analysis.ipynb         ← MAIN FILE: Full analysis pipeline
│   │   Reads:   Bank_of_England_XUDLJYS.csv + GBRCPIALLMINMEI.csv + JPNCPIALLMINMEI.csv
│   │   Outputs: clean_ppp_dataset.csv + fig1–fig7 PNGs
│
├── 📊 clean_ppp_dataset.csv            ← MERGED DATASET (132 obs, 10 variables)
│   │   Built from: BOE daily → monthly avg + UK CPI + JP CPI
│
├── 📂 Bank_of_England_XUDLJYS.csv      ← RAW: JPY/GBP daily spot rates (BOE)
├── 📂 GBRCPIALLMINMEI.csv              ← RAW: UK CPI monthly (FRED/OECD)
├── 📂 JPNCPIALLMINMEI.csv              ← RAW: Japan CPI monthly (FRED/OECD)
├── 📦 requirements.txt                 ← Python dependencies
└── 📜 LICENSE                          ← MIT License
```

### 🔗 Pipeline Flow

```
Raw Data Sources
  BOE XUDLJYS (daily) + UK CPI (monthly) + JP CPI (monthly)
        │
        ▼
[1] Data Cleaning & Monthly Aggregation    ← Section 2
        │  BOE daily → monthly mean
        ▼
[2] Variable Construction                  ← Section 3
        │  log_nominal, log_real, price_diff, real_rate
        ▼
[3] Descriptive & Visual Analysis          ← Sections 4–5
        │  Time series plots, PPP gap
        ▼
[4] Stationarity Diagnostics               ← Section 6–7
        │  ADF + KPSS + ACF/PACF → I(1) confirmed
        ▼
[5] PPP Testing                            ← Sections 8–9
        │  Engle-Granger (Absolute) + OLS (Relative)
        ▼
[6] ARIMA Model Selection                  ← Section 10
        │  8 candidates → ARIMA(0,1,0) by BIC
        ▼
[7] Residual Diagnostics                   ← Section 11
        │  Ljung-Box + ARCH-LM → white noise confirmed
        ▼
[8] 12-Month Forecast + Meese-Rogoff       ← Sections 12–13
```

---

## 📊 Dataset — `clean_ppp_dataset.csv`

| Variable | Description |
|---|---|
| Date | Monthly date (Jan 2010 – Dec 2020) |
| CPI_UK | UK Consumer Price Index (OECD, 2015=100) |
| CPI_JP | Japan Consumer Price Index (OECD, 2015=100) |
| Nominal_Rate | Monthly average JPY/GBP spot rate (BOE XUDLJYS) |
| log_nominal | log(Nominal_Rate) |
| log_cpi_uk | log(CPI_UK) |
| log_cpi_jp | log(CPI_JP) |
| real_rate | Nominal_Rate × CPI_JP / CPI_UK |
| log_real | log(real_rate) |
| price_diff | log_cpi_uk − log_cpi_jp |

> **Data Sources:** Bank of England Statistical Interactive Database (XUDLJYS) · FRED/OECD (GBRCPIALLMINMEI, JPNCPIALLMINMEI)

---

## 📉 Key Results

### Stationarity Tests
| Series | ADF p-value | KPSS | Conclusion |
|---|---|---|---|
| log_real (levels) | > 0.05 | > critical | I(1) — non-stationary |
| d_log_real (diff) | < 0.001 | < critical | I(0) — stationary |

### PPP Tests
| Test | Result |
|---|---|
| Engle-Granger ADF on residuals | p = 0.61 → **No cointegration → Absolute PPP rejected** |
| Relative PPP slope | β ≈ −1.42, p > 0.05, R² ≈ 0.01 → **Relative PPP rejected** |

### ARIMA Model Selection (8 candidates)
| Model | AIC | BIC | Selected |
|---|---|---|---|
| ARIMA(0,1,0) | −508.08 | **−505.30** | ✅ BIC |
| ARIMA(0,1,1) | **−508.25** | −502.69 | AIC |
| ARIMA(1,1,0) | −508.10 | −502.54 | |

### Forecast Accuracy (12-month holdout)
| Model | RMSE | MAE |
|---|---|---|
| ARIMA(0,1,0) | 0.042663 | 0.034570 |
| Naive Random Walk | 0.042663 | 0.034570 |

**Meese-Rogoff puzzle confirmed** — ARIMA(0,1,0) IS the random walk.

---

## 📊 Visualisations — 7 Charts

| Fig | Chart |
|---|---|
| fig1 | Log nominal + real rate + UK/JP CPI (4-panel) |
| fig2 | PPP gap over time with Brexit/COVID markers |
| fig3 | ACF & PACF — levels and first differences |
| fig4 | Absolute PPP regression + residuals |
| fig5 | Residual ACF + Normal Q-Q plot |
| fig6 | ARIMA(0,1,0) 12-month forecast with 95% CI |
| fig7 | 4-panel executive summary dashboard |

---

## 🚀 How to Run

```bash
git clone https://github.com/your-username/PPP-ARIMA-Exchange-Rate-Forecasting.git
cd PPP-ARIMA-Exchange-Rate-Forecasting
pip install -r requirements.txt
jupyter notebook PPP_ARIMA_Analysis.ipynb
```

---

## 📁 Related Projects

| # | Project | Skills |
|---|---|---|
| 5 | [Global Supply Chain Optimisation](../Global-Supply-Chain-Optimisation) | Python · MILP · Monte Carlo |
| 6 | [E-Commerce Funnel Analysis](../E-Commerce-Conversion-Funnel-Analysis) | Python · Plotly · Seaborn |
| **7** | **PPP & ARIMA Forecasting ← You are here** | **Python · Statsmodels · ARIMA** |
| 8 | [Signature Detection](../Signature-Detection-Verification) | PyTorch · CNN · OpenCV |

---

## 👩‍💻 Author

**Khushi Dhargawe**  
MSc Business Analytics — University College Cork (UCC)  
BE Artificial Intelligence & Machine Learning (Hons. Cybersecurity) — Mumbai University

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/khushi-dhargawe/)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black?logo=github)](https://github.com/Khushi-Dhargawe)

---

## 📜 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
