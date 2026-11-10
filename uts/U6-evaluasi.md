# U6. Evaluasi Arsitektur

**BAB V: Evaluasi dan ADR (bagian evaluasi)**  
**Penanggung jawab:** Annisa Meyti Friska Manurung, 13020240294

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

| Kriteria (SK-02) | Bobot (%) | Skor A (mis. 1–5) | Bobot × A | Skor B (1–5) | Bobot × B | Dasar penilaian |
|---|---:|---:|---:|---:|---:|---|
| Ketepatan informasi (SK-01) | 40 | 3 | 120 | 4 | 160 | langsung, tetapi bila pembaruan indeks gagal setelah transaksi tersimpan, status tidak sama. B: event tidak hilang (1 transaksi), jeda ≤ 5 dtk. |
| Ketersediaan layanan | 25 | 2 | 50 | 5 | 125 | gangguan Catalog Service/Elasticsearch ikut menggagalkan peminjaman. B: peminjaman tetap jalan; event menunggu di RabbitMQ. |
| Kemudahan pemeliharaan | 15 | 3 | 45 | 4 | 60 | pelanggan baru (notifikasi, katalog FIKOM) ditambah tanpa mengubah Circulation Service. |
| Kompleksitas implementasi | 20 | 4 | 80 | 2 | 40 | tanpa jalur baru. B: RabbitMQ memikul jalur status, perlu Index Updater dan pemantauan. |
| **Jumlah** | **100** | — | **295 → 2,95** | — | **385 → 3,85** | — |

**Rumus skor berbobot:** `Σ(bobot_kriteria × skor_alternatif) / 100` jika bobot ditulis sebagai persen.  
**Alternatif terpilih dan alasannya:** **B** (3,85 dibanding 2,95). **Uji kepekaan:** jika bobot kompleksitas dinaikkan dan bobot lain diturunkan secara proporsional, A baru menyamai B ketika bobot kompleksitas mencapai **45%.** Jadi B tetap unggul selama kesederhanaan bukan pertimbangan yang hampir setengah dari keputusan. Harga yang dibayar adalah ketergantungan pada RabbitMQ, sehingga muncul Risiko 1 pada analisis ATAM.

## 6.3 Analisis ala ATAM
| Jenis temuan | Temuan spesifik rancangan | Dampak / tindak lanjut |
|---|---|---|
| Risiko 1 | RabbitMQ berjalan di satu container pada Server UMI (struktur alokasi B3, anggaran terbatas) sehingga menjadi titik gagal tunggal. Jika mati, event tertahan dan Elasticsearch tidak diperbarui selama gangguan. | SK-02 (≤ 5 dtk) gagal dan pengingat (R4) tertunda; event tidak hilang karena tersimpan bersama transaksi. Tindak lanjut: health check RabbitMQ dengan alarm ke Tim IT (T-01); bila jeda > 5 dtk, Catalog Service membaca status langsung dari Replika PostgreSQL; kluster RabbitMQ saat anggaran tersedia. |
| Risiko 2 | Status dan lokasi rak hanya berubah lewat transaksi di Panel Sirkulasi. Buku yang dibaca di tempat lalu diletakkan di rak lain, atau dipindah tanpa dicatat, tidak menghasilkan event. | Status benar tetapi lokasi rak salah (kasus R3); data FIKOM juga belum lengkap (R2). Tindak lanjut: rekonsiliasi rak, yaitu petugas memindai barkode satu rak dan sistem menerbitkan event koreksi; setiap rak dijadwalkan minimal sekali seminggu. |
| Non-risiko 1 | Penguncian per baris eksemplar (pessimistic locking) dalam transaksi ACID. | Hanya satu baris yang dikunci selama transaksi yang singkat. Satu buku fisik hanya bisa berada di satu meja, jadi perebutan kunci hampir tidak terjadi. Cukup diberi batas waktu kunci 3 dtk. |
| Sensitivity point 1 |  Interval pengambilan event dan jumlah konsumen pada Index Updater, serta refresh interval Elasticsearch. | SK-02 sangat peka terhadap angka ini. Dengan interval 1 dtk, jeda ≈ 2–3 dtk (memenuhi ≤ 5 dtk). Jika dinaikkan ke 10 dtk demi menghemat sumber daya, SK-02 langsung gagal. Nilainya ditetapkan di konfigurasi dan jedanya dipantau. |
| Trade-off point 1 | Pembaruan katalog secara asinkron melalui RabbitMQ (P-02). | Menaikkan ketersediaan (SK-01: peminjaman tidak bergantung pada katalog) dan kemudahan pemeliharaan, tetapi membuka jendela ketidaksesuaian ≤ 5 dtk pada ketepatan informasi (SK-02). Jendela ini diterima karena jauh lebih kecil daripada kondisi saat ini. |

## 6.4 Apakah masalah utama teratasi?
| Bukti Masalah | Solusi (pola/taktik) | Ukuran (SK-02) | Status |
|---|---|---|---|
| R3: buku tercantum di katalog tetapi tidak ada saat hendak dipinjam | P-02 + T-02: status dan event disimpan dalam 1 transaksi PostgreSQL, lalu Elasticsearch diperbarui | Status di Katalog Web ter-update ≤ 5 dtk; tidak ada peminjaman ganda | Teratasi (secara rancangan) |
| R4: informasi ketersediaan belum jelas; butuh pengingat | Notification Service sebagai pelanggan event; T-03 filter status| RPengingat terkirim setelah event | Teratasi (secara rancangan) |
| R3, R5: lokasi rak kurang rinci atau salah | Rekonsiliasi rak (bagian T-02) dan T-03 | ≥ 95 dari 100 sampel cocok (judul, status, rak) | Teratasi sebagian (Risiko 2) |

**Kesimpulan.** Secara argumentatif berdasarkan rancangan dan skenario, masalah utama **teratasi untuk semua perubahan status yang tercatat di sistem.** Dengan Single Source of Truth, P-02, dan transaksi ACID, status yang dilihat mahasiswa mengikuti transaksi di Panel Sirkulasi dalam ≤ 5 detik. Masalah **teratasi sebagian untuk lokasi rak**, karena perpindahan buku secara fisik tanpa pencatatan berada di luar jangkauan arsitektur dan hanya dapat ditekan lewat rekonsiliasi berkala (Risiko 2). Kesimpulan ini **masih berupa hipotesis:** belum ada pengujian nyata. Pembuktiannya direncanakan dengan uji 100 sampel buku setelah satu minggu uji coba, sesuai cara uji B2. Rancangan dinyatakan berhasil bila ≥ 95 sampel cocok dan jeda event selalu ≤ 5 detik.

## 6.5 Keterkaitan dengan ADR
Dua keputusan baru yang perlu dicatat: **ADR-UTS-001** memakai P-02 Publish–Subscribe dengan RabbitMQ dan Index Updater untuk sinkronisasi status (alternatif B); **ADR-UTS-002** memakai T-01, yaitu health check di API Gateway dan dua instans aktif untuk Catalog Service dan Circulation Service.
