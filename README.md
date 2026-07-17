  
# ETL E-Commerce Orders Pipeline

Proyek ini merupakan implementasi **End-to-End ETL (Extract, Transform, Load)** untuk memproses data transaksi e-commerce dari data mentah menjadi data yang bersih, tervalidasi, dan siap digunakan untuk analisis maupun pelaporan.

Selain proses ETL, proyek ini juga menerapkan **mini orchestrator berbasis Python** yang mensimulasikan konsep dasar Apache Airflow, seperti dependency antar task, retry, logging, dan eksekusi pipeline secara berurutan.

---

# Deskripsi Proyek

Pipeline ini dirancang untuk:

- Mengambil data transaksi dari file CSV.
- Membersihkan dan mentransformasi data.
- Memvalidasi kualitas data.
- Menyimpan data bersih ke dalam file output.
- Menghasilkan laporan ringkasan transaksi.
- Mencatat proses eksekusi pipeline melalui file log.

---

# Dataset

## Data Masukan

- `raw_orders.csv`
- `raw_products.csv`

Data mentah memiliki beberapa permasalahan, seperti:

- Data duplikat
- Missing values
- Harga negatif
- Format tanggal yang tidak konsisten
- Penulisan nama kota yang berbeda-beda
- Penulisan channel penjualan yang tidak konsisten

---

# Alur ETL

```text
Extract
   ↓
Transform
   ↓
Validate
   ↓
Load
   ↓
Generate Report
   ↓
Notify
```

---

# Proses Transformasi Data

Pipeline melakukan beberapa proses pembersihan data, antara lain:

- Menghapus data duplikat.
- Menghapus data dengan harga negatif.
- Mengisi missing values.
- Menstandarkan format tanggal.
- Menstandarkan penulisan nama kota dan channel.
- Menambahkan kolom:
  - `bulan`
  - `kategori_harga`

---

# Output

Pipeline menghasilkan beberapa file berikut:

| File | Deskripsi |
|------|-----------|
| `orders_clean.csv` | Data transaksi yang telah dibersihkan | 
<img width="1000"  alt="image" src="https://github.com/user-attachments/assets/79c8627d-02e2-4bbc-8d2b-4cdc21119d97" />

| `summary_report.csv` | Ringkasan transaksi berdasarkan kategori |
<img width="1000"  alt="image" src="https://github.com/user-attachments/assets/d3d24335-1bfd-4334-a396-24bf139db951" />

| `pipeline_log.txt` | Log proses eksekusi pipeline |
<img width="1000"  alt="image" src="https://github.com/user-attachments/assets/e0d97dff-92ed-4bdc-8ad0-d4c33803630c" />


---

# Struktur Proyek

```text
ETL-E-Commerce-Orders/
│
├── ETL_Order_Product.ipynb
├── etl_design.md
├── orders_clean.csv
├── summary_report.csv
├── pipeline_log.txt
├── README.md
├── raw_orders.csv
└── raw_products.csv
```

---

# Tools yang Digunakan

- Python
- Pandas
- NumPy
- CSV
- Apache Airflow (Konsep)

---

# Fitur

- End-to-End ETL Pipeline
- Data Cleaning
- Data Validation
- Summary Report
- Logging
- Retry Mechanism
- Mini Orchestrator
- Simulasi Workflow Apache Airflow

---

# Pengembangan Selanjutnya

Beberapa pengembangan yang dapat dilakukan:

- Integrasi dengan Apache Airflow.
- Menyimpan data ke PostgreSQL atau BigQuery.
- Menjalankan pipeline menggunakan Docker.
- Deploy ke Cloud Platform.

---

# Author

**Stefhanie Amalia**

Mahasiswa Sistem Informasi | Data Engineering & Business Intelligence Enthusiast
