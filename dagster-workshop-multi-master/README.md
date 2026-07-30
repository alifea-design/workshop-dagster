# Multi-Container Data Engineering Pipeline: E-Commerce & Exchange Rates ETL

Multi-container data engineering pipeline yang mengintegrasikan data transaksi e-commerce, nilai tukar mata uang (*FX rates*), serta prediksi Machine Learning menggunakan Dagster framework. Pipeline ini dirancang untuk memproses data penjualan secara otomatis, mengonversi mata uang transaksi, dan memprediksi pendapatan bisnis secara transparan.

Built on top of [dagster-workshop-multi](https://github.com/alifea-design/workshop-dagster), a multi-container Dagster workshop — see that repo's README for the base architecture (`pipeline_products`, `pipeline_fx`, `pipeline_ml`, `pipeline_analytics`).

---

## What I built

- **Track:** Cross-pipeline analytics & Multi-container Data Platform
- **Data source:** Open Exchange Rates API / Historical FX Rates Dataset & Internal E-Commerce Orders
- **Key assets:**
  - `raw_exchange_rates`: Mengambil data nilai tukar mata uang harian dari API/sumber FX secara otomatis.
  - `cleaned_exchange_rates`: Melakukan pembersihan data, normalisasi format tanggal, dan validasi rasio kurs.
  - `orders_in_eur`: Menggabungkan data order e-commerce dengan data kurs mata uang untuk menghasilkan nominal transaksi standar dalam Euro (EUR).
- **Quality gate:** Menggunakan `@asset_check` untuk memverifikasi bahwa tidak ada nilai kurs yang bernilai nol atau negatif (`rate > 0`) serta memastikan kolom tanggal tidak mengandung *null values* sebelum proses penggabungan data.

---

## Architecture

                   +-------------------+
                   | pipeline_products |
                   +---------+---------+
                             |
                             v
                   +-------------------+
                   |    pipeline_fx    |
                   +---------+---------+
                             |
       +---------------------+---------------------+
       |                                           |
       v                                           v
+--------------------+                       +-------------------+
| pipeline_analytics |                       |    pipeline_ml    |
+--------------------+                       +-------------------+

---

## Running it

```bash
# Build dan jalankan seluruh multi-container setup
docker compose up --build -d
