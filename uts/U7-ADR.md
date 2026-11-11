# U7. Architecture Decision Records (ADR)

**BAB V: Evaluasi dan ADR (bagian ADR)**  
**Penanggung jawab:** Wahyuningsih S

**Ketentuan dosen:** Minimal **2 ADR BARU**. Masing-masing ADR harus mempunyai **Title, Status, Context, Decision, dan Consequences**. Jangan hanya menyalin ADR dari Tugas Kelompok I.

## ADR-UTS-001

### Title
Penerapan Pola Publish–Subscribe Menggunakan RabbitMQ

### Status
Accepted (Diterima)

### Context
Informasi status ketersediaan buku pada katalog terkadang tidak sesuai dengan kondisi koleksi sebenarnya. Hal ini dapat menyebabkan mahasiswa memperoleh informasi yang kurang akurat mengenai ketersediaan buku. Sistem membutuhkan mekanisme pembaruan status secara otomatis setelah transaksi peminjaman atau pengembalian dilakukan.
### Decision
Menggunakan pola arsitektur Publish–Subscribe dengan RabbitMQ sebagai perantara pengiriman event. Circulation Service akan mengirimkan event StatusBukuBerubah setelah transaksi berhasil disimpan ke PostgreSQL. Selanjutnya, Index Updater menerima event tersebut untuk memperbarui indeks katalog pada Elasticsearch. Notification Service juga dapat menerima event untuk kebutuhan notifikasi.

### Consequences
**Dampak positif:** Pembaruan status buku dapat dilakukan secara otomatis, mengurangi ketidaksesuaian informasi katalog, dan mengurangi ketergantungan langsung antarlayanan.  
**Dampak negatif / harga yang dibayar:** Membutuhkan infrastruktur RabbitMQ tambahan, pemantauan antrean pesan, serta pengelolaan event yang lebih kompleks. Terdapat kemungkinan keterlambatan pembaruan status pada katalog.

---

## ADR-UTS-002

### Title
Penerapan Health Check dan Pemulihan Layanan

### Status
Accepted (Diterima)

### Context
Berdasarkan hasil wawancara dengan staf perpustakaan PUBA UMI, terdapat kendala pada server dan jaringan yang dapat menghambat akses layanan perpustakaan. Oleh karena itu, sistem membutuhkan mekanisme untuk mendeteksi gangguan layanan dan menjaga agar katalog serta layanan peminjaman tetap dapat digunakan.

### Decision
Menerapkan mekanisme Health Check melalui API Gateway dengan pemeriksaan kondisi layanan setiap 5 detik. Sistem menyediakan dua instans pada Catalog Service dan Circulation Service. Apabila salah satu instans mengalami gangguan, API Gateway akan mengalihkan permintaan ke instans lain yang masih berjalan. PostgreSQL juga dirancang menggunakan konfigurasi primer dan replika untuk mendukung ketersediaan data.

### Consequences
**Dampak positif:** Gangguan pada salah satu instans tidak langsung menghentikan seluruh layanan, sistem dapat mendeteksi masalah lebih cepat, dan ketersediaan layanan dapat ditingkatkan  
**Dampak negatif / harga yang dibayar:** Membutuhkan server tambahan, meningkatkan biaya infrastruktur, serta memerlukan pemantauan dan pemeliharaan yang lebih kompleks.


