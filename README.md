<div align="center">
  <h1>pindar-risk-atlas</h1>
  <p><strong>Peta risiko gagal bayar pinjaman online berizin (TWP90) antarprovinsi di Indonesia</strong></p>

  <p align="center">
    <img src="https://img.shields.io/badge/Language-Python_3-3776AB?style=flat-square&logo=python&logoColor=white" alt="Language">
    <img src="https://img.shields.io/badge/Data-OJK_%7C_BPS_%7C_Play_Store-blue?style=flat-square" alt="Data sources">
    <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="License">
    <img src="https://img.shields.io/badge/Status-Analysis_complete-success?style=flat-square" alt="Status">
  </p>

  <p align="center">
    Satu notebook yang mengambil statistik layanan pendanaan bersama berbasis teknologi informasi (LPBBTI atau pindar) dari OJK, indikator sosial-ekonomi dari BPS, serta data aplikasi dan penindakan, lalu membersihkannya, memeriksanya, dan menganalisis apa yang berasosiasi dengan TWP90 di 38 provinsi.
  </p>
</div>

## Tech Stack

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black)

## Ringkasan

TWP90 adalah porsi pinjaman yang menunggak lebih dari 90 hari terhadap total outstanding. Pada tingkat provinsi, repositori ini menghitungnya sebagai:

$$
\text{TWP90} = \frac{\text{Macet}}{\text{Outstanding}} \times 100
$$

dari tabel kualitas pembiayaan OJK. Panel akhir berisi 38 provinsi dan 40 bulan (2023-01 sampai 2026-04); empat provinsi Papua baru hanya memiliki 22 bulan. Seluruh pekerjaan ada di satu notebook, `notebooks/pindar_risk_atlas.ipynb`, yang berjalan dari akuisisi data mentah sampai peta risiko provinsi. Alurnya digambar sebagai flowchart di awal notebook.

## Temuan utama

- **TWP90 adalah ciri provinsi yang cukup stabil.** 63% variansnya antarprovinsi.
- **Provinsi dengan pasar lebih besar cenderung punya TWP90 lebih tinggi.** Spearman antara outstanding dan TWP90 pada 38 provinsi adalah 0,75 (CI bootstrap 95% 0,48 sampai 0,90). Hubungan ini tidak muncul di dalam provinsi dari waktu ke waktu (korelasi within-province sekitar 0,10), jadi ini perbedaan antarprovinsi yang stabil dan bukan bukti bahwa pertumbuhan pasar menaikkan gagal bayar.
- **Kemiskinan berasosiasi negatif dengan TWP90** (Spearman -0,43), bukan nol: provinsi lebih miskin cenderung punya TWP90 lebih rendah.
- **Dua kelompok kasar dengan batas lembek.** Dari clustering tanpa TWP90 sebagai fitur, 27 provinsi memiliki TWP90 median 2,20% dan 10 provinsi timur 1,13%. Silhouette 0,31 dan stabilitas 0,55 (ARI) menunjukkan gradien, dengan tujuh provinsi di perbatasan.
- **Hipotesis "portofolio muda" (H1) tidak terbukti sebagai penjelasan umum.** Koefisien pertumbuhan outstanding negatif tetapi tidak signifikan dan hilang saat empat provinsi Papua baru disertakan.
- **Lonjakan Jakarta pada 2025-11 dan 2025-12 adalah satu peristiwa di segmen syariah**, bukan artefak pelaporan: macet syariah Jakarta naik dari 107 ke 1.333 miliar rupiah sementara segmen konvensional datar.
- **Kolom turunan tabel OJK 144 tidak konsisten antarbulan** (jumlah antarprovinsi 73,39 pada 2025-03, 79,05 pada 2025-04, dan 0,846 pada 2025-05, padahal bulan lain 2,3 sampai 3,3), sehingga tidak dipakai.
- **Peta risiko provinsi (bagian J).** Median TWP90 12 bulan terakhir dibagi tiga: 22 provinsi hijau (di bawah 2%), 11 kuning, dan 5 merah (NTB, Jawa Timur, DKI Jakarta, Jawa Barat, Kalimantan Utara). Empat provinsi (Gorontalo, Bali, Jawa Timur, Sulawesi Utara) diberi penanda kenaikan cepat. Hijau tidak berarti aman, karena banyak provinsi hijau berportofolio kecil dan muda. Ambang 2% dan 3% adalah pilihan penulis; ambang 1,5% dan 2,5% mengubah label 15 provinsi.

Hubungan di atas berlaku di tingkat provinsi (n=38) dan bersifat asosiasi, bukan sebab-akibat. Daftar hipotesis yang belum diuji dan batasan lengkap ada di bagian F notebook.

## Struktur repositori

- `notebooks/pindar_risk_atlas.ipynb`: notebook tunggal (bagian 0 dan A sampai J).
- `data/raw/`: data mentah hasil akuisisi, dibagi per sumber (`ojk_xlsx`, `ojk_sjkpublic`, `ojk_directory`, `bps`, `playstore`), ditambah `geo` (batas 38 provinsi untuk peta, lengkap dengan `SOURCE.md`).
- `data/clean/`: delapan CSV hasil pembersihan, termasuk `master_pindar_panel_clean.csv`.
- `artifacts/`: keluaran bagian G dan J (tabel cluster, deret TWP90 nasional, grafik pencar, label risiko per provinsi, peta PNG, dan peta HTML interaktif); terisi saat notebook dijalankan.
- `LICENSE`: lisensi MIT.

## Cara menjalankan

Instal paket yang dipakai notebook:

