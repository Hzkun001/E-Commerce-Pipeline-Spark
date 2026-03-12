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
- ✅ **Streaming Pipeline** - Real-time processing dengan Spark Structured Streaming
- ✅ **Transaction Generator** - Simulasi data transaksi real-time (JSON)
- ✅ **Streamlit Dashboard** - Real-time analytics dashboard interaktif

---

## 🏗️ Arsitektur Data

```
                    ┌──────────────────────┐
                    │   Batch Pipeline     │
                    └──────────────────────┘
┌─────────────┐           │
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
│ Serving Layer │  ← CSV + Parquet untuk visualization/reporting
└───────────────┘

                    ┌──────────────────────┐
                    │   Streaming Pipeline │
                    └──────────────────────┘
┌──────────────────┐       │
│ Transaction      │ ← JSON files (stream_data/)
│ Generator        │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│ Structured       │  ← Spark Streaming (micro-batch)
│ Streaming        │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│ Streamlit        │  ← Real-time dashboard
│ Dashboard        │
└──────────────────┘
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
│   ├── analytics_layer.py            # Analytics & KPI calculation
│   ├── streaming_layer.py            # Spark Structured Streaming
│   └── transaction_generator.py      # Real-time JSON data simulator
│
├── dashboard/
│   └── dashboard_streamlit.py        # Streamlit real-time dashboard
│
├── stream_data/                # JSON transaksi real-time (generated)
│   └── transaction_*.json
│
├── logs/                       # Pipeline execution logs
│   ├── batch_pipeline.log
│   └── stream_checkpoint/      # Spark streaming checkpoint
│
├── requirements.txt            # Python dependencies
├── venv/                       # Python virtual environment
├── .gitignore
└── README.md
```

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| **Python 3.8+** | Bahasa pemrograman utama |
| **Apache Spark (PySpark)** | Distributed batch & stream processing |
| **Spark Structured Streaming** | Real-time data pipeline |
| **Streamlit** | Real-time web dashboard |
| **Parquet** | Columnar storage format |
| **CSV / JSON** | Raw data & serving layer |
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
   pip install -r requirements.txt
   ```

4. **Verifikasi instalasi**
   ```bash
   python -c "import pyspark; print(pyspark.__version__)"
   python -c "import streamlit; print(streamlit.__version__)"
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

### 3. Jalankan Streaming Pipeline (Real-Time)

Pipeline ini memproses JSON transaksi yang masuk secara real-time:

**Terminal 1 — Jalankan Transaction Generator:**
```bash
python scripts/transaction_generator.py
```

**Terminal 2 — Jalankan Spark Streaming:**
```bash
spark-submit scripts/streaming_layer.py
```

**Terminal 3 — Jalankan Streamlit Dashboard:**
```bash
streamlit run dashboard/dashboard_streamlit.py
```

**Output:**
- JSON transaksi di folder `stream_data/` (generated setiap 3 detik)
- Parquet hasil streaming di `data/serving/stream/`
- Dashboard real-time di browser (default: `http://localhost:8501`)

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

- **Streamlit Dashboard** - Real-time dashboard (built-in, lihat `dashboard/`)
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


## 🙏 Acknowledgments

- Apache Spark Documentation
- PySpark API Reference
- Big Data Course Materials
- Stack Overflow Community

---

**⭐ Jika proyek ini bermanfaat, jangan lupa berikan star!**
