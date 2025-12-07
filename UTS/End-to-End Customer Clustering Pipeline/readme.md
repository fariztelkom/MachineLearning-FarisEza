# End-to-End Customer Clustering Pipeline

## 📋 Deskripsi Proyek

Pipeline clustering pelanggan end-to-end yang mengimplementasikan proses lengkap dari data loading, exploratory data analysis (EDA), preprocessing, hingga pembangunan model clustering untuk segmentasi pelanggan berbasis data kartu kredit. Proyek ini menggunakan berbagai algoritma clustering untuk mengidentifikasi pola perilaku pelanggan dan memberikan insight bisnis yang actionable.

## 🎯 Tujuan

- Melakukan segmentasi pelanggan secara otomatis menggunakan multiple clustering algorithms
- Mengidentifikasi pola perilaku transaksi dan penggunaan kartu kredit
- Membandingkan performa berbagai algoritma clustering (K-Means, DBSCAN, Hierarchical)
- Memberikan interpretasi bisnis dari setiap segment pelanggan
- Membangun pipeline yang reproducible dan scalable

## 🛠️ Teknologi yang Digunakan

### Core Libraries
- **Python 3.x**
- **Jupyter Notebook / Google Colab**

### Data Processing & Analysis
- `pandas` - Manipulasi dan analisis data
- `numpy` - Komputasi numerik
- `gdown` - Download dataset dari Google Drive

### Machine Learning
- `scikit-learn` - Machine learning algorithms dan preprocessing:
  - `RobustScaler` - Feature scaling robust terhadap outliers
  - `PCA` - Dimensionality reduction
  - `KMeans` - K-Means clustering
  - `DBSCAN` - Density-based clustering
  - `AgglomerativeClustering` - Hierarchical clustering
  - `NearestNeighbors` - Untuk DBSCAN parameter tuning

### Evaluation Metrics
- `silhouette_score` - Mengukur cohesion dan separation
- `calinski_harabasz_score` - Variance ratio criterion
- `davies_bouldin_score` - Cluster similarity measure

### Visualization
- `matplotlib.pyplot` - Plotting dasar
- `seaborn` - Statistical visualizations
- `scipy.cluster.hierarchy` - Dendrogram untuk hierarchical clustering

## 📊 Dataset

Dataset berisi informasi transaksi kartu kredit dari **8,950 customers** dengan **18 features**:

### Features Description

**Identifikasi:**
- `CUST_ID` - Customer ID (unique identifier)

**Balance & Credit:**
- `BALANCE` - Saldo kartu kredit
- `BALANCE_FREQUENCY` - Frekuensi update balance (0-1)
- `CREDIT_LIMIT` - Limit kartu kredit

**Purchase Behavior:**
- `PURCHASES` - Total pembelian
- `ONEOFF_PURCHASES` - Pembelian one-time (tidak cicilan)
- `INSTALLMENTS_PURCHASES` - Pembelian dengan cicilan
- `PURCHASES_FREQUENCY` - Frekuensi pembelian (0-1)
- `ONEOFF_PURCHASES_FREQUENCY` - Frekuensi one-off purchases
- `PURCHASES_INSTALLMENTS_FREQUENCY` - Frekuensi installment purchases
- `PURCHASES_TRX` - Jumlah transaksi pembelian

**Cash Advance:**
- `CASH_ADVANCE` - Total cash advance
- `CASH_ADVANCE_FREQUENCY` - Frekuensi cash advance
- `CASH_ADVANCE_TRX` - Jumlah transaksi cash advance

**Payments:**
- `PAYMENTS` - Total pembayaran
- `MINIMUM_PAYMENTS` - Pembayaran minimum
- `PRC_FULL_PAYMENT` - Persentase full payment (0-1)

**Account Info:**
- `TENURE` - Lama menjadi customer (bulan)

### Data Statistics
- **Total Customers:** 8,950
- **Total Features:** 18
- **Missing Values:** 
  - `MINIMUM_PAYMENTS`: 313 (3.5%)
  - `CREDIT_LIMIT`: 1 (0.01%)

## 🔄 Pipeline Workflow

### 1. **Data Loading & Initial Exploration**

```python
# Download dataset from Google Drive
file_id = "1XtaKeD7b-la2R1ygFBNLj16VD1k0MB2Q"
gdown.download(f"https://drive.google.com/uc?id={file_id}", "data.csv")
df = pd.read_csv("data.csv")
```

**Output:**
- Shape: 8,950 customers × 18 features
- Informasi kolom dan tipe data
- Preview 5 data pertama

### 2. **Exploratory Data Analysis (EDA)**

**Missing Values Analysis:**
- Identifikasi missing values di `MINIMUM_PAYMENTS` (313) dan `CREDIT_LIMIT` (1)
- Handling: Menggunakan **median imputation** untuk kedua kolom

