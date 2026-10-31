# ADR-003: Penerapan Asynchronous Message Queue untuk Pengiriman Notifikasi Sirkulasi

* **Status:** Diterima
* **Tanggal:** 31-10-2026
* **Penulis:** Andi Tsabitha Hulwah

## Context
Sistem perpustakaan memerlukan pengiriman notifikasi otomatis terkait pengingat jatuh tempo peminjaman, keterlambatan, dan ketersediaan buku reservasi. Jika pemrosesan notifikasi dijalankan secara sinkron (*synchronous*) bersamaan dengan proses peminjaman/perpanjangan, kegagalan layanan email/WhatsApp atau lonjakan lalu lintas akan memperlambat alur transaksi utama dan membebani server (melanggar ASR-4 dan target ketersediaan layanan).

## Decision
Kami memutuskan untuk menerapkan pemrosesan notifikasi secara asinkron (*asynchronous background processing*) menggunakan antrean pesan (*Message Queue*) berbasis RabbitMQ / Redis Queue yang dikelola oleh *Notification Service*.

## Consequences
* **Keuntungan:** Memisahkan beban pemrosesan notifikasi dari transaksi sirkulasi utama, mencegah *bottleneck*, serta menjamin waktu pengiriman notifikasi < 5 detik tanpa mengganggu kinerja pencarian maupun peminjaman.
* **Kerugian:** Menambah kompleksitas infrastruktur operasional karena memerlukan pemantauan dan pengelolaan *worker/broker* antrean pesan secara terpisah.

## Compliance
Divalidasi melalui pengujian pengiriman notifikasi massal secara simultan dan pemantauan waktu antrean (*queue latency*) pada *dashboard* pengawasan server.
