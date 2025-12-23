# Fraud Detection Pipeline — Transaction Analysis

## Ringkasan Proyek
Repositori ini berisi pipeline *end-to-end* untuk deteksi transaksi fraud menggunakan dataset **IEEE-CIS Fraud Detection** (Kaggle), mulai dari pengunduhan data, preprocessing, feature engineering, feature selection, penanganan class imbalance, training model, evaluasi, hingga pembuatan file submission. [web:2][page:1][file:1]  
Notebook utama menjalankan proses pada data `traintransaction.csv` dan `testtransaction.csv` yang diunduh via `gdown` dari Google Drive. [file:1]

## Dataset & Ruang Lingkup
Dataset kompetisi berisi 5 file CSV (termasuk `train_transaction.csv` dan `test_transaction.csv`) dengan total ukuran sekitar 1.35 GB pada halaman data kompetisi. [page:1]  
Pada notebook ini, data yang dipakai adalah transaksi dengan ukuran: train **590,540 baris × 394 kolom** dan test **506,691 baris × 393 kolom**. [file:1]  
Target `isFraud` pada train memiliki fraud rate sekitar **3.499%** (sangat imbalanced). [file:1]

### Ringkasan data (dari notebook)
| Split | Rows | Columns | Catatan |
|---|---:|---:|---|
| Train | 590,540 | 394 | Memuat `isFraud` dan `TransactionID`. [file:1] |
| Test | 506,691 | 393 | Tidak memuat target. [file:1] |

## Metodologi / Pipeline
Tahapan utama yang diimplementasikan pada notebook:

- **Setup & download data**: instal library (`gdown`, `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `imbalanced-learn`, `lightgbm`) lalu download `traintransaction.csv` dan `testtransaction.csv` dari Google Drive. [file:1]  
- **Pemisahan target & ID**: `isFraud` dipisah ke `ytrain`, `TransactionID` dipisah untuk kebutuhan submission, lalu kolom tersebut di-drop dari fitur. [file:1]  
- **Align kolom train vs test**: fitur disejajarkan menjadi **392 common columns** sebelum tahap lanjutan. [file:1]  
- **Missing value handling**: numerik diisi `-999`, kategorikal diisi string `"missing"`, dan diverifikasi tidak ada missing tersisa. [file:1]  
- **Feature engineering** (contoh fitur baru):
  - `TransactionAmtlog`, `TransactionAmtdecimal`, `TransactionAmtrounded` dari `TransactionAmt`. [file:1]
  - `Transactionhour`, `Transactionday`, `Transactiondayofweek`, `Transactionisweekend` dari `TransactionDT`. [file:1]
  - Fitur gabungan seperti `card1card2` dan `addr1addr2`. [file:1]
  - Flag null untuk domain email (`PemaildomainisNull`, `RemaildomainisNull`). [file:1]
- **Encoding kategorikal**: Label Encoding dilakukan konsisten dengan menggabungkan train+test per kolom; setelah ini seluruh fitur menjadi numerik. [file:1]  
- **Feature selection**: RandomForest dilatih pada sample 50,000 baris untuk menghitung feature importance, lalu dipilih **top 150 fitur** dari total fitur setelah rekayasa fitur. [file:1]  
- **Split & imbalance handling**: train/val split 80/20 (stratified), lalu SMOTE diterapkan **hanya pada train split** dengan `sampling_strategy=0.3`. [file:1]  
- **Model**: LightGBM binary classification dengan training hingga 1000 boosting rounds dan konfigurasi early stopping (50). [file:1]

## Hasil Eksperimen (Evaluasi)
Model LightGBM mencapai **Validation ROC-AUC = 0.9511** dan **Validation PR-AUC = 0.7601** pada split validasi notebook. [file:1]  
Pada threshold 0.5, confusion matrix yang tercetak adalah: TN=113,816; FP=159; FN=1,990; TP=2,143. [file:1]  
Notebook juga menampilkan 20 fitur teratas berdasarkan gain importance (contoh: `C12`, `V279`, `V294`, `C14`, `C8`). [file:1]

### Metrik utama (dari notebook)
| Metrik | Nilai |
|---|---:|
| Train ROC-AUC | 0.9968 [file:1] |
| Validation ROC-AUC | 0.9511 [file:1] |
| Validation PR-AUC | 0.7601 [file:1] |

## Cara Menjalankan & Output
### Menjalankan di lokal / Colab
1. Buka notebook: `Fraud_Detection_Pipeline_Transaction_Analysis.ipynb`. [file:1]  
2. Jalankan sel dari atas ke bawah (notebook sudah mencakup instalasi dependency, download data, training, evaluasi, dan prediksi). [file:1]

### Output yang dihasilkan
Notebook menghasilkan file `submission.csv` dengan kolom `TransactionID` dan probabilitas `isFraud`, berukuran **506,691 baris × 2 kolom**. [file:1]  
Statistik prediksi test yang tercetak: mean sekitar **0.036784** dan jumlah prediksi di atas 0.5 sebanyak **9,587** pada test set. [file:1]

---

**Catatan**: Dataset IEEE-CIS Fraud Detection tersedia melalui Kaggle (memerlukan akses/aturan kompetisi pada halaman data). [page:1]
