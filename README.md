# 📈 Bagian 1: Prediksi Penjualan Mobil Mingguan dengan SARIMA

Repositori ini berisi eksperimen dan pengembangan model prediksi penjualan mobil mingguan menggunakan pendekatan time series klasik: **SARIMA**. Beberapa pendekatan alternatif seperti penggunaan fitur eksternal dari Google Trends dan hybrid model sempat dieksplorasi, namun **tidak memberikan peningkatan signifikan**, sehingga **model final tetap menggunakan SARIMA murni**.

## 📂 Struktur Folder

```
.
├── sarima_model.pkl       # Model SARIMA final yang sudah dilatih
├── predict.py             # Script untuk melakukan
├── datathon_model.ipynb   # Notebook utama yang berisi seluruh eksperimen
└── README.md              # Dokumentasi proyek
```

## ⚙️ Fitur Proyek

* Prediksi penjualan mobil per minggu berdasarkan data historis
* Evaluasi menggunakan sMAPE, MAE, log-RMSE, dan R²
* Implementasi Time Series Cross-Validation (TSCV)
* Eksperimen tambahan:

  * SARIMAX (dengan Google Trends) — *tidak dipakai di model final*
  * Hybrid SARIMA + Polynomial Regression — *hanya eksplorasi*

## 🧪 Dataset

Dataset utama: `car_data.csv`

* Fitur utama: Tanggal dan Car ID
* Penjualan dihitung per minggu

## 📊 Hasil Utama

| Model                          | Mean sMAPE  |
| ------------------------------ | ----------- |
| **SARIMA (FINAL)**             | **14.8%** |
| SARIMAX + Google Trends        | 15.2%     |
| SARIMA + Polynomial Regression | 19.4%    |

Model SARIMA dipilih karena memberikan hasil yang kompetitif dan stabil tanpa membutuhkan data eksternal tambahan.

## 🧠 Cara Pakai

### 1. Instalasi Dependensi

```bash
pip install pandas numpy matplotlib seaborn joblib statsmodels scikit-learn
```

### 2. Jalankan Prediksi

Pastikan file `sarima_model.pkl` ada di folder yang sama dengan `predict.py`, lalu jalankan:

```bash
python predict.py
```

Output akan berupa prediksi penjualan mobil 10 minggu ke depan.

## 📈 Visualisasi

Model telah divisualisasikan lengkap dalam `datathon_model.ipynb`, termasuk evaluasi per fold dan perbandingan antar pendekatan.

## ❌ Kenapa Tidak Pakai Google Trends?

Eksperimen menggunakan data eksternal (Google Trends) sempat dilakukan dengan pendekatan SARIMAX dan normalisasi, namun:

* Model tidak menunjukkan perbaikan signifikan
* Menambah kompleksitas pipeline

Akhirnya model SARIMA standar dipilih sebagai solusi terbaik.


---

## 🧠 Bagian 2: Analisis Distribusi Penjualan

Notebook `datathon_analysis.ipynb` berisi analisis spasial dari penjualan mobil untuk menjawab pertanyaan seperti:

* **Wilayah mana yang paling banyak menyerap stok mobil?**
* **Jenis mobil apa yang paling banyak diminati di tiap region?**
* **Perlu tidaknya redistribusi stok antar dealer/daerah?**

### 📍 Contoh Insight:

* Model **Prizm** paling laku di Austin (96 unit) dan Janesville (81 unit) Ini jadi sinyal kuat bahwa Prizm cocok diprioritaskan di dua wilayah itu.
* Model **Diamante** perlu perhatian khusus pada warna Merah karena proporsinya cukup besar.
* **Distribusi stok** perlu disesuaikan berdasarkan tren lokal dan historis penjualan

Insight semacam ini bisa digunakan untuk menyarankan:

* Optimasi logistik dan pengiriman
* Strategi pemasaran lokal
* Perencanaan pengadaan stok ke depannya

---

```bash
pip install pandas numpy matplotlib seaborn joblib statsmodels scikit-learn gender-guesser pytrends
```

