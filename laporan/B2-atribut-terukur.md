
# B2. Atribut Kualitas yang Terukur

**Penanggung jawab: Besse Armelya Cindy Damayanti M**

## Dasar Penentuan Atribut Kualitas

Berdasarkan tiga karakteristik pendorong yang dipilih pada B1, yaitu Availability, Functional Correctness, dan Usability, bagian ini menetapkan target kualitas yang terukur untuk rancangan Student Learning Platform — Layanan Perpustakaan UMI. Penentuan atribut kualitas mengacu pada prinsip evaluasi kualitas arsitektur perangkat lunak [1], [2].

Hasil wawancara R1–R5 menunjukkan adanya beberapa kebutuhan, antara lain ketersediaan akses layanan, ketepatan informasi koleksi, serta kemudahan penggunaan katalog digital. Oleh karena itu, ketiga karakteristik tersebut digunakan sebagai dasar penentuan target pengukuran.

## Target, Kondisi Pengukuran, dan Cara Pengujian

| No. | Karakteristik Pendorong | Target Terukur | Kondisi Pengukuran | Cara Pengujian |
|---|---|---|---|---|
| 1 | Availability (Ketersediaan Layanan) | Tingkat ketersediaan layanan minimal 99%. | Layanan pencarian katalog dan informasi peminjaman dipantau pada lingkungan pengujian selama 7 hari berturut-turut dengan pemeriksaan setiap 1 menit. | Melakukan pemantauan otomatis, mencatat pemeriksaan berhasil dan gagal, kemudian menghitung persentase ketersediaan layanan. |
| 2 | Functional Correctness (Ketepatan Informasi Koleksi) | Minimal 95 dari 100 data buku memiliki informasi yang sesuai dengan kondisi sebenarnya. | Pengujian terhadap 100 sampel buku yang telah tercatat, meliputi judul, status ketersediaan, dan lokasi rak. Data dibandingkan dengan catatan koleksi dan peminjaman yang dikonfirmasi petugas pada waktu yang sama. | Melakukan pengujian kesesuaian informasi katalog terhadap data koleksi dan status peminjaman, kemudian menghitung persentase data yang seluruh atributnya benar. |
| 3 | Usability (Kemudahan Penggunaan) | Minimal 90% mahasiswa berhasil menemukan informasi buku dalam waktu maksimal 2 menit tanpa bantuan petugas. | Pengujian menggunakan prototipe sistem terhadap 10 mahasiswa dengan skenario pencarian yang setara. | Melakukan task-based usability testing dengan mencatat keberhasilan peserta dan waktu penyelesaian tugas. |

## Penjelasan Atribut Kualitas

### 1. Availability (Ketersediaan Layanan)

Availability diprioritaskan karena gangguan server dan jaringan yang disampaikan staf perpustakaan R1 dan R2 dapat menghambat akses terhadap layanan digital.

Target ketersediaan layanan ditetapkan minimal 99% selama periode pengujian. Pengukurannya menggunakan rumus:

**Ketersediaan (%) = (Jumlah pemeriksaan berhasil / Jumlah seluruh pemeriksaan) × 100%**

Pengujian dilakukan melalui pemantauan otomatis untuk mengetahui kemampuan layanan dalam menyediakan akses selama periode pengukuran.

### 2. Functional Correctness (Ketepatan Informasi Koleksi)

Functional Correctness diperlukan untuk memastikan informasi yang diberikan sistem sesuai dengan kondisi koleksi sebenarnya.

Berdasarkan wawancara R3 dan R4, mahasiswa membutuhkan informasi status ketersediaan dan lokasi buku yang lebih jelas. R3 juga menemukan ketidaksesuaian informasi katalog dengan ketersediaan buku.

Oleh karena itu, ditetapkan target minimal 95% kesesuaian informasi dari 100 sampel buku yang diuji.

Pengukurannya menggunakan rumus:

**Ketepatan Informasi (%) = (Jumlah data sesuai / Jumlah data diperiksa) × 100%**

Pengujian dilakukan dengan membandingkan informasi katalog terhadap data koleksi dan peminjaman yang dikonfirmasi petugas. Sampel dibatasi pada koleksi yang telah terdata karena katalog Perpustakaan FIKOM UMI masih dalam proses pendataan.

### 3. Usability (Kemudahan Penggunaan)

Usability diperlukan agar mahasiswa dapat menggunakan layanan katalog secara mandiri.

Berdasarkan wawancara, R1 memprioritaskan kemudahan penggunaan, sedangkan R5 menyampaikan kendala dalam memahami penggunaan katalog digital.

Oleh karena itu, target pengujian ditetapkan minimal 90% mahasiswa berhasil menemukan judul, status ketersediaan, dan lokasi rak buku dalam waktu maksimal 2 menit tanpa bantuan petugas.

Pengukurannya menggunakan rumus:

**Keberhasilan Pengguna (%) = (Jumlah peserta yang berhasil sesuai batas waktu / Jumlah seluruh peserta) × 100%**

Pengujian dilakukan terhadap 10 mahasiswa menggunakan skenario pencarian yang setara. Peserta dinyatakan berhasil apabila memperoleh informasi yang diminta dalam batas waktu yang telah ditentukan.

## Kesimpulan

Ketiga atribut kualitas tersebut digunakan sebagai dasar evaluasi rancangan layanan perpustakaan UMI. Availability memastikan layanan dapat diakses, Functional Correctness memastikan informasi koleksi sesuai kondisi sebenarnya, sedangkan Usability memastikan layanan mudah digunakan.

Seluruh angka merupakan target kualitas rancangan yang diusulkan, bukan hasil pengujian terhadap sistem perpustakaan UMI yang saat ini digunakan.

