# U3. Taktik Arsitektur

**BAB III: Taktik dan Pola (bagian taktik)**  
**Penanggung jawab:** Wahyuningsih S

**Ketentuan dosen:** Minimal **satu taktik untuk setiap skenario** U2, dengan cara kerja dan **harga yang dibayar** (konsekuensi, biaya, atau trade-off).


# 2.6 Taktik Arsitektur (Architectural Tactics)

Taktik arsitektur merupakan strategi yang digunakan untuk memenuhi kebutuhan atribut kualitas sistem. Setiap skenario memiliki minimal satu taktik yang menjelaskan cara kerja serta harga yang harus dibayar (*trade-off*) dalam penerapannya.

## 2.6.1 Tabel Taktik Arsitektur

| Kode | Skenario | Nama Taktik | Cara Kerja | Harga yang Dibayar (Trade-off) |
|---|---|---|---|---|
| T-01 | SK-01 - Availability | Health Check dan Pemulihan | Sistem melakukan pemeriksaan kesehatan layanan setiap 5 detik. Jika salah satu instans mengalami gangguan, API Gateway mengalihkan permintaan ke instans lain yang masih berjalan. | Membutuhkan server atau instans tambahan, biaya infrastruktur lebih besar, serta pemeliharaan dan pemantauan sistem yang lebih kompleks. |
| T-02 | SK-02 - Functional Correctness | Sinkronisasi Status melalui RabbitMQ | Setiap transaksi peminjaman atau pengembalian menghasilkan event perubahan status. RabbitMQ meneruskan event ke Index Updater untuk memperbarui data pada Elasticsearch. | Membutuhkan tambahan infrastruktur RabbitMQ, meningkatkan kompleksitas sistem, dan terdapat kemungkinan keterlambatan sinkronisasi data. |
| T-03 | SK-03 - Usability | Filter Status dan Lokasi Rak | Sistem menyediakan fitur pencarian dan filter berdasarkan status ketersediaan buku serta lokasi rak sehingga mahasiswa dapat menemukan buku dengan mudah. | Membutuhkan waktu pengembangan fitur, pengelolaan data lokasi rak yang konsisten, serta pemeliharaan indeks pencarian Elasticsearch. |

## 2.6.2 Penjelasan Taktik

### 1. T-01 - Health Check dan Pemulihan

Taktik ini diterapkan untuk menjaga ketersediaan layanan perpustakaan. Sistem melakukan pemeriksaan kondisi layanan setiap 5 detik. Apabila salah satu instans mengalami gangguan, API Gateway akan mengalihkan permintaan pengguna ke instans lain yang masih aktif.

**Cara Kerja:**
1. Sistem melakukan pemeriksaan kesehatan layanan (*health check*).
2. Sistem mendeteksi instans yang mengalami gangguan.
3. API Gateway mengalihkan permintaan ke instans yang sehat.
4. Pengguna tetap dapat mengakses layanan perpustakaan.

**Harga yang Dibayar (Trade-off):**
- Membutuhkan server atau instans tambahan.
- Meningkatkan biaya infrastruktur.
- Membutuhkan pemantauan dan pemeliharaan sistem.

### 2. T-02 - Sinkronisasi Status melalui RabbitMQ

Taktik ini diterapkan untuk menjaga ketepatan informasi ketersediaan buku. Ketika staf melakukan transaksi peminjaman atau pengembalian, sistem mengirimkan event perubahan status melalui RabbitMQ agar informasi katalog dapat diperbarui.

**Cara Kerja:**
1. Staf mencatat transaksi peminjaman atau pengembalian buku.
2. Circulation Service menyimpan perubahan transaksi.
3. Event perubahan status dikirim melalui RabbitMQ.
4. Index Updater menerima event tersebut.
5. Elasticsearch memperbarui informasi status buku.
6. Katalog menampilkan informasi terbaru kepada pengguna.

**Harga yang Dibayar (Trade-off):**
- Membutuhkan infrastruktur RabbitMQ.
- Menambah kompleksitas pengembangan sistem.
- Membutuhkan pengelolaan pesan dan sinkronisasi data.

### 3. T-03 - Filter Status dan Lokasi Rak

Taktik ini diterapkan untuk meningkatkan kemudahan penggunaan sistem. Mahasiswa dapat mencari buku berdasarkan judul, status ketersediaan, dan lokasi rak melalui Katalog Web.

**Cara Kerja:**
1. Mahasiswa membuka halaman Katalog Web.
2. Mahasiswa memasukkan judul buku yang ingin dicari.
3. Mahasiswa memilih filter status ketersediaan.
4. Sistem menampilkan hasil pencarian beserta lokasi rak buku.
5. Mahasiswa dapat mengetahui informasi buku tanpa harus bertanya kepada petugas.

**Harga yang Dibayar (Trade-off):**
- Membutuhkan waktu pengembangan fitur.
- Membutuhkan pengelolaan data lokasi rak.
- Membutuhkan pemeliharaan indeks pencarian Elasticsearch.

## 2.6.3 Kesimpulan

Berdasarkan ketiga taktik yang telah dirancang, setiap taktik memiliki tujuan untuk memenuhi kebutuhan kualitas sistem Student Learning Platform - Layanan Perpustakaan UMI.

1. **T-01:** Menjaga ketersediaan layanan (*Availability*).
2. **T-02:** Menjaga ketepatan informasi koleksi (*Functional Correctness*).
3. **T-03:** Meningkatkan kemudahan penggunaan sistem (*Usability*).

Penerapan ketiga taktik tersebut diharapkan dapat meningkatkan kualitas layanan perpustakaan, meskipun membutuhkan biaya infrastruktur, waktu pengembangan, dan pemeliharaan tambahan.
