# U1. Konteks dan ASR

**BAB I: PENDAHULUAN**  

**Penanggung Jawab:** Besse Armelya Cindy Damayanti M 

## 1.1 Konteks dan Ringkasan Layanan

Student Learning Platform merupakan salah satu subsistem Smart Campus Universitas Muslim Indonesia (UMI). Pada kelompok II-4, layanan yang dikaji adalah Layanan Perpustakaan, dengan ruang lingkup pencarian katalog buku, peminjaman, pengembalian, perpanjangan masa peminjaman, serta akses koleksi digital.

Berdasarkan hasil analisis pada Tugas Kelompok I, Perpustakaan UMI telah memanfaatkan layanan katalog digital dan menyediakan akses koleksi melalui Moco PUBA UMI. Namun, hasil wawancara dengan staf dan mahasiswa menunjukkan masih adanya kendala dalam akses layanan, ketepatan informasi ketersediaan buku, serta kemudahan penggunaan katalog digital.

Perancangan arsitektur pada tugas MID ini difokuskan untuk mendukung pengelolaan informasi koleksi yang lebih akurat, layanan yang tersedia saat dibutuhkan, dan proses pencarian yang mudah digunakan.

Rancangan menggunakan pendekatan Service-Based Architecture, dengan pemisahan tanggung jawab melalui pola Layered serta penyebaran perubahan status buku melalui pola Publish–Subscribe. Kedua pola tersebut dijelaskan lebih lanjut pada BAB III.

## 1.2 Masalah Utama dan Bukti Pendukung

Masalah utama yang dipilih dalam perancangan ini adalah ketidaksesuaian informasi status ketersediaan buku pada katalog dengan kondisi koleksi dan transaksi peminjaman yang sebenarnya.

Masalah tersebut dapat menyebabkan mahasiswa memperoleh informasi bahwa suatu buku tersedia, tetapi ketika hendak dipinjam ternyata buku tersebut tidak tersedia. Akibatnya, mahasiswa perlu melakukan pengecekan ulang kepada petugas atau mencari koleksi lain.

Pemilihan masalah utama didasarkan pada hasil wawancara Tugas Kelompok I, khususnya temuan berikut.

**Tabel 1.1 Bukti Pendukung Masalah Layanan**

| Responden | Temuan Wawancara | Hubungan dengan Rancangan |
| --- | --- | --- |
| R1 – Staf PUBA UMI | Terdapat kendala server dan jaringan. Kemudahan serta ketersediaan layanan menjadi perhatian. | Sistem memerlukan pemantauan dan mekanisme penanganan gangguan. |
| R2 – Staf Perpustakaan FIKOM UMI | Katalog perpustakaan fakultas belum lengkap dan layanan masih bergantung pada perpustakaan pusat. | Rancangan perlu mempertimbangkan integrasi dan ketersediaan informasi koleksi. |
| R3 – Mahasiswa | Pernah menemukan informasi katalog yang tidak sesuai dengan ketersediaan buku sebenarnya dan membutuhkan informasi lokasi rak yang lebih jelas. | Menjadi bukti utama perlunya peningkatan ketepatan informasi katalog. |
| R4 – Mahasiswa | Membutuhkan informasi ketersediaan koleksi yang jelas dan pengingat peminjaman. | Mendukung kebutuhan pembaruan status serta notifikasi transaksi. |
| R5 – Mahasiswa | Mengalami kebingungan dalam menggunakan katalog digital. | Mendukung kebutuhan antarmuka pencarian yang lebih sederhana. |

Sumber: Dokumentasi wawancara R1–R5 pada Tugas Kelompok I.

Berdasarkan temuan tersebut, kelompok mengusulkan mekanisme pembaruan status buku yang terhubung dengan pencatatan transaksi peminjaman dan pengembalian.

Dalam rancangan ini, Circulation Service mencatat perubahan transaksi ke PostgreSQL sebagai sumber data utama. Perubahan status kemudian disampaikan melalui event StatusBukuBerubah menggunakan RabbitMQ. Index Updater menerima event tersebut untuk memperbarui indeks katalog pada Elasticsearch sehingga Katalog Web dapat menampilkan informasi terbaru.

Mekanisme tersebut merupakan solusi arsitektur yang diusulkan untuk mengurangi ketidaksesuaian informasi. Penyebab teknis ketidaksesuaian pada sistem perpustakaan yang sedang digunakan belum dapat dipastikan hanya berdasarkan wawancara.

## 1.3 Identifikasi Stakeholder

Stakeholder adalah pihak yang memiliki kepentingan atau terlibat dalam penggunaan, pengelolaan, dan pengembangan layanan perpustakaan. Identifikasi stakeholder mengacu pada hasil analisis Power/Interest dalam Tugas Kelompok I.

**Tabel 1.2 Identifikasi Stakeholder**

