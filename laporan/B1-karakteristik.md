# B1. Identifikasi dan Klasifikasi Karakteristik

**Penanggung jawab:** Wahyuningsih S

Studi Kasus: Student Learning Platform — Layanan Perpustakaan UMI

**B1.1 Identifikasi Kandidat Karakteristik Arsitektur**
Karakteristik arsitektur merupakan aspek kualitas atau kebutuhan teknis yang dapat memengaruhi keputusan dan rancangan arsitektur perangkat lunak. Identifikasi dilakukan berdasarkan kendala pelayanan, kebutuhan staf, dan pengalaman mahasiswa dalam menggunakan layanan perpustakaan.

Dalam pengelompokan ini digunakan tiga kategori:
* Operasional: karakteristik yang berkaitan dengan perilaku sistem ketika dijalankan.
* Struktural: karakteristik yang berkaitan dengan susunan komponen, modul, dan kemudahan pengembangan atau pemeliharaan sistem.
* Lintas sektor: karakteristik yang memengaruhi beberapa bagian sistem sekaligus dan tidak terbatas pada satu fungsi.
Sifat karakteristik dibedakan menjadi:
* Eksplisit: disebutkan secara langsung oleh responden atau terlihat jelas sebagai kebutuhan dalam hasil wawancara.
* Implisit: tidak disebutkan secara langsung sebagai karakteristik kualitas, tetapi dapat disimpulkan dari kendala dan kebutuhan yang disampaikan responden.

## B1. Identifikasi dan Klasifikasi Karakteristik Arsitektur

| No. | Karakteristik | Kategori | Sifat | Alasan |
|---|---|---|---|---|
| 1 | Availability (Ketersediaan) | Operasional | Eksplisit | Ketersediaan layanan menjadi kebutuhan penting. Wawancara juga menunjukkan adanya gangguan server dan jaringan yang dapat menghambat akses perpustakaan. |
| 2 | Functional Correctness (Ketepatan Fungsi) | Lintas sektor | Implisit | Hasil pencarian katalog belum selalu sesuai dengan kondisi buku sebenarnya. Informasi ketersediaan dan lokasi rak juga perlu lebih akurat. |
| 3 | Usability (Kemudahan Penggunaan) | Lintas sektor | Eksplisit | Kemudahan penggunaan menjadi perhatian dalam layanan perpustakaan. Mahasiswa juga mengalami kebingungan saat menggunakan katalog digital. |
| 4 | Performance Efficiency (Efisiensi Kinerja) | Operasional | Implisit | Proses pencarian buku yang membutuhkan waktu dan gangguan jaringan menunjukkan perlunya kinerja sistem yang lebih efisien. |
| 5 | Maintainability (Kemudahan Pemeliharaan) | Struktural | Implisit | Gangguan server dan jaringan menunjukkan perlunya sistem yang mudah dipelihara serta didukung pengelolaan teknis yang baik. |
| 6 | Interoperability (Interoperabilitas) | Struktural | Implisit | Adanya layanan perpustakaan fakultas dan perpustakaan pusat menjadi pertimbangan untuk mendukung pertukaran informasi antarlayanan. |
| 7 | Security (Keamanan) | Lintas sektor | Implisit | Sistem yang menggunakan akun pengguna dan menyimpan informasi peminjaman perlu memperhatikan keamanan data serta hak akses. |
| 8 | Data Integrity (Integritas Data) | Lintas sektor | Implisit | Perbedaan informasi antara katalog digital dan ketersediaan buku sebenarnya menunjukkan perlunya konsistensi dan keakuratan data. |
Catatan klasifikasi: Kategori di atas merupakan hasil analisis arsitektural terhadap kebutuhan perpustakaan, bukan istilah yang seluruhnya disebutkan oleh responden. Karakteristik nomor 7 dan 8 merupakan pertimbangan desain tambahan, bukan masalah yang secara langsung dilaporkan oleh narasumber.

Sumber identifikasi:
* Wawancara R1, staf PUBA UMI. HASIL WAWANCARA Apl.pdf
* Wawancara R2, staf FIKOM UMI. Wawancara R2 FIKOM.pdf
* Wawancara R3, R4, dan R5, mahasiswa PUBA UMI. Hasil Wawancara Mahasiswa R3,R4,R5.pdf
B1.2 Pemilihan Tiga Karakteristik Pendorong
Berdasarkan delapan kandidat yang telah diidentifikasi, tiga karakteristik dipilih sebagai karakteristik pendorong (architecturally significant characteristics), yaitu Availability, Functional Correctness, dan Usability.

Ketiganya dipilih karena mempunyai hubungan paling kuat dengan permasalahan nyata yang ditemukan dalam wawancara dan kebutuhan utama pengembangan layanan perpustakaan.

## Tiga Karakteristik Pendorong

| No. | Karakteristik Pendorong | Alasan Pemilihan | Dampak terhadap Arsitektur |
|---|---|---|---|
| 1 | Availability (Ketersediaan) | Gangguan server dan jaringan dapat menghambat mahasiswa maupun petugas dalam mengakses layanan perpustakaan. | Membutuhkan arsitektur yang mendukung kestabilan layanan, pemantauan sistem, dan penanganan gangguan. |
| 2 | Functional Correctness (Ketepatan Fungsi) | Informasi katalog, status ketersediaan buku, dan lokasi rak harus sesuai dengan kondisi sebenarnya. | Membutuhkan pengelolaan data yang konsisten, validasi informasi, dan sinkronisasi data. |
| 3 | Usability (Kemudahan Penggunaan) | Mahasiswa masih mengalami kesulitan ketika menggunakan katalog digital untuk mencari informasi buku. | Membutuhkan antarmuka yang sederhana, navigasi yang jelas, dan proses pencarian yang mudah dipahami. |

