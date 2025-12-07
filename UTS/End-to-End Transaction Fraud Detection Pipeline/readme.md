# End-to-End Transaction Fraud Detection Pipeline

## 📖 Deskripsi Proyek

Repository ini berisi implementasi lengkap pipeline machine learning untuk mendeteksi transaksi fraud menggunakan dataset transaksi keuangan. Proyek ini merupakan bagian dari UTS Machine Learning dengan pendekatan end-to-end yang mencakup data loading dengan memory optimization, exploratory data analysis (EDA), preprocessing, training model LightGBM, dan evaluasi performa menggunakan metrik klasifikasi.

## 🎯 Tujuan

Membangun model klasifikasi binary yang dapat mendeteksi transaksi fraud dengan akurasi tinggi menggunakan algoritma LightGBM, dengan fokus pada optimasi memory dan performa model.

## 📊 Dataset

### Train Dataset
- **Ukuran Dataset**: 590,540 baris × 125 kolom
- **Ukuran File**: ~683 MB
- **Memory Usage**: 563.2 MB → 169.5 MB (69.9% reduction)
- **Fraud Rate**: 3.50%

### Test Dataset
- **Ukuran Dataset**: 506,691 baris × 124 kolom
- **Ukuran File**: ~613 MB
- **Memory Usage**: 479.4 MB → 146.9 MB (69.4% reduction)

### Fitur Dataset
- **TransactionID**: ID unik transaksi
- **isFraud**: Target variable (0 = Normal, 1 = Fraud)
- **TransactionDT**: Delta waktu dari tanggal referensi
- **TransactionAmt**: Jumlah transaksi
- **ProductCD**: Kode produk
- **card1-card6**: Informasi kartu
- **addr1, addr2**: Informasi alamat
- **dist1**: Jarak
- **P_emaildomain, R_emaildomain**: Domain email
- **C1-C14**: Counting features
- **D1-D15**: Timedelta features
- **V1-V339**: Vesta engineered features

## 🔧 Teknologi & Library

### Core Libraries
```python
- numpy
- pandas
- gdown (untuk download dataset)
- gc (garbage collection)
```

### Machine Learning
```python
- scikit-learn
  - train_test_split
  - LabelEncoder
  - roc_auc_score
  - classification_report
  - confusion_matrix
- lightgbm (LightGBM Classifier)
```

### Visualization
```python
- matplotlib
```

## 🚀 Fitur Utama

### 1. **Memory Optimization**
   - Custom function `reduce_mem_usage()` untuk optimasi memory
   - Konversi dtype otomatis (int8, int16, int32, float16, float32)
   - Pengurangan memory usage hingga 70%
   - Smart column selection (hanya memuat fitur penting)

### 2. **Smart Feature Selection**
   - Membuang kolom dengan missing values >50%
   - Fokus pada fitur-fitur penting:
     - Transaction features (DT, Amount, ProductCD)
     - Card features (card1-card6)
     - Address features (addr1, addr2)
     - Counting features (C1-C14)
     - Timedelta features (D1-D15)
     - Selected V features (V1-V321)

### 3. **Fraud Detection Model**
   - Algoritma: LightGBM (Light Gradient Boosting Machine)
   - Optimasi untuk imbalanced data (fraud rate 3.5%)
   - Fast training dengan memory efficiency

## 📁 Struktur Notebook

### 1. **Import Library**
   - Setup environment
   - Import dependencies
   - Konfigurasi warnings

### 2. **Memory Optimization Function**
   - `reduce_mem_usage()`: Aggressive memory reduction
   - Automatic dtype conversion
   - Memory usage tracking

### 3. **Data Loading**
   - Download dari Google Drive
   - Smart column selection
   - Memory optimization
   - Garbage collection

### 4. **Exploratory Data Analysis (EDA)**
   - Target distribution analysis
   - Missing values analysis
   - Fraud rate calculation
   - Visualizations

### 5. **Data Preprocessing** *(Coming in notebook)*
   - Missing value handling
   - Label encoding untuk categorical features
   - Train-test split

### 6. **Model Training** *(Coming in notebook)*
   - LightGBM classifier
   - Handling imbalanced data
   - Parameter tuning

### 7. **Model Evaluation** *(Coming in notebook)*
   - ROC-AUC Score
   - Classification Report
   - Confusion Matrix
   - Feature importance analysis

## 💻 Cara Menggunakan

### Prerequisites
```bash
pip install numpy pandas gdown scikit-learn lightgbm matplotlib
```

### Menjalankan Notebook