**Statistical Summary:**
- Descriptive statistics (mean, std, min, max, quartiles)
- Distribusi untuk 8 key features:
  - BALANCE
  - PURCHASES
  - CASH_ADVANCE
  - CREDIT_LIMIT
  - PAYMENTS
  - MINIMUM_PAYMENTS
  - PRC_FULL_PAYMENT
  - TENURE

**Key Findings dari EDA:**
- Distribusi sangat skewed untuk features finansial
- Banyak customers dengan nilai 0 untuk purchases/cash advance
- Credit limit bervariasi dari 50 hingga 30,000
- Mayoritas customers memiliki tenure 12 bulan

### 3. **Data Preprocessing**

**Handling Missing Values:**
```python
df_clean['MINIMUM_PAYMENTS'].fillna(df_clean['MINIMUM_PAYMENTS'].median(), inplace=True)
df_clean['CREDIT_LIMIT'].fillna(df_clean['CREDIT_LIMIT'].median(), inplace=True)
```

**Feature Scaling - RobustScaler:**
- Menggunakan `RobustScaler` karena data memiliki banyak outliers
- RobustScaler robust terhadap outliers dengan menggunakan median dan IQR
- Scaling semua numerical features kecuali `CUST_ID`

```python
scaler = RobustScaler()
df_scaled = scaler.fit_transform(df_features)
```

**Dimensionality Reduction - PCA:**
- Menerapkan PCA untuk visualisasi dan mengurangi dimensi
- Mempertahankan komponen yang menjelaskan variance terbesar
- Analisis explained variance ratio

### 4. **Determining Optimal Number of Clusters**

**Metode yang digunakan:**

**A. Elbow Method (WCSS):**
- Within-Cluster Sum of Squares
- Mencari "elbow point" di plot K vs WCSS
- Testing K dari 2 hingga 10

**B. Silhouette Analysis:**
- Mengukur seberapa baik objek cocok dengan cluster-nya
- Score range: [-1, 1], higher is better
- Optimal K memiliki silhouette score tertinggi

**C. Silhouette Diagrams:**
- Visualisasi detail untuk setiap nilai K
- Menampilkan silhouette score per sample
- Membantu identify cluster yang poorly separated

### 5. **Model Development & Comparison**

#### **A. K-Means Clustering**

Algoritma partitioning-based yang membagi data menjadi K clusters.

**Karakteristik:**
- Fast and scalable
- Assumes spherical clusters
- Sensitive to initialization

**Implementation:**
```python
kmeans = KMeans(n_clusters=optimal_k, random_state=42, n_init=10)
kmeans_labels = kmeans.fit_predict(df_scaled)
```

#### **B. DBSCAN (Density-Based Spatial Clustering)**

Algoritma density-based yang menemukan clusters berdasarkan densitas.

**Karakteristik:**
- Dapat menemukan clusters dengan bentuk arbitrary
- Automatically detects outliers (noise points)
- Tidak perlu specify jumlah clusters

**Parameter Tuning:**
- `eps` - Maximum distance between neighbors
- `min_samples` - Minimum samples untuk core point
- Menggunakan k-distance graph untuk menentukan eps

```python
# Find optimal eps using k-distance graph
neighbors = NearestNeighbors(n_neighbors=min_samples)
distances, indices = neighbors.fit(df_scaled).kneighbors(df_scaled)

# DBSCAN clustering
dbscan = DBSCAN(eps=optimal_eps, min_samples=min_samples)
dbscan_labels = dbscan.fit_predict(df_scaled)
```

#### **C. Hierarchical Clustering (Agglomerative)**

Algoritma bottom-up yang membangun hierarchy of clusters.

**Karakteristik:**
- Creates dendrogram untuk visualisasi
- Flexible linkage methods (ward, average, complete)
- Tidak perlu specify K di awal

**Implementation:**
```python
# Create dendrogram
linkage_matrix = linkage(df_scaled, method='ward')
dendrogram(linkage_matrix)

# Agglomerative clustering
hierarchical = AgglomerativeClustering(n_clusters=optimal_k, linkage='ward')
hierarchical_labels = hierarchical.fit_predict(df_scaled)
```

### 6. **Model Evaluation & Comparison**

**Evaluation Metrics:**

| Metric | K-Means | DBSCAN | Hierarchical |
|--------|---------|---------|--------------|
| **Silhouette Score** | Higher is better (0 to 1) | - | - |
| **Calinski-Harabasz** | Higher is better | - | - |
| **Davies-Bouldin** | Lower is better | - | - |
| **Number of Clusters** | User-defined | Auto-detected | User-defined |
| **Noise Points** | None | Detected | None |

