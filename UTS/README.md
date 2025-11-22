Faris Eza Purwono Adi
1103223154

# 🚀 Koleksi Pipeline Machine Learning (ML) End-to-End

Proyek ini adalah repositori yang berisi implementasi komprehensif **End-to-End (E2E)** dari tiga kasus penggunaan utama dalam Machine Learning (ML). Setiap *notebook* Jupyter (`.ipynb`) merupakan *pipeline* mandiri yang memandu pengguna melalui seluruh siklus hidup proyek ML: mulai dari akuisisi data, pembersihan, pra-pemrosesan, rekayasa fitur, pelatihan model, hingga evaluasi hasil akhir.

## 🎯 Isi dan Tugas Utama

Tabel berikut merangkum setiap *pipeline* yang ada dalam repositori ini:

| Nama File | Judul | Tugas ML | Deskripsi Singkat |
| :--- | :--- | :--- | :--- |
| `E2E_Clustering.ipynb` | **End-to-End Customer Clustering Pipeline** | *Unsupervised Learning* (Klustering) | Digunakan untuk segmentasi pelanggan ke dalam kelompok-kelompok homogen berdasarkan pola perilaku, misalnya, dalam data penggunaan kartu kredit. |
| `E2E_Fraud_Detection.ipynb` | **End-to-End Transaction Fraud Detection Pipeline** | *Supervised Learning* (Klasifikasi Biner) | Membangun model yang efektif untuk mengidentifikasi dan memprediksi transaksi penipuan (*fraud*) dalam set data yang tidak seimbang (*imbalanced*). |
| `E2E_Song_Release_per_Year.ipynb` | **End-to-End Regression Pipeline for Song Release Year Prediction** | *Supervised Learning* (Regresi) | Bertujuan memprediksi nilai numerik (Tahun Rilis Lagu) berdasarkan fitur-fitur audio, membandingkan performa berbagai model Regresi. |

## ⚙️ Metode dan Pendekatan yang Digunakan

Setiap *pipeline* mengikuti metodologi standar Data Science dan ML, dengan penekanan pada langkah-langkah kritis tertentu:

### 1. Pra-pemrosesan Data (Data Preprocessing)

* **Pembersihan Data:** Penanganan nilai yang hilang (*missing values*) melalui teknik imputasi yang sesuai.
* **Penskalaan (Scaling):** Fitur numerik distandarisasi menggunakan **StandardScaler** untuk memastikan semua fitur memiliki kontribusi yang setara, krusial untuk algoritma berbasis jarak seperti K-Means.

### 2. Algoritma dan Teknik Utama

| Tugas | Algoritma dan Teknik Kunci | Metrik Evaluasi |
| :--- | :--- | :--- |
| **Klustering** | **K-Means Clustering**; Penentuan K optimal melalui *Elbow Method* atau *Silhouette Score*. | Interpretasi Kluster dan Visualisasi Sebaran Kluster. |
| **Klasifikasi (Fraud)** | Model Klasifikasi Kuat (e.g., **XGBoost** atau **Random Forest**); Penanganan **Imbalance Data** (misalnya, *Oversampling* dengan SMOTE). | **AUC-ROC**, *Precision*, *Recall*, *F1-Score*. |
| **Regresi (Prediksi Tahun)** | **Ridge**, **Lasso**, **Random Forest Regressor**; Perbandingan model untuk menemukan yang paling akurat. | **RMSE** (*Root Mean Square Error*), **MAE** (*Mean Absolute Error*), dan $\mathbf{R^2}$. |

---

## 🛠️ Persyaratan dan Instalasi

Untuk menjalankan *notebook* ini, Anda memerlukan lingkungan Python yang sesuai.

### Persyaratan Lingkungan

* **Python:** Versi 3.7+
* **Pustaka Utama:** `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`.
* **Pustaka Tambahan:** Pustaka spesifik ML seperti `xgboost` dan alat penanganan data seperti `gdown` (jika data diunduh dari Google Drive).

### Instalasi Dependensi

Semua pustaka yang diperlukan dapat diinstal menggunakan `pip`:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn xgboost gdown