```bash
pip install pandas numpy scipy scikit-learn matplotlib openpyxl
```

Buka `notebooks/pindar_risk_atlas.ipynb` di Jupyter atau VS Code, lalu jalankan semua sel (Run All) di kernel baru. Notebook bisa dibuka dari folder `notebooks/` maupun dari akar repositori. Pengujian lokal memakai Python 3.14, pandas 3.0, NumPy 2.5, SciPy 1.18, scikit-learn 1.9, Matplotlib 3.11, dan openpyxl 3.1; Run All memakan 1 sampai 4 menit pada pengujian lokal (60 sampai 260 detik tergantung beban mesin; sel bootstrap dan uji kestabilan cluster paling lama).

Catatan penting:

- Notebook disimpan tanpa output. Menjalankannya menulis ulang `data/clean/` dan mengisi `artifacts/`. Master panel hasil jalan baru berisi 67 kolom; dua kolom penanda outlier dari versi awal sengaja dibuang.
- Default `RUN_ACQUISITION = False`, sehingga bagian A membaca `data/raw/` dan tidak memakai jaringan. Untuk mengambil file yang hilang, set `RUN_ACQUISITION = True` dan pasang paket tambahan:

```bash
pip install requests beautifulsoup4 lxml curl_cffi pdfplumber google-play-scraper
```

  `FORCE_REFRESH = True` mengunduh ulang semua file. Scraper BPS memakai `BPS_NEXT_ACTION_ID` yang tertanam di kode dan akan gagal jika BPS mengganti build situsnya.
- Pada pengujian, sel-sel dijalankan berurutan lewat skrip. Run All di kernel Jupyter belum diuji.

## Data

| Sumber | Isi | Cakupan |
|---|---|---|
| OJK SJKPublic | 18 tabel LPBBTI per provinsi (konvensional dan syariah), termasuk kualitas pembiayaan | 2024-07 sampai 2026-04 (beberapa tabel sampai 2026-08) |
| OJK XLSX historis | 18 workbook statistik LPBBTI, sheet 9 per provinsi | 2022 sampai 2025 |
| OJK Direktori | 94 penyelenggara berizin (PDF Mei 2026) | snapshot |
| BPS | 10 tabel sosial-ekonomi provinsi dan UMP Refnusa | 2023 sampai 2026, berbeda per indikator |
| Play Store | aplikasi dan ulasan terbaru penyelenggara berizin | ulasan terbaru saja |
| Satgas PASTI | daftar pinjol ilegal (PDF Oktober 2026) dan siaran pers | siaran pers sampai 2025-11 |
| Batas provinsi | GeoJSON 38 provinsi dari repositori pihak ketiga (CC BY 4.0) | statis |

Variabel BPS yang belum terbit untuk tahun terakhir diisi maju (forward-fill) per provinsi dan diberi flag `bps_ffill_annual_flag`; UMP Papua Pegunungan 2025 dipinjam dari Papua dan diberi flag `ump_imputed_flag`.

## Alur notebook

1. **Bagian 0 dan A, akuisisi.** Setup, saklar akuisisi, dan pengambilan data mentah dari OJK, BPS, Play Store, dan Satgas PASTI, ditutup dengan kontrak jumlah file.
2. **Bagian B, wrangling.** Harmonisasi 38 provinsi, penurunan TWP90 dari kualitas pembiayaan, penyambungan XLSX dengan SJKPublic, pembersihan BPS, dan master panel.
3. **Bagian C, audit dan EDA.** Tren TWP90, outlier level dan lonjakan, serta porsi varians antarprovinsi.
4. **Bagian D, korelasi.** Spearman antarprovinsi dengan CI bootstrap, korelasi parsial, dan korelasi within-province.
5. **Bagian E, clustering.** K-means tanpa TWP90 sebagai fitur, pemilihan k dengan silhouette dan kestabilan subsampel.
6. **Bagian F dan G, interpretasi dan ekspor.** Kesimpulan yang ditopang data, hipotesis, batasan, dan penyimpanan hasil.
7. **Bagian H, validasi independen.** Rekonsiliasi dengan total nasional resmi, penurunan ulang dari tabel mentah, forensik lonjakan, dan uji sensitivitas.
8. **Bagian I, uji H1.** Regresi panel dengan efek tetap provinsi dan bulan dan wild cluster bootstrap.
9. **Bagian J, peta risiko.** Label hijau, kuning, dan merah per provinsi, ditampilkan sebagai gambar statis dan HTML interaktif mandiri.

## Batasan

- Hanya 38 provinsi, sehingga interval kepercayaan lebar dan struktur cluster lemah.
- Analisis ekologis: hubungan berlaku di tingkat provinsi, bukan peminjam.
- Variabel sosial-ekonomi hampir statis, dan IPM 2025 dan 2026 serta kemiskinan dan PDRB 2026 adalah salinan tahun sebelumnya.
- TWP90 2024-07 sampai 2024-10 dari SJKPublic berbeda dari workbook XLSX resmi (outstanding 1,1 sampai 1,8% lebih rendah), dan penyebabnya belum dapat dijelaskan.
- Tidak ada desain identifikasi, jadi tidak ada klaim sebab-akibat.

## Lisensi

Dirilis di bawah lisensi MIT. Lihat berkas [LICENSE](LICENSE). Batas provinsi di `data/raw/geo/` berlisensi CC BY 4.0 (atribusi: "Indonesia GeoJSON & TopoJSON Maps (38 Provinces)" oleh denyherianto); asal-usul batasnya tidak sepenuhnya jelas, lihat `data/raw/geo/SOURCE.md`.
