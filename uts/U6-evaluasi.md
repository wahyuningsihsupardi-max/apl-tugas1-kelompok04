# U6. Evaluasi Arsitektur

**BAB V: Evaluasi dan ADR (bagian evaluasi)**  
**Penanggung jawab:** [isi sesuai pembagian MID]

**Ketentuan dosen:** pohon utilitas mini (3 skenario); trade-off **berbobot** dua alternatif; analisis ala ATAM: 2 risiko, 1 non-risiko, 1 sensitivity point, 1 trade-off point; keputusan apakah masalah utama teratasi.

## 6.1 Pohon utilitas mini — tiga skenario
```text
Utilitas Layanan Perpustakaan UMI
├── Availability → SK-01 → [prioritas/ukuran]
├── Functional Correctness → SK-02 → [prioritas/ukuran]
└── Usability → SK-03 → [prioritas/ukuran]
```

**Penilaian prioritas:** [jelaskan tingkat penting/risiko masing-masing skenario dan alasannya].

## 6.2 Dua alternatif rancangan yang dibandingkan
- **Alternatif A:** [contoh: pola berlapis dengan pembaruan status langsung setelah transaksi].
- **Alternatif B:** [contoh: pola berlapis dan publish–subscribe melalui antrean event].

**Kriteria dan bobot:** [tentukan bersama; jumlah bobot harus 100%].

| Kriteria | Bobot (%) | Skor A (mis. 1–5) | Bobot × A | Skor B (1–5) | Bobot × B | Dasar penilaian |
|---|---:|---:|---:|---:|---:|---|
| Ketepatan informasi | [isi] | [isi] | [isi] | [isi] | [isi] | [alasan] |
| Ketersediaan layanan | [isi] | [isi] | [isi] | [isi] | [isi] | [alasan] |
| Kemudahan pemeliharaan | [isi] | [isi] | [isi] | [isi] | [isi] | [alasan] |
| Kompleksitas implementasi | [isi] | [isi] | [isi] | [isi] | [isi] | [alasan] |
| **Jumlah** | **100** | — | **[total A]** | — | **[total B]** | — |

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
