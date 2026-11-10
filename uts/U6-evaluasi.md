# U6. Evaluasi Arsitektur

**BAB V: Evaluasi dan ADR (bagian evaluasi)**  
**Penanggung jawab:** [Annisa Meyti Friska Manurung, 13020240294]

**Ketentuan dosen:** pohon utilitas mini (3 skenario); trade-off **berbobot** dua alternatif; analisis ala ATAM: 2 risiko, 1 non-risiko, 1 sensitivity point, 1 trade-off point; keputusan apakah masalah utama teratasi.

## 6.1 Pohon utilitas mini — tiga skenario
```text
Utilitas Layanan Perpustakaan UMI
├── Availability → SK-01 → Ketersediaan ≥ 99% selama 7 hari
├── Functional Correctness → SK-02 → status dikatalog ter update ≤ 5 detik, ≥ 95 dari 100 sampel cocok
└── Usability → SK-03 → ≥ 9 dari 10 mahasiswa berhasil mencari ≤ 2 menit tanpa bantuan
```

**Penilaian prioritas:** [jelaskan tingkat penting/risiko masing-masing skenario dan alasannya].

## 6.2 Dua alternatif rancangan yang dibandingkan
- **Alternatif A:** Layered dengan pembaruan status langsung: dalam satu permintaan, Circulation Service menyimpan transaksi ke PostgreSQL lalu langsung memanggil Catalog Service untuk memperbarui Elasticsearch.
- **Alternatif B:** Layered + Publish–Subscribe melalui RabbitMQ
  Bobot disepakati dari prioritas U1. Ketepatan informasi diberi bobot terbesar karena merupakan masalah utama, sedangkan kompleksitas tetap diberi bobot karena anggaran terbatas. Skor 1 (buruk) sampai 5 (sangat baik); untuk kompleksitas, skor tinggi berarti lebih sederhana. Rumus: Σ(bobot × skor)/100.

**Kriteria dan bobot:** [tentukan bersama; jumlah bobot harus 100%].

| Kriteria | Bobot (%) | Skor A (mis. 1–5) | Bobot × A | Skor B (1–5) | Bobot × B | Dasar penilaian |
|---|---:|---:|---:|---:|---:|---|
| Ketepatan informasi | [40 | 3 | 120 | 4 | 160 | langsung, tetapi bila pembaruan indeks gagal setelah transaksi tersimpan, status tidak sama. B: event tidak hilang (1 transaksi), jeda ≤ 5 dtk. |
| Ketersediaan layanan | 25 | 2 | 50 | 5 | 125 | gangguan Catalog Service/Elasticsearch ikut menggagalkan peminjaman. B: peminjaman tetap jalan; event menunggu di RabbitMQ. |
| Kemudahan pemeliharaan | 15 | 3 | 45 | 4 | 60 | pelanggan baru (notifikasi, katalog FIKOM) ditambah tanpa mengubah Circulation Service. |
| Kompleksitas implementasi | 20 | 4 | 80 | 2 | 40 | tanpa jalur baru. B: RabbitMQ memikul jalur status, perlu Index Updater dan pemantauan. |
| **Jumlah** | **100** | — | **295 -> 2,95** | — | **385 -> 3,85** | — |

**Rumus skor berbobot:** `Σ(bobot_kriteria × skor_alternatif) / 100` jika bobot ditulis sebagai persen.  
**Alternatif terpilih dan alasannya:** [isi].

## 6.3 Analisis ala ATAM
| Jenis temuan | Temuan spesifik rancangan | Dampak / tindak lanjut |
|---|---|---|
| Risiko 1 | [isi] | [isi] |
| Risiko 2 | [isi] | [isi] |
| Non-risiko 1 | [isi keputusan dengan konsekuensi terkendali] | [isi] |
| Sensitivity point 1 | [parameter yang sangat memengaruhi atribut kualitas, misalnya jeda pembaruan] | [isi] |
| Trade-off point 1 | [parameter/keputusan yang memengaruhi lebih dari satu kualitas] | [isi] |

## 6.4 Apakah masalah utama teratasi?
[Evaluasi *secara argumentatif berdasarkan rancangan dan skenario*. Nyatakan bila masih berupa hipotesis dan belum dilakukan pengujian nyata. Jelaskan batasan dan risiko tersisa.]

## 6.5 Keterkaitan dengan ADR
[Ringkas dua keputusan baru yang harus didokumentasikan pada U7.]
