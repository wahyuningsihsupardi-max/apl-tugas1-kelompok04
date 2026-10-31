# B5. Analisis Trade-off

**Penanggung Jawab:** Andi Tsabitha Hulwah

Pada pengembangan Student Learning Platform — Layanan Perpustakaan UMI, analisis trade-off dilakukan berdasarkan tiga karakteristik pendorong yang telah ditetapkan pada bagian B1, yaitu **Availability** (Ketersediaan Layanan), **Functional Correctness** (Ketepatan Fungsi dan Informasi), dan **Usability** (Kemudahan Penggunaan).

Ketiga karakteristik tersebut saling berkaitan dalam mendukung pelayanan perpustakaan. *Availability* berkaitan dengan kemampuan sistem untuk tetap menyediakan layanan ketika dibutuhkan, *Functional Correctness* berkaitan dengan ketepatan informasi koleksi yang diberikan, sedangkan *Usability* berkaitan dengan kemudahan mahasiswa dalam memahami dan menggunakan sistem.

---

## Analisis Trade-off Antar Karakteristik

| No. | Karakteristik | Keuntungan | Konsekuensi (Trade-off) |
| :---: | :--- | :--- | :--- |
| **1.** | **Availability** | Menjaga layanan pencarian katalog dan informasi peminjaman agar tetap dapat diakses oleh mahasiswa maupun petugas. | Upaya meningkatkan ketersediaan layanan membutuhkan pengelolaan server, jaringan, dan pemantauan secara berkala. Hal ini dapat meningkatkan kebutuhan sumber daya operasional dan pemeliharaan sistem. |
| **2.** | **Functional Correctness** | Memastikan informasi judul buku, status ketersediaan, dan lokasi rak sesuai dengan kondisi koleksi sebenarnya. | Validasi dan pembaruan data yang lebih ketat dapat meningkatkan ketepatan informasi, tetapi membutuhkan waktu dan proses pengelolaan tambahan bagi petugas perpustakaan. |
| **3.** | **Usability** | Memudahkan mahasiswa memahami navigasi, melakukan pencarian katalog, dan mengakses layanan perpustakaan secara mandiri. | Penyederhanaan tampilan dapat meningkatkan kemudahan penggunaan, tetapi jika dilakukan secara berlebihan dapat mengurangi akses terhadap fungsi dan informasi penting. |

---

## Pembahasan Trade-off

### 1. Availability dan Functional Correctness
*Availability* dan *Functional Correctness* perlu diperhatikan secara bersamaan karena sistem yang dapat diakses belum tentu memberikan informasi yang benar. Pada layanan perpustakaan UMI, mahasiswa membutuhkan informasi mengenai judul, status ketersediaan, dan lokasi rak buku yang sesuai dengan kondisi koleksi sebenarnya.

Untuk menjaga ketepatan informasi, sistem memerlukan proses validasi serta pembaruan data koleksi dan transaksi peminjaman. Proses tersebut dapat membutuhkan waktu pengelolaan tambahan dan berpotensi memengaruhi ketersediaan layanan apabila tidak dikelola dengan baik. Oleh karena itu, proses pembaruan data perlu dirancang agar dapat dilakukan secara teratur tanpa menghambat akses mahasiswa dan petugas terhadap sistem.

### 2. Functional Correctness dan Usability
*Functional Correctness* dan *Usability* memiliki hubungan dalam penyajian informasi kepada pengguna. Sistem perlu memberikan informasi yang lengkap dan akurat, tetapi informasi tersebut juga harus disusun dengan tampilan yang mudah dipahami.

Jika seluruh informasi ditampilkan sekaligus dalam satu halaman, mahasiswa dapat mengalami kesulitan ketika mencari informasi yang dibutuhkan. Sebaliknya, jika tampilan terlalu disederhanakan, informasi penting seperti status ketersediaan dan lokasi rak buku dapat menjadi kurang terlihat atau sulit ditemukan.

Oleh karena itu, informasi katalog perlu disusun secara terstruktur dengan menampilkan informasi utama secara jelas. Informasi tambahan dapat ditempatkan pada bagian yang sesuai agar ketepatan informasi tetap terjaga tanpa mengurangi kemudahan penggunaan sistem.

### 3. Availability dan Usability
*Availability* dan *Usability* juga perlu dipertimbangkan karena sistem tidak hanya harus tersedia, tetapi juga harus mudah digunakan ketika diakses. Ketersediaan layanan membutuhkan pengelolaan server dan jaringan yang baik, sedangkan kemudahan penggunaan membutuhkan rancangan antarmuka dan navigasi yang sederhana.

Gangguan server atau jaringan dapat menyebabkan mahasiswa tidak dapat mengakses katalog meskipun tampilan sistem sudah dirancang dengan baik. Sebaliknya, sistem yang tersedia secara konsisten tetapi memiliki navigasi yang membingungkan tetap dapat menyulitkan pengguna.

Dengan demikian, pengembangan sistem perlu memperhatikan kestabilan akses sekaligus menyediakan tampilan yang sederhana, navigasi yang jelas, dan alur penggunaan yang mudah dipahami.

---

## Keputusan dan Strategi Pengelolaan Trade-off

Berdasarkan hasil analisis, terdapat beberapa strategi yang dapat diterapkan untuk mengurangi konsekuensi dari setiap keputusan arsitektur, yaitu:

1. **Menjaga ketersediaan layanan:** Melakukan pemantauan server dan jaringan secara berkala agar gangguan dapat diketahui dan ditangani sehingga akses katalog dan informasi peminjaman tetap tersedia.
2. **Menjaga ketepatan informasi:** Menerapkan validasi dan pembaruan data koleksi serta status peminjaman secara teratur agar informasi yang ditampilkan sesuai dengan kondisi sebenarnya.
3. **Mempertahankan kemudahan penggunaan:** Merancang antarmuka dengan navigasi sederhana dan penyajian informasi yang terstruktur. Fitur utama seperti pencarian katalog, status ketersediaan, dan lokasi rak harus mudah ditemukan oleh mahasiswa.
4. **Melakukan pengujian kualitas:** Pengujian dilakukan berdasarkan target yang telah ditetapkan pada B2, yaitu ketersediaan layanan minimal 99%, ketepatan informasi minimal 95% dari 100 sampel buku, serta keberhasilan minimal 90% mahasiswa dalam menemukan informasi buku dalam waktu maksimal 2 menit tanpa bantuan petugas.
5. **Melakukan evaluasi secara berkala:** Menggunakan hasil pengujian dan masukan dari mahasiswa maupun petugas perpustakaan untuk mengevaluasi apakah keputusan arsitektur sudah memenuhi kebutuhan layanan.

---

Berdasarkan analisis trade-off tersebut, pengembangan **Student Learning Platform — Layanan Perpustakaan UMI** perlu mempertimbangkan *Availability*, *Functional Correctness*, dan *Usability* secara seimbang. Ketiganya saling mendukung karena sistem yang tersedia harus tetap memberikan informasi yang tepat dan mudah digunakan. Dengan mempertimbangkan keuntungan serta konsekuensi dari setiap karakteristik, rancangan arsitektur diharapkan dapat mendukung pelayanan perpustakaan yang lebih stabil, akurat, dan mudah diakses oleh mahasiswa maupun petugas.

