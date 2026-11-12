# Analisis Penjualan Produk Nike U.S. 2020–2021

Proyek **Ujian Tengah Semester (UTS) Kapita Selekta Informatika**: menganalisis data penjualan Nike di Amerika Serikat dan membangun model klasifikasi untuk memprediksi apakah sebuah transaksi memiliki **volume penjualan tinggi (High Sales Volume)**.

> Nama: Najwa kathrenia · NIM: 20240801221 · Program Studi: Teknik Informatika

---

## Tujuan

1. Mengeksplorasi pola penjualan Nike U.S. periode Januari 2020 – Desember 2021.
2. Membuat target biner dari kolom `Units Sold` (di atas median = **High**, selain itu = **Low/Normal**).
3. Membandingkan tiga algoritma klasifikasi: Logistic Regression, Random Forest, dan Gradient Boosting.

## Dataset

- **Sumber:** [Kaggle](https://www.kaggle.com/datasets) — Nike U.S. Sales Dataset (2020–2021)
- **Ukuran:** 9.360 baris × 9 kolom, tanpa nilai kosong dan tanpa duplikat

| Kolom | Keterangan |
| --- | --- |
| `Invoice Date` | Tanggal faktur (format **dd-mm-yyyy**) |
| `Product` | 6 kategori produk (Men's/Women's × Street Footwear / Athletic Footwear / Apparel) |
| `Region` | 5 wilayah AS |
| `Retailer` | 6 retailer (Foot Locker, West Gear, Sports Direct, Kohl's, Amazon, Walmart) |
| `Sales Method` | In-store, Outlet, Online |
| `State` | 48 negara bagian |
| `Price per Unit` | Harga per unit (USD) |
| `Total Sales` | Total penjualan (USD) |
| `Units Sold` | Jumlah unit terjual |

## Alur Pengerjaan

1. Menentukan objek bisnis
2. Mengumpulkan data dan menentukan objek data
3. Menelaah, mengonstruksi, dan memvalidasi data (cek nilai kosong, duplikat, format tanggal, EDA)
4. Melabeli data (`High_Sales`) dan encoding fitur kategorikal dengan `LabelEncoder`
5. Membangun model (split 70:30, `stratify=y`, `random_state=42`)
6. Evaluasi model (classification report dan confusion matrix)
7. Menyimpan model terbaik dengan `pickle`

Fitur model: `Product`, `Region`, `Retailer`, `Sales Method`, `State`, `Price per Unit`.
`Units Sold` dan `Total Sales` tidak dipakai sebagai fitur untuk mencegah *data leakage*.

## Hasil Utama

- Total penjualan 2020 ≈ USD 1,82 juta dan 2021 ≈ USD 6,81 juta (naik sekitar 3,7 kali).
- Produk terlaris: *Men's Street Footwear*; wilayah tertinggi: West; retailer terbesar: West Gear.

| Model | Akurasi Training | Akurasi Testing |
| --- | --- | --- |
| Logistic Regression | 63% | 62% |
| Random Forest | 98% | 84% |
| Gradient Boosting | 85% | 83% |

Random Forest unggul tipis pada data uji tetapi mengalami *overfitting* (selisih training–testing 14 poin). **Gradient Boosting direkomendasikan** karena akurasinya hampir sama dan jauh lebih stabil.

## Struktur Repositori

```
.
├── kapita_selekta_UTS.ipynb                  # Notebook utama (Google Colab)
├── Nike_Dataset.csv                          # Dataset
├── Laporan_UTS_Analisis_Penjualan_Nike.docx  # Laporan lengkap
├── gambar/                                   # Gambar hasil visualisasi untuk laporan
└── README.md
```

## Cara Menjalankan

### Google Colab
1. Buka `kapita_selekta_UTS.ipynb` di Google Colab.
2. Unggah `Nike_Dataset.csv` ke sesi Colab, lalu sesuaikan path pada `pd.read_csv(...)` dengan nama berkas Anda.
3. Jalankan seluruh sel secara berurutan (*Runtime → Run all*).

### Lokal
```bash
git clone https://github.com/<username>/<nama-repo>.git
cd <nama-repo>
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook kapita_selekta_UTS.ipynb
```

## Catatan Penting

- Kolom `Invoice Date` berformat hari-bulan-tahun. Gunakan format eksplisit agar seluruh tanggal terbaca benar:
  ```python
  df['Invoice Date'] = pd.to_datetime(df['Invoice Date'], format='%d-%m-%Y')
  ```
- Hanya sekitar 20% baris yang nilai `Total Sales`-nya sama dengan `Price per Unit × Units Sold`, sehingga `Total Sales` tidak dipakai sebagai fitur.
- Kode penyimpanan model memilih model dengan akurasi uji tertinggi (Random Forest). Untuk menyimpan Gradient Boosting, ganti `best_model_obj = gbt_model`.

## Teknologi

Python · pandas · NumPy · Matplotlib · Seaborn · scikit-learn · Google Colab

## Referensi

- Kaggle — Nike U.S. Sales Dataset (2020–2021)
- Pedregosa, F. et al. (2011). *Scikit-learn: Machine Learning in Python.* JMLR 12, 2825–2830.