Alasan Pemilihan Ketiga Karakteristik
1. Availability (Ketersediaan Layanan)

Availability dipilih karena layanan perpustakaan digital membutuhkan sistem yang dapat diakses ketika mahasiswa maupun petugas memerlukan informasi koleksi. Berdasarkan wawancara R1, gangguan server dan jaringan menjadi salah satu kendala pelayanan. R2 juga menegaskan bahwa ketersediaan layanan merupakan prioritas karena proses pelayanan dapat terhambat apabila sistem tidak dapat digunakan.

Oleh karena itu, Availability menjadi karakteristik pendorong karena berpengaruh terhadap keberlangsungan layanan pencarian katalog, perpanjangan buku, dan akses koleksi digital.

2. Functional Correctness (Ketepatan Fungsi dan Informasi)

Functional Correctness dipilih karena informasi yang diberikan sistem harus sesuai dengan kondisi koleksi sebenarnya. Berdasarkan wawancara R3, terdapat pengalaman ketika buku tercantum dalam sistem pencarian, tetapi ternyata tidak tersedia saat hendak dipinjam. R3 dan R4 juga mengharapkan informasi ketersediaan buku yang lebih jelas, sedangkan R3 membutuhkan informasi lokasi rak yang lebih rinci.

Oleh karena itu, ketepatan informasi menjadi karakteristik penting agar mahasiswa tidak mengalami kesulitan dalam menemukan koleksi. Sistem harus mampu menampilkan informasi judul, status ketersediaan, dan lokasi koleksi secara tepat.

3. Usability (Kemudahan Penggunaan)

Usability dipilih karena sistem perpustakaan harus mudah dipahami oleh mahasiswa maupun petugas. Berdasarkan wawancara R1, kemudahan penggunaan menjadi salah satu aspek yang diprioritaskan dalam layanan digital. Selain itu, R5 menyampaikan bahwa dirinya masih merasa bingung ketika menggunakan katalog digital.

Dengan demikian, sistem perlu menyediakan tampilan yang sederhana, navigasi yang jelas, serta fitur pencarian yang mudah digunakan agar mahasiswa dapat menemukan buku dan mengakses layanan tanpa mengalami kesulitan.

Ketiga karakteristik tersebut saling mendukung. Availability memastikan layanan dapat diakses, Functional Correctness memastikan informasi yang diberikan benar, sedangkan Usability memastikan layanan mudah digunakan.
B1.3 Pengujian Tiga Karakteristik Pendorong dengan Tiga Ciri Karakteristik Arsitektur
Sesuai ketentuan B1, ketiga karakteristik pendorong perlu diuji menggunakan tiga ciri untuk memastikan bahwa karakteristik tersebut memang signifikan terhadap arsitektur perangkat lunak.

Tiga pertanyaan pengujian yang digunakan adalah:
1. Bukan sekadar kebutuhan domain: Apakah karakteristik tersebut merupakan pertimbangan kualitas atau desain sistem, bukan hanya fungsi bisnis yang harus disediakan?
2. Memengaruhi struktur arsitektur: Apakah karakteristik tersebut memengaruhi pemilihan, susunan, hubungan, atau tanggung jawab komponen sistem?
3. Penting bagi keberhasilan sistem: Apakah kegagalan memenuhi karakteristik tersebut dapat menghambat tujuan utama sistem?

## Pengujian Karakteristik Pendorong

| No. | Karakteristik | Bukan Sekadar Kebutuhan Fungsional | Memengaruhi Struktur Arsitektur | Penting bagi Keberhasilan Sistem | Kesimpulan |
|---|---|---|---|---|---|
| 1 | Availability | Ya, karena berkaitan dengan kualitas ketersediaan layanan. | Ya, memengaruhi rancangan server, jaringan, dan mekanisme pemulihan layanan. | Ya, karena sistem harus dapat diakses ketika dibutuhkan. | Memenuhi |
| 2 | Functional Correctness | Ya, karena menekankan ketepatan hasil dan informasi sistem. | Ya, memengaruhi pengelolaan, validasi, dan konsistensi data. | Ya, karena informasi buku harus sesuai dengan kondisi sebenarnya. | Memenuhi |
| 3 | Usability | Ya, karena berkaitan dengan kemudahan interaksi pengguna. | Ya, memengaruhi rancangan antarmuka dan alur interaksi sistem. | Ya, karena sistem harus mudah digunakan mahasiswa dan petugas. | Memenuhi |

Hasil Pengujian
Berdasarkan ketiga ciri tersebut, Availability, Functional Correctness, dan Usability memenuhi kriteria sebagai karakteristik pendorong arsitektur.

Ketiga karakteristik tidak hanya berkaitan dengan kebutuhan pengguna, tetapi juga memengaruhi keputusan perancangan sistem. Availability memengaruhi rancangan infrastruktur dan penanganan gangguan, Functional Correctness memengaruhi pengelolaan serta validasi data, sedangkan Usability memengaruhi rancangan antarmuka dan interaksi pengguna.

Dengan demikian, ketiganya layak dijadikan dasar penyusunan B2 (Atribut Kualitas yang Terukur) dan B4 (Pemetaan ke ISO/IEC 25010).
