# U3. Taktik Arsitektur

**BAB III: Taktik dan Pola (bagian taktik)**  
**Penanggung jawab:** [isi sesuai pembagian MID]

**Ketentuan dosen:** Minimal **satu taktik untuk setiap skenario** U2, dengan cara kerja dan **harga yang dibayar** (konsekuensi, biaya, atau trade-off).

| ID | Skenario U2 | Taktik yang dipilih | Cara kerja teknis | Harga yang dibayar / konsekuensi | Komponen di U5 |
|---|---|---|---|---|---|
| T-01 | SK-01 Availability |T-01 Health Check dan Pemulihan | Sistem melakukan pemeriksaan kesehatan layanan (health check) setiap 5 detik. Jika salah satu instans mengalami gangguan, API Gateway mengalihkan permintaan ke instans lain yang masih berjalan. | Membutuhkan server atau instans tambahan, biaya infrastruktur lebih besar, serta pemeliharaan dan pemantauan sistem yang lebih kompleks. |
| T-02 | SK-02 Functional Correctness | T-02 Sinkronisasi Status melalui RabbitMQ | Setiap transaksi peminjaman atau pengembalian buku akan menghasilkan event perubahan status. RabbitMQ meneruskan event tersebut ke Index Updater untuk memperbarui data pada Elasticsearch sehingga status buku di katalog ikut diperbarui. |Membutuhkan tambahan infrastruktur RabbitMQ, meningkatkan kompleksitas sistem, dan terdapat kemungkinan keterlambatan sinkronisasi antara data transaksi dan katalog.| [komponen] |
| T-03 | SK-03 Usability | [misalnya filter status dan petunjuk pencarian yang jelas; usulan] | [jelaskan] | [misalnya penambahan waktu desain dan pengujian antarmuka] | [komponen] |

## Hubungan dengan masalah utama
[Terangkan secara spesifik bagaimana T-02 dan taktik lain menyelesaikan masalah utama yang dipilih pada U1.]

## Pengecekan sebelum final
- [ ] Taktik minimal 3 (satu per skenario).
- [ ] Cara kerja tiap taktik jelas dan dapat ditunjukkan di diagram.
- [ ] Setiap taktik punya konsekuensi/biaya yang realistis.
