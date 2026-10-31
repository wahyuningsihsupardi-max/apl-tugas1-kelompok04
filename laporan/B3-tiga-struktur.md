# B3. Tiga Struktur Desain

**Studi Kasus:** Student Learning Platform (Subsistem II-4) — Layanan Perpustakaan

**Penanggung Jawab :** Annisa Meyti Friska Manurung

**Ruang lingkup:** pencarian katalog, peminjaman, perpanjangan, dan akses koleksi digital.

_**Materi acuan:** Pertemuan 6 — Karakteristik Arsitektur & Atribut Kualitas_

---

## 5. Tiga Struktur Desain Arsitektur

Arsitektur dapat dilihat dari tiga sudut pandang (Bass dkk., 2021). Setiap sudut pandang membantu menjawab pertanyaan yang berbeda.

| Struktur | Elemen | Menjawab | Contoh pada Sistem Ini |
|---|---|---|---|
| Struktur modul (module) | Modul: unit kode dengan tanggung jawab tertentu | Bagaimana kode dibagi dan saling memakai? | Dekomposisi, uses, layers, class |
| Struktur komponen & konektor (C&C) | Komponen yang berjalan dan konektor yang menghubungkannya | Bagaimana sistem bekerja saat berjalan? | Proses, concurrency, shared data, client-server |
| Struktur alokasi (allocation) | Perangkat lunak dan elemen lingkungannya (perangkat keras, tim) | Di mana dan oleh siapa perangkat lunak ditempatkan dan dibuat? | Deployment, implementation, work assignment |

### 5.1 Struktur Modul

| Lapisan | Modul | Tanggung Jawab |
|---|---|---|
| Presentation | Katalog Web (responsif) | Antarmuka pencarian, halaman detail koleksi, dan halaman akun bagi mahasiswa dan dosen |
| Presentation | Panel Sirkulasi | Antarmuka bagi pustakawan: peminjaman, pengembalian, perpanjangan, denda, pengelolaan bibliografi |
| Presentation | Pembaca Koleksi Digital | Penampil berkas digital dengan kontrol akses dan pembatasan pengunduhan |
| Business logic | Catalog Service | Logika pencarian, penyaringan hasil, dan pengelolaan data bibliografi |
| Business logic | Circulation Service | Aturan peminjaman, perpanjangan, reservasi, dan perhitungan denda |
| Business logic | Digital Collection Service | Penyajian koleksi digital, penerbitan tautan berbatas waktu, dan pencatatan pemanfaatan |
| Business logic | Notification Service | Penyusunan dan pengiriman notifikasi jatuh tempo, keterlambatan, dan ketersediaan reservasi |
| Business logic | Auth Service | Autentikasi dan otorisasi berbasis peran serta status keanggotaan |
| Data access | Repository | Akses data ke basis data: bibliografi, salinan, anggota, transaksi, dan aturan sirkulasi |
| Integration | SIAKAD Connector | Sinkronisasi status keanggotaan mahasiswa dan dosen |
| Integration | Metadata Harvester | Panen dan penyediaan metadata melalui OAI-PMH, termasuk pemetaan ke format MARC |
| Integration | Content Provider Connector | Integrasi dengan penyedia basis data jurnal dan e-book berlisensi |

Struktur modul pada sistem ini berkaitan erat dengan kemudahan perubahan: aturan sirkulasi dan kebijakan denda terkonsentrasi pada Circulation Service, sehingga perubahan kebijakan perpustakaan tidak menyebar ke seluruh kode.

### 5.2 Struktur Komponen & Konektor

| Komponen | Jenis | Konektor | Peran |
|---|---|---|---|
| Katalog Web | Client | REST API (HTTPS) | Kanal mahasiswa dan dosen untuk mencari dan mengakses koleksi |
| Panel Sirkulasi | Client | REST API (HTTPS) | Kanal pustakawan untuk melayani transaksi di meja perpustakaan |
| API Gateway | Konektor | REST API | Pintu masuk tunggal: perutean, pembatasan laju permintaan, dan pemeriksaan autentikasi |
| Catalog Service | Service | REST API | Pencarian dan pengelolaan data bibliografi |
| Circulation Service | Service | REST API | Transaksi peminjaman, perpanjangan, reservasi, dan denda |
| Digital Collection Service | Service | REST API | Penyajian koleksi digital dan penerbitan tautan berbatas waktu |
| Notification Service | Service | Message Queue (RabbitMQ) | Pengiriman notifikasi secara asinkron |
| Auth Service | Service | REST API | Autentikasi dan otorisasi berbasis peran |
| Elasticsearch | Data store | REST API | Indeks pencarian katalog |
| PostgreSQL | Data store | TCP/IP | Data bibliografi, salinan, anggota, transaksi, dan aturan sirkulasi |
| Object Storage + CDN | Data store | HTTPS | Penyimpanan dan penyajian berkas koleksi digital |
| SIAKAD | Sistem eksternal | REST API / Basis data | Sumber data status keanggotaan |
| Katalog Eksternal | Sistem eksternal | OAI-PMH | Pertukaran metadata bibliografi |
| Penyedia Koleksi Digital | Sistem eksternal | HTTPS / API | Sumber jurnal dan e-book berlisensi |

Struktur C&C pada sistem ini paling berkaitan dengan kinerja dan ketersediaan: pemisahan modul katalog dari modul sirkulasi memastikan lonjakan pencarian pada musim ujian tidak mengganggu transaksi peminjaman di meja perpustakaan.

### 5.3 Struktur Alokasi

| Komponen | Penempatan | Penanggung Jawab |
|---|---|---|
| Katalog Web | Server UMI (Nginx) | Tim Web Developer |
| Panel Sirkulasi | Komputer meja layanan perpustakaan dan server UMI | Tim Web Developer bersama staf perpustakaan |
| API Gateway | Server UMI (container) | Tim Backend Developer |
| Catalog Service | Server UMI (container) | Tim Backend Developer |
| Circulation Service | Server UMI (container) | Tim Backend Developer |
| Digital Collection Service | Server UMI (container) | Tim Backend Developer |
| Notification Service | Server UMI (container) | Tim Backend Developer |
| Auth Service | Server UMI (container) | Tim Backend Developer |
| Elasticsearch | Server UMI (kluster kecil) | Tim Backend Developer |
| PostgreSQL | Server UMI (primer + replika) | Tim DBA / IT UMI |
| Object Storage + CDN | Server UMI dan jaringan distribusi konten | Tim IT UMI |
| SIAKAD | Server UMI (eksisting) | Tim IT UMI |
| Katalog Eksternal & Penyedia Koleksi Digital | Luar kampus (mitra) | Pihak mitra dengan pendampingan Tim IT UMI |

Struktur alokasi pada sistem ini berkaitan dengan penempatan dan pembagian kerja: seluruh layanan ditempatkan pada infrastruktur kampus agar sesuai batasan teknologi dan operasional yang telah ditetapkan pada Bagian A, dengan pengecualian penyajian berkas digital yang memanfaatkan CDN untuk menekan beban bandwidth jaringan kampus.