| No. | Stakeholder | Peran dan Kebutuhan |
| --- | --- | --- |
| 1. | Pengelola Perpustakaan UMI | Menetapkan kebijakan layanan dan membutuhkan informasi koleksi serta transaksi yang dapat diandalkan. |
| 2. | Tim Pengembang Sistem dan Tim IT UMI | Mengembangkan, memantau, serta memelihara layanan agar tetap berfungsi dan mudah diperbaiki ketika terjadi gangguan. |
| 3. | Staf Perpustakaan PUBA UMI | Mencatat peminjaman, pengembalian, dan perpanjangan buku serta membutuhkan status koleksi yang sesuai dengan transaksi. |
| 4. | Staf Perpustakaan FIKOM UMI | Membutuhkan akses informasi koleksi yang jelas dan mendukung koordinasi layanan dengan perpustakaan pusat. |
| 5. | Mahasiswa dan Dosen | Mencari buku, memeriksa status ketersediaan, mengetahui lokasi rak, dan mengakses koleksi digital dengan mudah. |
| 6. | Penyedia Koleksi Digital | Menyediakan akses dan layanan koleksi digital yang memerlukan koordinasi integrasi dengan sistem perpustakaan. |

Pengelola perpustakaan dan tim pengembang memiliki peran penting dalam pengambilan keputusan dan pengembangan sistem. Sementara itu, staf perpustakaan serta mahasiswa merupakan pengguna yang berinteraksi langsung dengan layanan.

Kebutuhan stakeholder menjadi dasar dalam menentukan prioritas kebutuhan signifikan arsitektur dan skenario atribut kualitas.

## 1.4 Architecture Significant Requirements (ASR)

Architecture Significant Requirements (ASR) merupakan kebutuhan yang berpengaruh besar terhadap struktur, komponen, dan keputusan arsitektur perangkat lunak [1].

Berdasarkan hasil Tugas Kelompok I, untuk kebutuhan MID ditetapkan tiga ASR utama, yaitu ketersediaan layanan, ketepatan dan konsistensi informasi status buku, serta kemudahan dan kinerja pencarian katalog. Ketiganya dirumuskan dengan mempertimbangkan karakteristik pendorong yang telah dipilih pada B1, yaitu Availability, Functional Correctness, dan Usability.

**Tabel 1.3 Kebutuhan Signifikan Arsitektur**

| Kode | Kebutuhan Signifikan | Target dan Pengaruh terhadap Arsitektur |
| --- | --- | --- |
| ASR-01 | Ketersediaan layanan katalog dan peminjaman | Layanan ditargetkan memiliki Availability minimal 99% selama periode pengukuran tujuh hari. Arsitektur memerlukan health check, instans layanan cadangan, dan mekanisme pemulihan. |
| ASR-02 | Ketepatan dan konsistensi informasi status buku | Minimal 95 dari 100 sampel informasi koleksi harus sesuai dengan data acuan. Perubahan status ditargetkan tampil pada katalog maksimal 5 detik setelah transaksi berhasil dicatat. Kebutuhan ini memengaruhi pencatatan transaksi dan mekanisme sinkronisasi berbasis event. |
| ASR-03 | Kemudahan dan kinerja pencarian katalog | Minimal 90% pengguna uji harus berhasil menemukan status serta lokasi buku dalam waktu maksimal 2 menit tanpa bantuan. Waktu respons pencarian ditargetkan kurang dari 2 detik. Kebutuhan ini memengaruhi desain antarmuka, filter pencarian, dan pengelolaan indeks katalog. |

ASR-02 menjadi prioritas utama dalam menjawab masalah ketidaksesuaian informasi ketersediaan buku. ASR-01 mendukung keberlangsungan operasional layanan, sedangkan ASR-03 mendukung kemudahan mahasiswa memperoleh informasi koleksi.

Ketiga ASR ini menjadi dasar penyusunan skenario atribut kualitas pada BAB II dan pemilihan taktik arsitektur pada BAB III.

## 1.5 Karakteristik Pendorong Arsitektur

Berdasarkan hasil identifikasi dan pengujian karakteristik pada bagian B1 Tugas Kelompok I, dipilih tiga karakteristik pendorong, yaitu Availability, Functional Correctness, dan Usability.

**Tabel 1.4 Karakteristik Pendorong Arsitektur**

| Karakteristik | Dasar Pemilihan | Pengaruh terhadap Rancangan |
| --- | --- | --- |
| Availability | R1 menyampaikan kendala server dan jaringan, sedangkan R2 menekankan ketersediaan layanan. | Memerlukan pemantauan kondisi layanan, instans cadangan, dan mekanisme pemulihan gangguan. |
| Functional Correctness | R3 menemukan ketidaksesuaian informasi katalog dengan kondisi koleksi, sedangkan R3 dan R4 membutuhkan status ketersediaan yang lebih jelas. | Memerlukan pencatatan transaksi yang konsisten dan pembaruan informasi katalog melalui mekanisme sinkronisasi status. |
| Usability | R1 memprioritaskan kemudahan penggunaan, sementara R5 mengalami kebingungan dalam menggunakan katalog digital. | Memerlukan antarmuka yang sederhana, navigasi jelas, serta fitur pencarian dan filter informasi buku. |

Ketiga karakteristik tersebut saling melengkapi. Availability memastikan layanan dapat diakses ketika dibutuhkan, Functional Correctness memastikan informasi yang diberikan sesuai dengan kondisi sebenarnya, sedangkan Usability memastikan layanan dapat digunakan dengan mudah.

Karakteristik tersebut kemudian diterjemahkan menjadi tiga skenario atribut kualitas yang memiliki ukuran keberhasilan berupa angka.
