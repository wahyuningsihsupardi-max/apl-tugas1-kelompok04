# BAB II

# SKENARIO ATRIBUT KUALITAS
**Penanggung Jawab:** Besse Armelya Cindy Damayanti M  

## 2.1 Dasar Penyusunan Skenario

Skenario atribut kualitas digunakan untuk menggambarkan bagaimana sistem diharapkan merespons suatu kondisi dan bagaimana respons tersebut diukur [1].

Skenario disusun berdasarkan tiga karakteristik pendorong yang telah ditetapkan pada BAB I. Setiap skenario terdiri atas enam komponen, yaitu:

1. Source of Stimulus: Sumber yang menimbulkan peristiwa.

2. Stimulus: Peristiwa atau kondisi yang harus direspons sistem.

3. Environment: Kondisi lingkungan ketika peristiwa terjadi.

4. Artifact: Komponen sistem yang terpengaruh.

5. Response: Tindakan sistem dalam menangani peristiwa.

6. Response Measure: Ukuran keberhasilan respons dalam bentuk angka.

Ketiga skenario diberi kode SK-01, SK-02, dan SK-03 agar konsisten dengan taktik T-01, T-02, dan T-03 yang digunakan dalam rancangan arsitektur kelompok.

## 2.2 SK-01 — Availability (Ketersediaan Layanan)

Skenario SK-01 menggambarkan kemampuan sistem mempertahankan layanan katalog dan peminjaman ketika salah satu instans layanan mengalami gangguan.

**Tabel 2.1 Skenario Availability**

| Komponen | Deskripsi |
| --- | --- |
| Source of Stimulus | Gangguan atau kegagalan pada salah satu instans layanan. |
| Stimulus | Salah satu instans Catalog Service atau Circulation Service berhenti merespons permintaan. |
| Environment | Sistem berada dalam kondisi operasional ketika mahasiswa mengakses katalog atau staf melakukan transaksi perpustakaan. |
| Artifact | API Gateway, Catalog Service, Circulation Service, serta infrastruktur PostgreSQL. |
| Response | API Gateway melakukan health check dan mengalihkan permintaan menuju instans layanan yang sehat. Rancangan menggunakan dua instans pada layanan utama serta PostgreSQL primer dan replika sebagai bagian dari strategi ketersediaan. |
| Response Measure | Ketersediaan layanan minimal 99% selama tujuh hari pengamatan dengan pemeriksaan setiap satu menit. Health check instans dilakukan setiap 5 detik. |

Pengujian dilakukan dengan memantau respons layanan secara berkala dan mencatat jumlah pemeriksaan yang berhasil maupun gagal.

**Rumus:**

Availability (%) = (Jumlah pemeriksaan berhasil / Jumlah seluruh pemeriksaan) × 100%

Target tersebut merupakan ukuran keberhasilan yang diusulkan. Penggunaan health check dan instans cadangan mendukung T-01 Health Check dan Pemulihan, sebagaimana direncanakan pada arsitektur kelompok.

## 2.3 SK-02 — Functional Correctness (Ketepatan Informasi Koleksi)

SK-02 merupakan skenario utama karena secara langsung menjawab masalah ketidaksesuaian status ketersediaan buku pada katalog dengan transaksi yang tercatat.

**Tabel 2.2 Skenario Functional Correctness**

| Komponen | Deskripsi |
| --- | --- |
| Source of Stimulus | Staf Perpustakaan PUBA UMI melalui Panel Sirkulasi. |
| Stimulus | Petugas berhasil mencatat transaksi peminjaman atau pengembalian yang menyebabkan perubahan status sebuah eksemplar buku. |
| Environment | Sistem berada dalam kondisi operasional. Transaksi dikelola melalui Circulation Service dan disimpan pada PostgreSQL, sedangkan informasi katalog dibaca melalui indeks Elasticsearch. |
| Artifact | Circulation Service, Repository, PostgreSQL, RabbitMQ, Index Updater, Elasticsearch, dan Catalog Service. |
| Response | Circulation Service menyimpan perubahan transaksi dan catatan event secara konsisten. Event StatusBukuBerubah kemudian diteruskan melalui RabbitMQ. Index Updater menerima event untuk memperbarui status buku pada Elasticsearch agar informasi terbaru dapat ditampilkan melalui Katalog Web. |
| Response Measure | Perubahan status ditargetkan tampil pada katalog maksimal 5 detik setelah transaksi berhasil dicatat. Dari 100 sampel informasi buku yang diuji, minimal 95 harus sesuai dengan data acuan yang dikonfirmasi petugas. |

Pada skenario ini, PostgreSQL digunakan sebagai sumber utama data transaksi, sedangkan Elasticsearch berperan sebagai indeks pencarian. Perubahan status disebarkan menggunakan mekanisme Publish–Subscribe, sehingga Circulation Service tidak perlu memperbarui seluruh komponen penerima secara langsung.