**Silhouette Score Interpretation:**
- 0.71-1.0: Strong structure
- 0.51-0.70: Reasonable structure
- 0.26-0.50: Weak structure
- < 0.25: No substantial structure

### 7. **Cluster Analysis & Profiling**

**Per-Cluster Statistics:**
- Compute mean values untuk semua features per cluster
- Identifikasi karakteristik unik setiap cluster
- Size distribution (persentase customers per cluster)

**Feature Importance:**
- Analisis features yang paling membedakan antar clusters
- Radar charts untuk cluster comparison
- Box plots untuk distribusi features

### 8. **Visualization**

**A. 2D Cluster Visualization:**
- PCA reduction ke 2 komponen
- Scatter plot dengan color-coded clusters
- Cluster centers (untuk K-Means)

**B. 3D Cluster Visualization:**
- PCA reduction ke 3 komponen
- Interactive 3D plots
- Better separation visualization

**C. Cluster Characteristics:**
- Heatmaps untuk cluster profiles
- Parallel coordinates plot
- Pair plots untuk feature relationships

**D. Hierarchical Dendrogram:**
- Tree structure visualization
- Optimal cut level identification
- Cluster merge history

## 📈 Expected Results & Insights

### Typical Customer Segments

**Segment 1: High Value Customers**
- High credit limit (> $10,000)
- High purchase frequency
- Regular full payments
- Low cash advance usage
- **Business Strategy:** VIP programs, premium offers

**Segment 2: Revolvers**
- Moderate to high balance
- Low full payment percentage
- Consistent minimum payments
- Regular purchase activity
- **Business Strategy:** Interest income, balance transfer offers

**Segment 3: Transactors**
- High purchase volume
- High full payment rate
- Low or zero balance carried
- Minimal cash advance
- **Business Strategy:** Rewards programs, cashback offers

**Segment 4: Cash Advance Users**
- High cash advance frequency
- Low purchase activity
- Higher risk profile
- **Business Strategy:** Risk monitoring, fee-based revenue

**Segment 5: Inactive/Dormant**
- Low activity across all metrics
- Minimal purchases and payments
- **Business Strategy:** Re-engagement campaigns, churn prevention

## 🚀 Cara Menjalankan

### Prerequisites

```bash
# Install required libraries
pip install pandas numpy matplotlib seaborn scikit-learn scipy gdown
```

### Menjalankan di Google Colab

1. Upload notebook ke Google Colab
2. Jalankan cell pertama untuk install dan import libraries
3. Dataset akan otomatis di-download dari Google Drive
4. Run all cells secara berurutan

### Menjalankan di Local Jupyter

```bash
# Clone repository
git clone https://github.com/fariztelkom/MachineLearning-FarisEza.git

# Navigate to directory
cd "MachineLearning-FarisEza/UTS/End-to-End Customer Clustering Pipeline"

# Launch Jupyter
jupyter notebook E2E_Clustering.ipynb
```

### Struktur Notebook

```
E2E_Clustering.ipynb
│
├── 1. Library Import & Data Loading
│   ├── Import dependencies
│   ├── Download dataset via gdown
│   └── Initial data preview
│
├── 2. Data Exploration & Missing Value Handling
│   ├── Missing values check
│   ├── Statistical summary
│   ├── Median imputation
│   └── Distribution visualizations
│
├── 3. Data Preprocessing
│   ├── Feature scaling (RobustScaler)
│   └── PCA for dimensionality reduction
│
├── 4. Optimal K Determination
│   ├── Elbow method (WCSS)
│   ├── Silhouette scores
│   └── Silhouette diagrams
│
├── 5. Clustering Algorithms
│   ├── K-Means implementation
│   ├── DBSCAN implementation
│   └── Hierarchical clustering + dendrogram
│
├── 6. Model Evaluation
│   ├── Multiple metrics calculation
│   └── Algorithm comparison
│
├── 7. Cluster Analysis
│   ├── Cluster statistics
│   ├── Feature profiling
│   └── Business interpretation
│
└── 8. Visualizations
    ├── 2D/3D cluster plots
    ├── Heatmaps
    └── Comparison charts
```

## 📊 Key Performance Indicators

### Data Quality
- ✅ Missing Values: Handled via median imputation
- ✅ Outliers: Robust scaling applied
- ✅ Feature Distribution: Analyzed and visualized

### Model Performance
- Silhouette Score: Measures cluster quality
- Calinski-Harabasz Index: Variance between/within clusters
- Davies-Bouldin Index: Cluster separation
- Inertia (K-Means): Within-cluster sum of squares

### Business Impact
- Clear customer segmentation
- Actionable marketing strategies
- Risk profiling
- Personalization opportunities

## 💡 Business Recommendations

### 1. **Marketing Personalization**
- Customize campaigns berdasarkan cluster characteristics
- Targeted promotions sesuai spending behavior
- Channel optimization per segment

