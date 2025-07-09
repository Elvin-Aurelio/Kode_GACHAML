## 🚗 Prediksi Penjualan Mobil Mingguan dengan SARIMAX + Google Trends

Proyek ini menggunakan model **SARIMAX** untuk memprediksi jumlah unit mobil yang terjual setiap minggu. Model memanfaatkan **fitur eksternal (exogenous)** berupa data Google Trends dengan preprocessing dan **normalisasi** data.

Model dan scaler disimpan dalam bentuk `.pkl` agar bisa digunakan kembali oleh pengguna lain (misal untuk deployment via Hugging Face atau Streamlit).

---

## 🔧 Fitur

* Model time series berbasis **SARIMAX (Seasonal ARIMA with Exogenous Regressors)**.
* Eksternal feature dari Google Trends dengan keyword otomotif.
* **Normalisasi** pada data target (`units_sold`) dan fitur (`trend_lag_2`, `trend_lag_3`).
* **Cross-validation khusus time series (TSCV)**.
* Evaluasi model dengan **MAE**, **sMAPE**, dan **LogRMSE**.
* Model dan scaler disimpan untuk inference di masa depan.

---

## 🗂 File

| File                       | Deskripsi                                                                      |
| -------------------------- | ------------------------------------------------------------------------------ |
| `sarimax_scaled_model.pkl` | Model SARIMAX terlatih pada data penjualan mobil + trend (sudah dinormalisasi) |
| `sarima_trend_model.pkl`   | Model SARIMA untuk memprediksi Google Trends mingguan ke depan                 |
| `scaler_y.pkl`             | Scaler untuk menormalisasi/denormalisasi `units_sold`                          |
| `scaler_X.pkl`             | Scaler untuk `trend_lag_2`, `trend_lag_3`                                      |
| `scaler_trend.pkl`         | Scaler khusus untuk data Google Trends (avg\_trend)                            |

---

## 🧪 Cara Penggunaan

### 1. Load Model dan Scaler

```python
import joblib
import pandas as pd

# Load model dan scaler
sarimax_model = joblib.load("sarimax_scaled_model.pkl")
scaler_y = joblib.load("scaler_y.pkl")
scaler_X = joblib.load("scaler_X.pkl")
```

### 2. Siapkan Data Baru

```python
# Contoh data mingguan baru (pastikan kolom sama dan index bertipe datetime)
new_data = pd.DataFrame({
    'trend_lag_2': [...],
    'trend_lag_3': [...]
}, index=pd.date_range(start='2025-07-01', periods=10, freq='W'))

# Normalisasi
X_new_scaled = scaler_X.transform(new_data)
```

### 3. Forecast

```python
forecast_scaled = sarimax_model.forecast(steps=10, exog=X_new_scaled)

# Denormalisasi hasil forecast
forecast_final = scaler_y.inverse_transform(forecast_scaled.reshape(-1, 1)).flatten()
```

---

## 📊 Evaluasi

| Metrik  | Nilai |
| ------- | -------------- |
| MAE     | 55.1           |
| sMAPE   | 15.6%          |
| LogRMSE | 0.18           |

> Evaluasi dilakukan pada data asli (setelah denormalisasi) agar hasilnya bisa ditafsirkan secara bisnis.

---

## 📦 Requirements

* Python 3.8+
* `pandas`
* `numpy`
* `matplotlib`
* `scikit-learn`
* `statsmodels`
* `joblib`

Install dengan:

```bash
pip install pandas numpy matplotlib scikit-learn statsmodels joblib
```

---

## 🧠 Catatan Tambahan

* Model SARIMAX ini **tidak cocok untuk prediksi jangka panjang** (>10 minggu) **jika tidak disertai prediksi fitur eksternal (Google Trends)**.
* Untuk prediksi jangka panjang, gunakan juga model `sarima_trend_model.pkl` untuk memprediksi `trend_lag_2`, `trend_lag_3` terlebih dahulu.
* Gunakan normalisasi yang **sama seperti saat training**. Jangan `fit` ulang scaler pada data baru.
