# E-Commerce Big Data Analytics Pipeline

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![PySpark](https://img.shields.io/badge/PySpark-3.x-orange.svg)](https://spark.apache.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 About

Proyek ini merupakan implementasi **Enterprise Batch Data Pipeline** menggunakan **Apache Spark** untuk analisis data e-commerce. Pipeline ini dirancang untuk memproses data transaksi e-commerce dalam skala besar dengan pendekatan berbasis **Data Lakehouse Architecture** yang terdiri dari multiple layers (Raw, Clean, Curated, dan Serving).

**Mata Kuliah:** Big Data  
**Topik:** Batch Processing & Data Analytics dengan PySpark

---

## 🎯 Fitur Utama

- ✅ **ETL Pipeline** - Extract, Transform, Load data dari raw CSV
- ✅ **Data Cleaning** - Menghapus duplikasi, null values, dan data invalid
- ✅ **Data Transformation** - Kalkulasi total amount dan agregasi
- ✅ **Multi-Layer Architecture** - Raw → Clean → Curated → Serving
- ✅ **Partitioning** - Data partisi berdasarkan kategori produk
- ✅ **Analytics KPIs** - Total revenue, top products, category analysis
- ✅ **Logging System** - Monitoring dan tracking pipeline execution
- ✅ **Performance Tracking** - Execution time measurement

---

## 🏗️ Arsitektur Data

```
┌─────────────┐
│  Raw Layer  │  ← CSV files (ecommerce_raw.csv)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Clean Layer │  ← Parquet files (deduplicated, validated)
└──────┬──────┘
       │
       ▼
┌──────────────┐
│ Curated Layer│  ← Aggregated metrics (by category, product)
└──────┬───────┘
       │
       ▼
┌───────────────┐
│ Serving Layer │  ← CSV exports untuk visualization/reporting
└───────────────┘
```

---

## 📂 Struktur Proyek

```
│
├── data/
│   ├── raw/                    # Layer 1: Raw data (CSV)
│   │   └── ecommerce_raw.csv
│   │
│   ├── clean/                  # Layer 2: Cleaned data
│   │   ├── parquet/           # Format Parquet untuk performa
│   │   └── partitioned_by_category/  # Partisi by category
│   │
│   ├── curated/                # Layer 3: Aggregated metrics
│   │   ├── category_revenue/
│   │   ├── top_products/
│   │   └── avg_transaction/
│   │
│   └── serving/                # Layer 4: CSV untuk BI tools
│       ├── total_revenue/
│       ├── top_products/
│       ├── category_revenue/
│       └── avg_transaction/
│
├── scripts/
│   ├── batch_pipeline_enterprise.py  # Main ETL pipeline
│   └── analytics_layer.py            # Analytics & KPI calculation
│
├── logs/                       # Pipeline execution logs
│   └── batch_pipeline.log
│
├── venv/                       # Python virtual environment
├── .gitignore
└── README.md
```

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| **Python 3.8+** | Bahasa pemrograman utama |
| **Apache Spark (PySpark)** | Distributed data processing |
| **Parquet** | Columnar storage format |
| **CSV** | Raw data & serving layer |
| **Logging** | Pipeline monitoring |

---

## 📊 KPI & Metrics

Pipeline ini menghasilkan beberapa Key Performance Indicators (KPIs):

1. **Total Revenue** - Total pendapatan dari seluruh transaksi
2. **Top 10 Products** - Produk terlaris berdasarkan quantity
3. **Revenue per Category** - Pendapatan per kategori produk
4. **Average Transaction Value** - Rata-rata nilai transaksi per customer

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8 atau lebih baru
- Apache Spark 3.x
- Minimal 4GB RAM (8GB recommended)

### Installation

1. **Clone repository**
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. **Buat virtual environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # macOS/Linux
   # atau
   venv\Scripts\activate     # Windows
   ```

3. **Install dependencies**
   ```bash
   pip install pyspark
   pip install pandas  # optional, untuk data exploration
   ```

4. **Verifikasi instalasi**
   ```bash
   python -c "import pyspark; print(pyspark.__version__)"
   ```

---

## 📖 Usage

### 1. Jalankan Batch Pipeline (ETL)

Pipeline ini melakukan ekstraksi, cleaning, transformasi, dan loading data:

```bash
python scripts/batch_pipeline_enterprise.py
```

**Output:**
- Data bersih dalam format Parquet
- Data terpartisi berdasarkan kategori
- Metrics agregat (category revenue, top products, avg transactions)
- Log file di folder `logs/`

### 2. Jalankan Analytics Layer

Menghitung KPI dan export ke CSV untuk reporting:

```bash
python scripts/analytics_layer.py
```

**Output:**
- CSV files di folder `data/serving/`
- KPI summary di console
- Ready untuk import ke Tableau/Power BI

---

## 📝 Detail Pipeline

### Batch Pipeline Enterprise

**Input:** `data/raw/ecommerce_raw.csv`

**Proses:**
1. **Load Data** dengan schema validation
2. **Cleaning:**
   - Remove duplicates
   - Drop null values (transaction_id, customer_id, price, quantity)
   - Filter invalid data (price <= 0, quantity <= 0)
   - Validate & parse transaction_date
3. **Transformation:**
   - Calculate `total_amount = price × quantity`
4. **Aggregation:**
   - Group by category untuk total revenue
   - Top 5 products by quantity
   - Average transaction value per customer
5. **Save:**
   - Parquet format (clean layer)
   - Partitioned by category
   - Curated metrics (parquet)

**Output:**
- `data/clean/parquet/` - Clean data
- `data/clean/partitioned_by_category/` - Partitioned data
- `data/curated/*` - Aggregated metrics

---

### Analytics Layer

**Input:** `data/clean/parquet/`

**Proses:**
1. Load clean Parquet data
2. Calculate KPIs:
   - Total Revenue
   - Top 10 Products
   - Revenue per Category
   - Average Transaction Value per Customer
3. Export to CSV (serving layer)

**Output:**
- `data/serving/total_revenue/` - Total revenue CSV
- `data/serving/top_products/` - Top products CSV
- `data/serving/category_revenue/` - Category revenue CSV
- `data/serving/avg_transaction/` - Avg transaction CSV

---

## 📈 Performance

- **Raw Data Processing:** ~1,000-10,000 records
- **Execution Time:** 5-15 seconds (local mode)
- **Storage Format:** Parquet (compressed, ~70% size reduction vs CSV)
- **Partitioning:** By category untuk query optimization

---

## 🔍 Monitoring & Logging

Semua pipeline execution dicatat dalam log file:

```bash
cat logs/batch_pipeline.log
```

Log berisi:
- Timestamp setiap operasi
- Status (INFO, ERROR, WARNING)
- Execution time

---

## 📊 Visualisasi Data

Data di folder `data/serving/` dapat digunakan untuk:

- **Tableau** - Import CSV files untuk dashboard
- **Power BI** - Connect to CSV sources
- **Excel** - Manual analysis
- **Python (Pandas/Matplotlib)** - Custom visualization

---

## 🐛 Troubleshooting

### Error: "Java not found"
PySpark memerlukan Java 8 atau 11:
```bash
# macOS
brew install openjdk@11

# Verifikasi
java -version
```

### Error: "Memory issues"
Atur Spark memory configuration:
```python
spark = SparkSession.builder \
    .config("spark.driver.memory", "4g") \
    .getOrCreate()
```

### Error: "File not found"
Pastikan working directory benar:
```bash
pwd  # Should be in project root
ls data/raw/  # Should see ecommerce_raw.csv
```

---

## 🤝 Contributing

Proyek ini adalah bagian dari tugas kuliah. Untuk kontribusi:

1. Fork repository
2. Buat feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📄 License

Proyek ini dibuat untuk keperluan akademik - Big Data Course.

---

## 👤 Author

**Akhmad Hafidz Ardianto**  
NIM: 230104040118  
Universitas: [Nama Universitas]  
Email: [email-anda]@[domain]

---

## 🙏 Acknowledgments

- Apache Spark Documentation
- PySpark API Reference
- Big Data Course Materials
- Stack Overflow Community

---

**⭐ Jika proyek ini bermanfaat, jangan lupa berikan star!**
