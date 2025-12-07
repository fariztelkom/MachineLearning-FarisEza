# End-to-End Regression Pipeline for Song Release Year Prediction

## 📖 Deskripsi Proyek

Repository ini berisi implementasi lengkap pipeline machine learning untuk memprediksi tahun rilis lagu berdasarkan fitur-fitur audio. Proyek ini merupakan bagian dari UTS Machine Learning dengan pendekatan end-to-end yang mencakup data loading, exploratory data analysis (EDA), preprocessing, training berbagai model regresi, dan evaluasi performa.

## 🎯 Tujuan

Membangun model regresi yang dapat memprediksi tahun rilis lagu (1922-2011) berdasarkan 90 fitur audio menggunakan berbagai algoritma machine learning dan membandingkan performanya.

## 📊 Dataset

- **Ukuran Dataset**: 515,345 baris × 91 kolom
- **Jumlah Fitur**: 90 fitur audio
- **Target Variable**: Tahun rilis (year)
- **Rentang Tahun**: 1922 - 2011
- **Ukuran File**: ~443 MB
- **Format**: CSV (float32)

### Statistik Dataset
- Total tahun unik: 89 tahun
- Tahun paling umum: 2007
- Mean: 1998.4
- Median: 2002

## 🔧 Teknologi & Library

### Core Libraries
```python
- numpy
- pandas
- matplotlib
- seaborn
```

### Machine Learning
```python
- scikit-learn
  - StandardScaler
  - train_test_split
  - cross_val_score
```

### Models Implemented
- Ridge Regression
- Lasso Regression
- Random Forest Regressor
- Gradient Boosting Regressor

## 📁 Struktur Notebook

### 1. **Import Libraries**
   - Setup environment
   - Import semua dependencies
   - Konfigurasi warnings dan random seed

### 2. **Data Loading**
   - Download dataset dari Google Drive menggunakan `gdown`
   - Load data dengan pandas
   - Initial data inspection
   - Memory usage analysis

### 3. **Data Exploration**
   - Statistik deskriptif target variable (year)
   - Analisis distribusi fitur
   - Visualisasi:
     - Distribusi tahun
     - Korelasi fitur
     - Exploratory plots

### 4. **Data Preprocessing** *(Coming in notebook)*
   - Handling missing values
   - Feature scaling dengan StandardScaler
   - Train-test split

### 5. **Model Training** *(Coming in notebook)*
   - Ridge Regression
   - Lasso Regression
   - Random Forest
   - Gradient Boosting
   - Cross-validation

### 6. **Model Evaluation** *(Coming in notebook)*
   - Mean Squared Error (MSE)
   - Mean Absolute Error (MAE)
   - R² Score
   - Model comparison

## 🚀 Cara Menggunakan

### Prerequisites
```bash
pip install gdown numpy pandas matplotlib seaborn scikit-learn
```

### Menjalankan Notebook

1. **Clone repository**
```bash
git clone https://github.com/fariztelkom/MachineLearning-FarisEza.git
cd "MachineLearning-FarisEza/UTS/End-to-End Regression Pipeline for Song Release Year Prediction"
```

2. **Buka di Google Colab**
   - Upload notebook ke Google Colab
   - Atau akses langsung via link

3. **Jalankan sel secara berurutan**
   - Dataset akan otomatis didownload dari Google Drive
   - Proses training akan berjalan otomatis

### Akses File Raw
```
https://raw.githubusercontent.com/fariztelkom/MachineLearning-FarisEza/refs/heads/main/UTS/End-to-End%20Regression%20Pipeline%20for%20Song%20Release%20Year%20Prediction/E2E_Song_Release_per_Year.ipynb
```

## 📈 Output yang Dihasilkan

### Data Loading Results
- Shape dataset
- Memory usage
- Data types
- Preview data

### Exploratory Analysis
- Target variable statistics
- Feature statistics
- Visualizations:
  - Year distribution histogram
  - Feature correlation heatmap
  - Sample feature distributions

### Model Performance *(Coming soon)*
- Metrics untuk setiap model
- Comparison plot
- Best model recommendation

## 💡 Fitur Utama

✅ **Automatic Data Download** - Dataset didownload otomatis dari Google Drive  
✅ **Memory Efficient** - Menggunakan float32 untuk menghemat memory  
✅ **Comprehensive EDA** - Visualisasi dan statistik lengkap  
✅ **Multiple Models** - Perbandingan 4 algoritma regresi  
✅ **Cross-validation** - Evaluasi model yang robust  
✅ **Clean Code** - Kode terstruktur dan mudah dipahami  

## 📝 Notes

- Dataset menggunakan dtype `float32` untuk efisiensi memory (178.90 MB)
- Random seed diset ke 42 untuk reproducibility
- Warnings difilter untuk output yang lebih bersih
- Garbage collection (`gc`) diimport untuk memory management

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