### 2. **Risk Management**
- Monitor cash advance clusters lebih ketat
- Early warning system untuk at-risk customers
- Dynamic credit limit adjustments

### 3. **Revenue Optimization**
- Upsell opportunities di transactors segment
- Interest income optimization dari revolvers
- Fee structure customization

### 4. **Customer Retention**
- Re-engagement programs untuk dormant accounts
- Loyalty rewards untuk high-value customers
- Churn prediction dan prevention

### 5. **Product Development**
- Feature development based on segment needs
- Pricing strategy differentiation
- Service tier creation

## 🔮 Future Improvements

### Technical Enhancements
- [ ] Implement time-series clustering untuk behavior evolution
- [ ] Add customer lifetime value (CLV) calculation per segment
- [ ] Integrate with real-time transaction data
- [ ] Automated retraining pipeline
- [ ] Model deployment (API/Dashboard)

### Analysis Depth
- [ ] Feature engineering (ratios, interactions)
- [ ] Subgroup discovery within clusters
- [ ] Anomaly detection layer
- [ ] Cluster stability analysis
- [ ] Cross-validation dengan time splits

### Visualization & Reporting
- [ ] Interactive dashboards (Plotly Dash, Streamlit)
- [ ] Automated report generation
- [ ] Real-time monitoring dashboard
- [ ] A/B testing framework untuk marketing strategies

### Advanced Techniques
- [ ] Ensemble clustering methods
- [ ] Deep learning-based clustering (Autoencoders)
- [ ] Graph-based clustering
- [ ] Fuzzy clustering (soft assignments)

## 📝 Technical Notes

### RobustScaler vs StandardScaler
- **RobustScaler** dipilih karena data memiliki banyak outliers
- Menggunakan median dan IQR instead of mean dan std
- Lebih robust terhadap extreme values

### PCA Considerations
- PCA digunakan untuk visualisasi, bukan final clustering
- Original features digunakan untuk clustering untuk interpretability
- Explained variance analysis untuk dimensionality understanding

### Algorithm Selection
- **K-Means:** Fast, scalable, interpretable
- **DBSCAN:** Handles arbitrary shapes, detects outliers
- **Hierarchical:** Provides dendrogram, flexible K selection

### Evaluation Best Practices
- Multiple metrics untuk comprehensive evaluation
- Visual inspection penting untuk validation
- Business context harus guide final cluster selection

## 🛠️ Troubleshooting

### Common Issues

**Problem:** Dataset tidak ter-download
```python
# Solution: Verify Google Drive file permissions
# Make sure file is publicly accessible
```

**Problem:** Memory error dengan dataset besar
```python
# Solution: Use batch processing atau sample data
df_sample = df.sample(n=5000, random_state=42)
```

**Problem:** DBSCAN hanya menghasilkan 1-2 clusters
```python
# Solution: Tune eps dan min_samples parameters
# Use elbow point dari k-distance graph
```

**Problem:** Silhouette score sangat rendah
```python
# Solution: 
# 1. Try different K values
# 2. Check feature scaling
# 3. Consider feature selection
# 4. Explore different algorithms
```

## 👥 Author

**Faris Eza**
- GitHub: [@fariztelkom](https://github.com/fariztelkom)
- Repository: [MachineLearning-FarisEza](https://github.com/fariztelkom/MachineLearning-FarisEza)

## 📄 License

Project ini dibuat untuk keperluan akademis (UTS - Ujian Tengah Semester).

## 🙏 Acknowledgments

- **Dataset Source:** Credit Card Dataset for Clustering
- **Platform:** Google Colab
- **Libraries:** scikit-learn, pandas, matplotlib, seaborn
- **Reference:** Best practices in customer segmentation

## 📚 References

### Academic Papers
- "Customer Segmentation Using Clustering Techniques" - Various sources
- "Comparative Analysis of Clustering Algorithms" - Research papers

### Documentation
- [scikit-learn Clustering Documentation](https://scikit-learn.org/stable/modules/clustering.html)
- [PCA Explained](https://scikit-learn.org/stable/modules/decomposition.html#pca)
- [Silhouette Analysis](https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html)

### Tutorials
- Customer Segmentation best practices
- K-Means optimization techniques
- DBSCAN parameter tuning guides

---

**Last Updated:** December 2024  
**Version:** 1.0  
**Status:** ✅ Completed  
**Course:** Machine Learning - UTS Project

---

## 📞 Contact & Support

Untuk pertanyaan, saran, atau kolaborasi:
- 📧 Email: [Contact via GitHub]
- 💬 Issues: [GitHub Issues](https://github.com/fariztelkom/MachineLearning-FarisEza/issues)
- ⭐ Star repo ini jika bermanfaat!

---

*Made with ❤️ for Machine Learning Education*
