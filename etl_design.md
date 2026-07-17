# ETL Pipeline Design: E-Commerce Orders

## 1. Overview
Pipeline ETL ini dirancang untuk memproses data transaksi e-commerce harian dari data mentah menjadi data yang bersih, terstruktur, dan siap digunakan untuk analisis maupun pelaporan. Proses ETL meliputi pengambilan data (Extract), pembersihan dan transformasi data (Transform), validasi kualitas data (Validate), serta penyimpanan hasil akhir (Load). Selain itu, proyek ini juga menerapkan konsep orkestrasi menggunakan mini orchestrator berbasis Python yang mensimulasikan alur kerja Apache Airflow.

## 2. Extract
- Sumber:
  Pipeline menggunakan dua file CSV sebagai sumber data:
  **raw_orders.csv** – Berisi data transaksi e-commerce yang masih memiliki berbagai permasalahan kualitas data.
  **raw_products.csv** – Berisi data referensi produk yang digunakan sebagai data pendukung.
- Format: CSV (Comma Separated Values)
- Volume: 
   **raw_orders.csv** : 130 baris data transaksi.
   **raw_products.csv** : 10 baris data produk.

## 3. Transform
Beberapa proses transformasi dilakukan untuk meningkatkan kualitas data.

      1. Menghapus Data Duplikat
      Baris yang memiliki data duplikat dihapus agar tidak terjadi perhitungan transaksi yang berulang.
      
      2. Menghapus Data Tidak Valid
      Data dengan nilai **total_harga** negatif dihapus karena dianggap sebagai data yang tidak valid.
      
      3. Menangani Missing Values
      - Nilai kosong pada **customer_email** diisi dengan `unknown@placeholder.com`.
      - Nilai kosong pada **total_harga** diisi menggunakan nilai median agar distribusi data tetap terjaga.
      
      4. Menstandarkan Format Tanggal
      Berbagai format tanggal diubah menjadi format **datetime** agar lebih mudah diproses dan dianalisis.
      
      5. Menstandarkan Format Teks
      - Nama kota diubah menjadi format **Title Case**.
      - Nama channel diubah menjadi huruf kecil dan menggunakan underscore (_) agar konsisten.
      
      6. Membuat Kolom Baru
      Dibuat dua kolom baru:
      - **bulan**, yang diambil dari tanggal transaksi.
      - **kategori_harga**, yang mengelompokkan transaksi menjadi kategori **kecil**, **sedang**, dan **besar** berdasarkan total harga.


## 4. Load
- Tujuan:  Setelah proses transformasi dan validasi selesai, data disimpan ke dalam dua file output
- Format output: 
  **orders_clean.csv** sebagai dataset transaksi yang telah dibersihkan.
  **summary_report.csv** sebagai ringkasan jumlah transaksi, total pendapatan, dan rata-rata pendapatan pada setiap kategori produk.
  File output ini dapat digunakan untuk proses analisis data maupun pembuatan dashboard.

## 5. Orchestration
- Tool: Apache Airflow
- Schedule: Dalam implementasi nyata menggunakan Apache Airflow, pipeline dapat dijalankan secara otomatis setiap hari pukul **06.00** menggunakan cron expression:

- DAG flow: 
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

## 6. Error Handling
- Skenario 1: File Input Tidak Ditemukan
  Pipeline akan dihentikan apabila file sumber data tidak ditemukan dan sistem akan mencatat pesan kesalahan ke dalam log.
- Skenario 2: Validasi Data Gagal
  Pipeline akan berhenti apabila masih ditemukan:
- Data duplikat.
- Missing values.
- Harga negatif.
- Format tanggal yang tidak valid.

## 7. Monitoring
- Bagaimana cara tahu pipeline sukses?
- Bagaimana cara tahu data berkualitas?

Keberhasilan pipeline dipantau melalui file **pipeline_log.txt**.
File log mencatat:
- Nama task yang dijalankan.
- Status eksekusi (Running, Success, Failed).
- Waktu eksekusi.
- Jumlah percobaan (retry).
- Pesan kesalahan apabila terjadi error.
Selain itu, kualitas data dipastikan melalui proses validasi sebelum data disimpan ke file output.
