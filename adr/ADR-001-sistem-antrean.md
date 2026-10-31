# ADR-001: Penerapan Database Indexing dan In-Memory Caching untuk Pencarian Katalog

* **Status:** Accepted
* **Tanggal:** 31-10-2026
* **Penulis:** Andi Tsabitha Hulwah

## Context
Sistem pencarian katalog perpustakaan mengalami lonjakan akses pada jam sibuk perkuliahan. Waktu tanggap pencarian tanpa indeks dan cache dapat melebihi 5 detik, yang melanggar ASR-1 (pencarian katalog harus tampil < 2 detik).

## Decision
Kami memutuskan untuk menerapkan indeks pada tabel data bibliografi di PostgreSQL dan menambahkan Redis sebagai *in-memory cache* untuk hasil pencarian katalog yang sering diakses.

## Consequences
* **Keuntungan:** Waktu tanggap pencarian katalog turun secara signifikan menjadi di bawah 2 detik pada jam puncak.
* **Kerugian:** Membutuhkan alokasi memori tambahan untuk server Redis serta mekanisme validasi untuk *cache invalidation* saat ada pembaruan data buku.

## Compliance
Ditinjau melalui pengujian beban (*load testing*) k6/JMeter sebelum rilis dan dipantau menggunakan *metrics Dashboard*.
