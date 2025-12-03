# Vehicle Insurance CLV Prediction

## 1. Latar Belakang
Industri asuransi menghadapi tantangan besar dalam mempertahankan pelanggan dan meningkatkan profitabilitas. Biaya untuk mendapatkan customer baru terus meningkat, sedangkan beberapa customer memiliki masa langganan yang relatif rendah. Profitabilitas jangka panjang perusahaan sangat bergantung pada **Customer Lifetime Value (CLV)**, yaitu nilai total keuntungan yang diberikan customer kepada perusahaan.

---

## 2. Pernyataan Masalah & Tujuan

**Pernyataan Masalah**

Perusahaan memiliki kesulitan untuk menentukan customer yang memberikan kentungan terbesar dan strategi retensi yang paling efektif. Sehingga, dapat berdampak kepada kehilangan customer high value, sedangkan penghabisan anggaran pada customer bernilai rendah. 

**Tujuan**
1.  Membangun model Machine Learning (Regresi) untuk memprediksi nilai CLV berdasarkan demografi dan perilaku polis.
2.  Mengidentifikasi faktor-faktor terpenting (Feature Importance) yang mempengaruhi nilai pelanggan untuk merumuskan strategi *cross-selling* dan retensi yang efektif.

---

## 3. Alur Kerja (*Workflow*)

Proses analisis data dilakukan secara terstruktur melalui tahapan berikut:

1.  **Data Cleaning**
    * Membersihkan data dari duplikasi dan memastikan data (*missing values*).
    * Menganalisis distribusi target variabel (CLV) yang memiliki kecenderungan *right-skewed*.

2.  **Preprocessing:**
    * **Scaling:** Menstandarisasi fitur numerik agar memiliki rentang nilai yang sama.
    * **Encoding :** Mengonversi data kategorikal menjadi format numerik agar dapat diproses oleh algoritma.

3.  **Pemilihan Model:**
    * Menguji 5 algoritma berbeda tanpa mengubah settingan apapun (*default*).
    * Menggunakan **Cross-Validation (5-Fold)** untuk memastikan hasil tesnya stabil 

4.  **Hyperparameter Tuning :**
    * Memilih model potensial dan mencari kombinasi parameter untuk meningkatkan akurasi prediksi.

5.  **Evaluasi & Interpretasi:**
    * Mengecek error model menggunakan grafik Residual dan melihat fitur apa yang paling berpengaruh.

---

## 4. Pemilihan Model

Dalam tahap eksperimen, terjadi perubahan peringkat performa model antara tahap awal dan setelah optimasi (*tuning*).

### Tahap Awal
Saat pertama kali dijalankan dengan settingan default, **Gradient Boosting** memiliki rata-rata error paling kecil.

| Peringkat | Model | Rata-rata Skor CV (RMSE) |
|-----------|-------|--------------------------|
| 1 | **Gradient Boosting** | **1.631.560** |
| 2 | Random Forest | 1.844.041 |
| 3 | XGBoost | 2.110.064 |
| 4 | Linear Regression | 4.050.335 |
| 5 | KNN | 4.485.082 |

### Hyperparameter Tuning
Meskipun pada awalnya berada di peringkat ketiga, **XGBoost** dipilih untuk dioptimasi lebih lanjut.

* **Model Akhir:** XGBoost (Tuned)
* **RMSE pada Data Uji:** ~3907 (Semakin rendah menunjukkan akurasi yang semakin baik).

---

## Pemilihan Model XGBoost

Meskipun Gradient Boosting dan Random Forest memiliki nilai rata-rata CV RMSE terendah pada tahap awal, **XGBoost** dipilih sebagai kandidat utama untuk optimasi. Berdasarkan Chen & Guestrin (2016), XGBoost banyak digunakan pada kompetisi data science di Kaggle karena kemampuannya manangani data sparse dan memiliki mekanisme regularisasi*. 

Data CLV yang dipakai sebagai resource memiliki outlier yang dapat cukup mempengaruhi model. Oleh karena itu, tahap *hyperparameter tuning* dilakukan pada Gradient Boosting dan XGBoost untuk membandingkan performa optimal keduanya.  

> **Referensi: Chen, T., & Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System.*

---

## 5. Evaluasi Model

Kualitas model final dievaluasi menggunakan dua pendekatan utama:

### Grafik Residual 
Grafik selisih antara nilai prediksi dan nilai aktual paling tinggi terpusat di angka 0. Hal ini menunjukkan sebagian besar tebakannya sangat dekat dengan nilai asli. Sehingga, model memiliki validitas yang baik.

### Feature Importance
Model mengidentifikasi variabel yang paling mempengaruhi nilai CLV:
1.  **Number of Policies (Jumlah Polis):** Faktor paling dominan dengan kontribusi pengaruh mencapai **48%**.
2.  **Monthly Premium Auto:** Besaran premi bulanan menempati posisi kedua terpenting.

---

## 6. Rekomendasi Bisnis

Berdasarkan analisis data, berikut adalah rekomendasi strategis bagi perusahaan:

1.  **Optimasi Feature Importance untuk Strategi Cross-Selling**
    Berdasarkan *Feature Importance* dari model *XGBoost*, variabel jumlah polis menjadi faktor yang sangat mempengaruhi CLV. Tim marketing dapat fokus menawarkan produk tambahan (asuransi properti, jiwa, dll) kepada pelanggan yang hanya memiliki 1 polis kendaraan. Berikan insentif seperti diskon khusus misal pembelian bundling

2.  **Segmentasi Layanan:**
    Dengan menggunakan prediksi model, kita dapat mengetahui customer yang memiliki nilai CLV yang tinggi. Sehingga, kita dapat memberikan *priority support* saat klaim untuk menjaga loyalitas customer 

3.  **Edukasi Pelanggan:**
    Untuk segmen dengan Total Claim Amount tinggi namun CLV rendah, diberikan program edukasi dan pelatihan berkendara aman. Jika masih sering klaim, maka pertimbangkan harga premi pada saat renewal.

---
*Dibuat oleh [Asma NU]*