Untuk mengurangi risiko kehilangan event, rancangan menggunakan pendekatan transactional outbox, yaitu menyimpan perubahan transaksi dan catatan event dalam satu transaksi basis data sebelum event diteruskan ke RabbitMQ [6].

Pengukuran ketepatan informasi dilakukan dengan membandingkan judul, status ketersediaan, dan lokasi rak pada katalog dengan data acuan yang telah dikonfirmasi petugas.

**Rumus:**

Functional Correctness (%) = (Jumlah sampel informasi sesuai / Jumlah seluruh sampel diperiksa) × 100%

Selain ketepatan informasi, waktu sinkronisasi diukur sejak transaksi berhasil dicatat hingga perubahan status dapat dibaca melalui katalog.

Skenario SK-02 menjadi dasar penerapan T-02 Sinkronisasi Status melalui RabbitMQ serta pola P-02 Publish–Subscribe pada BAB III.

## 2.4 SK-03 — Usability (Kemudahan Penggunaan)

Skenario SK-03 menggambarkan kemampuan pengguna dalam mencari informasi koleksi secara mandiri melalui Katalog Web.

**Tabel 2.3 Skenario Usability**

| Komponen | Deskripsi |
| --- | --- |
| Source of Stimulus | Mahasiswa atau dosen sebagai pengguna layanan katalog. |
| Stimulus | Pengguna ingin menemukan sebuah buku beserta status ketersediaan dan lokasi raknya. |
| Environment | Pengguna melakukan pencarian pada prototipe Katalog Web melalui skenario pengujian tanpa bantuan petugas. |
| Artifact | Katalog Web, API Gateway, Catalog Service, dan Elasticsearch. |
| Response | Sistem menyediakan kolom pencarian, filter status ketersediaan, serta informasi lokasi rak yang mudah ditemukan dan dipahami. Hasil pencarian ditampilkan melalui Katalog Web. |
| Response Measure | Minimal 9 dari 10 mahasiswa berhasil menemukan informasi buku dalam waktu maksimal 2 menit tanpa bantuan. Waktu respons permintaan pencarian ditargetkan kurang dari 2 detik pada kondisi pengujian yang ditetapkan. |

Pengujian dilakukan menggunakan pendekatan task-based usability testing dengan sepuluh mahasiswa. Setiap peserta diberikan skenario pencarian yang setara untuk menemukan judul, status ketersediaan, dan lokasi rak buku.

Peserta dinyatakan berhasil apabila mampu menemukan seluruh informasi yang diminta dalam batas waktu maksimal dua menit tanpa bantuan petugas.

**Rumus:**

Usability (%) = (Jumlah peserta berhasil / Jumlah seluruh peserta) × 100%

Selain mengukur keberhasilan pengguna, pengujian juga mencatat waktu respons pencarian untuk mengetahui apakah rancangan mampu mendukung proses pencarian yang efisien.

Skenario SK-03 didukung oleh T-03 Filter Status dan Lokasi Rak, sedangkan pemisahan Katalog Web dari logika pengelolaan transaksi didukung oleh pola P-01 Layered.

## 2.5 Keterkaitan Skenario, Taktik, dan Pola Arsitektur

Ketiga skenario atribut kualitas menjadi dasar dalam pemilihan taktik dan pola arsitektur yang dijelaskan pada BAB III.

**Tabel 2.4 Keterkaitan Skenario dengan Rancangan Arsitektur**

| Skenario | Target Terukur | Taktik Pendukung | Pola Pendukung |
| --- | --- | --- | --- |
| SK-01 – Availability | Ketersediaan minimal 99%; health check setiap 5 detik | T-01 Health Check dan Pemulihan | P-01 Layered sebagai struktur penempatan komponen |
| SK-02 – Functional Correctness | Ketepatan minimal 95% dan pembaruan status maksimal 5 detik | T-02 Sinkronisasi Status melalui RabbitMQ | P-02 Publish–Subscribe |
| SK-03 – Usability | Minimal 90% peserta berhasil dalam 2 menit; respons pencarian kurang dari 2 detik | T-03 Filter Status dan Lokasi Rak | P-01 Layered |

SK-02 menjadi skenario yang paling langsung menjawab masalah utama layanan perpustakaan, yaitu ketidaksesuaian informasi ketersediaan buku. SK-01 memastikan rancangan mempertimbangkan keberlangsungan layanan, sedangkan SK-03 memastikan kemudahan pengguna dalam memperoleh informasi.

Ketiga skenario saling berkaitan dan menjadi dasar penerapan taktik pada BAB III, penggambaran komponen pada BAB IV, serta evaluasi keputusan arsitektur pada BAB V.

Seluruh target angka yang tercantum merupakan target rancangan yang diusulkan untuk pengujian, bukan hasil implementasi maupun pengukuran terhadap sistem perpustakaan UMI yang saat ini beroperasi.