1. **Clone repository**
```bash
git clone https://github.com/fariztelkom/MachineLearning-FarisEza.git
cd "MachineLearning-FarisEza/UTS/End-to-End Transaction Fraud Detection Pipeline"
```

2. **Buka di Google Colab**
   - Upload notebook ke Google Colab
   - Atau akses langsung via link

3. **Jalankan sel secara berurutan**
   - Dataset akan otomatis didownload dari Google Drive
   - Memory optimization akan berjalan otomatis
   - Proses training akan dimulai

### Akses File Raw
```
https://raw.githubusercontent.com/fariztelkom/MachineLearning-FarisEza/refs/heads/main/UTS/End-to-End%20Transaction%20Fraud%20Detection%20Pipeline/E2E_Fraud_Detection.ipynb
```

## 📈 Output yang Dihasilkan

### Data Loading Results
- Train shape & test shape
- Memory usage before & after optimization
- Memory reduction percentage
- Fraud rate statistics

### EDA Results
- Target distribution visualization
- Missing values analysis (top 10)
- Feature correlation analysis

### Model Performance *(Coming soon)*
- ROC-AUC Score
- Precision, Recall, F1-Score
- Confusion Matrix
- Feature Importance Plot

## 💡 Highlights

✅ **Extreme Memory Efficiency** - Reduksi memory hingga 70% dengan smart dtype conversion  
✅ **Automatic Download** - Dataset didownload otomatis dari Google Drive  
✅ **Smart Feature Selection** - Hanya memuat kolom yang relevan  
✅ **Imbalanced Data Handling** - Optimasi untuk fraud rate rendah (3.5%)  
✅ **LightGBM** - Fast & accurate gradient boosting  
✅ **Production-Ready** - Pipeline yang siap untuk deployment  

## 🎯 Tantangan Dataset

### Class Imbalance
- **Normal transactions**: 96.5%
- **Fraud transactions**: 3.5%
- **Solution**: Menggunakan ROC-AUC sebagai metrik utama & class weight adjustment

### High Dimensionality
- **Original columns**: 394 kolom
- **Selected columns**: 125 kolom (train), 124 kolom (test)
- **Solution**: Smart feature selection berdasarkan missing values & domain knowledge

### Large Dataset Size
- **Total size**: ~1.3 GB
- **Solution**: Memory optimization function & chunked processing

## 📊 Key Metrics

| Metric | Value |
|--------|-------|
| Train Size | 590,540 rows |
| Test Size | 506,691 rows |
| Features | 125 (selected from 394) |
| Fraud Rate | 3.50% |
| Memory Saved | ~70% |

## 🔍 Feature Categories

### Transaction Features (5)
- TransactionID, isFraud, TransactionDT, TransactionAmt, ProductCD

### Card Features (6)
- card1, card2, card3, card4, card5, card6

### Address Features (2)
- addr1, addr2

### Email Features (2)
- P_emaildomain, R_emaildomain

### Counting Features (14)
- C1 through C14

### Timedelta Features (8)
- D1, D2, D3, D4, D5, D10, D11, D15

### V Features (88 selected)
- V1-V20, V29-V38, V44-V54, V69-V78
- V279-V287, V294-V321

## 🔧 Memory Optimization Details

```python
Memory Reduction Strategy:
├── int64 → int8/int16/int32
├── float64 → float16/float32
└── Smart range detection

Results:
├── Train: 563.2MB → 169.5MB (69.9% ↓)
└── Test:  479.4MB → 146.9MB (69.4% ↓)
```

## 📝 Notes

- **Smart Column Selection**: Membuang kolom V dengan missing values >50%
- **Memory First**: Prioritas pada efisiensi memory untuk big data
- **LightGBM Choice**: Optimal untuk dataset besar dengan imbalanced class
- **Production Ready**: Code structure yang clean dan scalable
- **Garbage Collection**: Automatic memory cleanup setelah setiap operasi besar

## 🤝 Kontribusi

Repository ini dibuat untuk keperluan akademik (UTS Machine Learning). Untuk pertanyaan atau saran, silakan buka issue di GitHub.

## 👨‍💻 Author

**Fariz Telkom**  
GitHub: [@fariztelkom](https://github.com/fariztelkom)

## 📄 License

Educational/Academic Purpose - Universitas Telkom

---

⭐ Jika repository ini membantu, jangan lupa berikan star!

**Last Updated**: December 2024

## 🎓 Learning Objectives

- ✅ Handling large-scale imbalanced datasets
- ✅ Memory optimization techniques for big data
- ✅ Feature engineering for fraud detection
- ✅ LightGBM implementation
- ✅ Production-ready ML pipeline development