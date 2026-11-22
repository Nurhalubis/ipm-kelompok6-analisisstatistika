# Connected, Educated, Developed?

**Pemodelan IPM 513 Kabupaten/Kota di Indonesia**

Proyek akhir mata kuliah STA2511 Analisis Statistika · Kelompok 6 · Program Studi Magister Statistika dan Sains Data, Sekolah Sains Data, Matematika, dan Informatika (SSMI), IPB University

Dosen pengampu: Dr. Ir. Indahwati, M.Si

Anggota: Nurhalimah · Arthur · Rifdah · Meliawati
Tautan proyek: <https://ipb.link/ipm-kelompok6> · Repositori: <https://github.com/Nurhalubis/ipm-kelompok6-analisisstatistika> 
---

## Ringkasan

Kami memodelkan Indeks Pembangunan Manusia (IPM) 2025 pada 513 kabupaten/kota terhadap akses internet, sanitasi layak, dan pendidikan SMA ke atas. Modelnya regresi linear berganda, ditambah uji efek interaksi dan kendali wilayah.

| Pertanyaan | Jawaban |
|---|---|
| **Connected?** | Ya. Akses internet berhubungan positif dengan IPM (+1,26 poin per 10 poin persen pada model akhir). |
| **Educated?** | Terutama. Pendidikan SMA+ adalah prediktor terkuat (+2,05 poin per 10 poin persen). Interaksi internet × pendidikan hilang setelah wilayah dikendalikan, jadi itu efek komposisi, bukan sinergi. |
| **Developed?** | Belum merata. Pada indikator yang setara, IPM Papua 6,5 poin di bawah Jawa. |

Model akhir (tiga indikator + 7 wilayah pulau + status kota) menjelaskan 89% variasi IPM (R² = 0,891; R² terkoreksi = 0,889). Hasil ini adalah asosiasi statistik, bukan hubungan sebab-akibat.

## Isi repositori

```
.
├── laporan_IPM_kelompok6.qmd      kode analisis lengkap (Quarto + R)
├── gaya.css                        gaya tampilan HTML
├── data/
│   ├── 20261023_Data_Kelompok_6_New_513kako.csv   data mentah 513 kab/kota
│   └── Data_Analisis_IPM_Kelompok6_STA2511.xlsx   buku kerja: kamus data, data + dugaan/sisaan C1,
│                                                   ringkasan, model, sisaan ekstrem, perbandingan model
├── peta/                           batas wilayah GeoJSON (lihat peta/README.md)
├── output/
│   ├── laporan_IPM_kelompok6.html  hasil render laporan analisis
│   ├── koefisien_per_pulau.csv     koefisien per +10 poin persen, nasional & per pulau
│   └── gambar/                     grafik, peta, dan ringkasan visual
├── laporan/                        laporan format Produk Multimedia IPB (.docx)
└── poster/                         poster infografis A3 dan kode QR (utama: ipb.link)
```

## Data

Sumber: Badan Pusat Statistik (BPS). Publikasi *Indeks Pembangunan Manusia 2025* dan *Survei Sosial Ekonomi Nasional (Susenas) Maret 2025*.

| Kolom | Nama di kode | Keterangan |
|---|---|---|
| Provinsi | `prov` | Nama provinsi (38 provinsi) |
| Kode Kabupaten/Kota | `kode` | Kode wilayah BPS 4 digit |
| Kabupaten/Kota | `kab` | Nama kabupaten/kota (415 kabupaten, 98 kota) |
| IPM (Y) | `Y` | Indeks Pembangunan Manusia 2025 |
| X1 | `X1` | % penduduk berumur 5 tahun ke atas yang mengakses internet dalam tiga bulan terakhir |
| X2 | `X2` | % rumah tangga yang memiliki akses terhadap sanitasi layak |
| X3 | `X3` | % penduduk berumur 25 tahun ke atas dengan pendidikan SMA ke atas |

Buku kerja Excel memuat kolom tambahan: `Wilayah Pulau`, `Status Kota`, `Papua`, serta `Dugaan IPM (Model C1)` dan `Sisaan` yang dihitung dengan formula dari koefisien model akhir.

Catatan: Kabupaten Puncak (Papua Tengah) tidak tersedia dalam sumber data sehingga n = 513. Kolom X1 tersimpan sebagai teks di CSV dan diubah menjadi numerik di dalam kode.

## Cara menjalankan

1. Pasang R (≥ 4.4) dan [Quarto](https://quarto.org).
2. Pasang paket:
   ```r
   install.packages(c("car", "sandwich", "lmtest", "MASS", "dplyr", "ggplot2",
                      "lme4", "sf", "spdep", "spatialreg", "knitr", "gridExtra"))
   ```
3. Siapkan berkas peta sesuai `peta/README.md`.
4. Dari folder repositori ini, jalankan:
   ```bash
   quarto render laporan_IPM_kelompok6.qmd
   ```

## Metode singkat

1. Model aditif OLS dan VIF
2. Pemusatan prediktor, lalu uji interaksi X1X2, X1X3, X2X3 (uji F parsial, SE HC3)
3. Kendali 7 wilayah pulau (rujukan: Jawa) dan status kota; pemilihan model dengan AIC, BIC, dan RMSE validasi silang 10-fold × 5
4. Diagnostik: Shapiro-Wilk, Breusch-Pagan, RESET, Moran's I (6 tetangga terdekat)
5. Uji ketahanan: SE tergerombol per provinsi, regresi robust Huber, model multilevel, Spatial Error Model

## Lisensi dan atribusi

Data © BPS. Batas wilayah dari [mashadinoor/Indonesia-Geojson-2026](https://github.com/mashadinoor/Indonesia-Geojson-2026) (MIT) dan [eppofahmi/geojson-indonesia](https://github.com/eppofahmi/geojson-indonesia).
