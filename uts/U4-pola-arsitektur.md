# U4. Pola Arsitektur

**BAB III: Taktik dan Pola (bagian pola)**  
**Penanggung jawab:** Andi Tsabitha Hulwah

**Kategori:** Struktur atau pembagian lapisan (modul).

**Definisi Singkat**

Layered Architecture merupakan pola arsitektur yang membagi sistem menjadi beberapa lapisan berdasarkan tanggung jawab tertentu. Setiap lapisan memiliki fungsi masing-masing, dan lapisan atas hanya boleh menggunakan layanan dari lapisan tepat di bawahnya [1], [2].

**Alasan Sesuai Kasus Perpustakaan UMI**

Pola Layered dipilih karena sistem Student Learning Platform – Layanan Perpustakaan UMI memiliki beberapa fungsi yang perlu dipisahkan, seperti pencarian katalog, pengelolaan transaksi peminjaman, pengelolaan data, dan integrasi dengan sistem eksternal. Pembagian lapisan membantu menjaga agar perubahan pada satu bagian sistem tidak langsung memengaruhi bagian lainnya.

Pola ini juga mendukung kebutuhan SK-03 dan T-03, khususnya dalam meningkatkan kemudahan penggunaan Katalog Web melalui penambahan filter status dan lokasi rak tanpa harus mengubah logika peminjaman.

**Cara Diterapkan**

Pola Layered diterapkan dengan membagi sistem menjadi beberapa lapisan sebagai berikut:

1. **Presentation Layer:** terdiri atas Katalog Web dan Panel Sirkulasi sebagai antarmuka yang digunakan oleh mahasiswa dan petugas perpustakaan.
2. **API Gateway:** menjadi pintu masuk komunikasi antara antarmuka pengguna dan layanan backend.
3. **Business Logic Layer:** terdiri atas Catalog Service dan Circulation Service yang menangani fungsi utama pengelolaan katalog dan transaksi peminjaman.
4. **Data Access Layer:** menggunakan Repository untuk menghubungkan layanan dengan PostgreSQL dan Elasticsearch.
5. **Integration Layer:** menangani hubungan sistem perpustakaan dengan sistem eksternal melalui komponen integrasi.

Alur komunikasi utama adalah Katalog Web / Panel Sirkulasi → API Gateway → Catalog Service / Circulation Service → Repository → PostgreSQL. Elasticsearch digunakan sebagai indeks katalog untuk mendukung proses pencarian buku.

Aturan peminjaman, yaitu maksimal 5 buku, masa pinjam 7 hari, perpanjangan maksimal 1 kali, dan denda Rp1.000 per hari, dikelola melalui Circulation Service.

**Kelebihan pada Layanan Perpustakaan UMI**

1. Tampilan Katalog Web dapat diperbaiki melalui penambahan filter status dan lokasi rak (T-03) untuk mengatasi keluhan R5 tanpa mengubah logika peminjaman.
2. Jika kebijakan perpustakaan berubah, cukup Circulation Service yang diperbarui sehingga perubahan tidak menyebar ke lapisan lainnya.
3. API Gateway menjadi satu titik untuk melakukan health check dan mengalihkan permintaan ke instans layanan yang masih sehat (T-01).

**Kekurangan pada Layanan Perpustakaan UMI**

1. Setiap permintaan melewati beberapa lapisan sehingga dapat menambah latensi. Oleh karena itu, waktu pencarian harus tetap kurang dari 2 detik.
2. Terdapat kemungkinan komponen melompati lapisan, misalnya laporan langsung membaca PostgreSQL tanpa melalui Repository, sehingga aturan akses data dapat terganggu apabila tidak diawasi.
3. Perubahan yang menyentuh seluruh lapisan, seperti penambahan kolom lokasi rak secara rinci, harus disesuaikan pada beberapa bagian sistem sekaligus.

---

## 4.2 Pola P-02 – Publish–Subscribe

**Kategori:** Komunikasi berbasis event (Component-and-Connector/C&C).

**Definisi Singkat**

Publish–Subscribe merupakan pola komunikasi yang memungkinkan komponen penerbit mengirimkan event melalui perantara tanpa mengetahui secara langsung siapa penerimanya. Sementara itu, komponen pelanggan atau subscriber menerima event sesuai dengan informasi yang diminatinya [1], [5].

**Alasan Sesuai Kasus Perpustakaan UMI**

Pola Publish–Subscribe dipilih karena terdapat permasalahan ketika status buku pada katalog tidak langsung berubah setelah transaksi peminjaman atau pengembalian dicatat oleh petugas perpustakaan.

Permasalahan tersebut membutuhkan mekanisme komunikasi yang dapat meneruskan perubahan status buku kepada komponen lain secara otomatis. Dengan pola ini, Circulation Service tidak perlu berkomunikasi secara langsung dengan setiap komponen yang membutuhkan informasi perubahan status buku.

Pola Publish–Subscribe mendukung SK-02 dan T-02, yaitu menjaga kesesuaian informasi status buku pada katalog serta mendukung pengiriman notifikasi pengingat jatuh tempo.

**Cara Diterapkan**

Pola Publish–Subscribe diterapkan melalui beberapa komponen berikut:

1. **Publisher – Circulation Service:** menerbitkan event StatusBukuBerubah ketika terjadi perubahan status eksemplar buku, seperti dipinjam, dikembalikan, diperpanjang, atau dipindahkan rak.
2. **Message Broker – RabbitMQ:** menerima dan meneruskan event kepada komponen yang berlangganan.
3. **Subscriber – Index Updater:** merupakan bagian dari Catalog Service yang menerima event untuk memperbarui indeks Elasticsearch.
4. **Subscriber – Notification Service:** menerima event yang relevan untuk mengirimkan pengingat jatuh tempo peminjaman.

Alur komunikasi yang diterapkan adalah Circulation Service → Message Queue RabbitMQ → Index Updater dan Notification Service.

Untuk mengurangi risiko event hilang, status transaksi dan event disimpan dalam satu transaksi PostgreSQL, kemudian diteruskan ke RabbitMQ melalui Outbox Relay. Mekanisme ini dikenal sebagai Transactional Outbox [6].

Pembaruan status pada Katalog Web ditargetkan berlangsung maksimal 5 detik setelah transaksi dicatat melalui Panel Sirkulasi.

**Kelebihan pada Layanan Perpustakaan UMI**

1. Status buku pada Katalog Web dapat berubah secara otomatis maksimal 5 detik setelah transaksi dicatat pada Panel Sirkulasi tanpa memerlukan input ulang oleh petugas.
2. Circulation Service tidak perlu mengetahui secara langsung seluruh komponen yang menerima event. Pelanggan baru, seperti katalog FIKOM, dapat ditambahkan tanpa mengubah kode sirkulasi.
3. Proses peminjaman tetap dapat berjalan ketika Catalog Service atau Elasticsearch mengalami gangguan karena event dapat menunggu di RabbitMQ untuk diproses kembali. Hal ini mendukung SK-01.

**Kekurangan pada Layanan Perpustakaan UMI**

1. Terdapat potensi keterlambatan sinkronisasi karena komunikasi berlangsung secara asynchronous. Akibatnya, terdapat jeda singkat ketika katalog belum menampilkan status buku terbaru, dengan target keterlambatan maksimal 5 detik.
2. Beban dan fungsi RabbitMQ bertambah karena tidak hanya menangani notifikasi, tetapi juga menjadi jalur penyampaian perubahan status buku. Oleh karena itu, RabbitMQ perlu dipantau oleh Tim IT dengan mempertimbangkan keterbatasan anggaran.
3. Alur komunikasi lebih sulit dilacak dan diuji karena event diproses secara asynchronous sehingga diperlukan log yang dapat menghubungkan transaksi dengan event yang diterbitkan.

---

## 4.3 Satu Perbedaan Gaya dan Pola Arsitektur

**Gaya (Architectural Style)**

Gaya arsitektur menggambarkan karakter umum cara keseluruhan sistem diorganisasikan, meliputi topologi, pembagian komponen, dan cara penerapannya [2]. Pada Student Learning Platform – Layanan Perpustakaan UMI, gaya arsitektur yang digunakan adalah Service-Based Architecture, yaitu pendekatan yang membagi sistem menjadi beberapa layanan domain berbutir kasar, seperti Catalog Service, Circulation Service, Digital Collection Service, Notification Service, dan Auth Service yang berbagi infrastruktur data.

**Pola (Architectural Pattern)**

Pola arsitektur merupakan solusi desain yang lebih spesifik dan dapat digunakan kembali untuk menyelesaikan permasalahan yang berulang pada bagian tertentu dari sistem [1], [5]. Pada proyek ini, Publish–Subscribe digunakan untuk mengatur penyebaran perubahan status buku melalui RabbitMQ, sedangkan Layered digunakan untuk mengatur pembagian tanggung jawab antarlapisan sistem.

**Perbedaan pada Contoh Kelompok**

Service-Based Architecture menggambarkan bagaimana keseluruhan layanan perpustakaan dibagi menjadi beberapa layanan berdasarkan fungsi masing-masing. Sementara itu, Publish–Subscribe mengatur cara komponen-komponen tersebut berkomunikasi dalam menangani perubahan status buku. Dengan demikian, gaya arsitektur mencakup organisasi sistem secara keseluruhan, sedangkan pola arsitektur menyelesaikan permasalahan desain yang lebih spesifik [2].

| Aspek | Gaya: Service-Based Architecture | Pola: P-02 Publish–Subscribe |
|---|---|---|
| Cakupan | Seluruh sistem perpustakaan. | Satu interaksi, yaitu penyebaran perubahan status buku. |
| Pertanyaan yang Dijawab | Bagaimana sistem dibagi menjadi beberapa bagian dan bagaimana bagian tersebut diterapkan? | Bagaimana katalog mengetahui perubahan status buku tanpa memanggil Circulation Service secara langsung? |
| Akibat Jika Diganti | Seluruh struktur, tim, dan penerapan ikut berubah sehingga membutuhkan biaya perubahan yang besar. | Hanya jalur event yang berubah, sedangkan layanan lainnya tetap dapat dipertahankan. |

---

## 4.4 Hubungan dengan Taktik, Diagram, dan ADR

Penerapan pola Layered dan Publish–Subscribe memiliki hubungan dengan taktik arsitektur pada U3, diagram desain pada U5, serta keputusan arsitektur yang didokumentasikan melalui Architecture Decision Record (ADR) pada U7.

Hubungan antara pola arsitektur dan taktik yang diterapkan adalah sebagai berikut:

| Pola Arsitektur | Taktik yang Didukung | Penerapan pada Sistem |
|---|---|---|
| P-01 Layered | SK-03 dan T-03 | Pembagian lapisan mendukung pengembangan Katalog Web, termasuk penambahan filter status dan lokasi rak. |
| P-01 Layered | T-01 | API Gateway menjadi satu titik untuk health check dan pengalihan permintaan ke instans layanan yang sehat. |
| P-02 Publish–Subscribe | SK-02 dan T-02 | Circulation Service menerbitkan event perubahan status buku melalui RabbitMQ untuk diterima Index Updater dan Notification Service. |

Pada U5, kedua pola tersebut direpresentasikan dalam diagram arsitektur. Layered Architecture ditunjukkan melalui pembagian Presentation, API Gateway, Business Logic, Data Access, dan Integration. Sementara itu, Publish–Subscribe ditunjukkan melalui hubungan komunikasi antara Circulation Service, RabbitMQ, Index Updater, dan Notification Service.

Selanjutnya, penerapan pola Publish–Subscribe diusulkan untuk didokumentasikan dalam ADR-UTS-001 (P-02), sedangkan keputusan penerapan T-01 yang berkaitan dengan API Gateway, instans layanan, dan PostgreSQL diusulkan dalam ADR-UTS-002.

Dengan demikian, kedua pola arsitektur memiliki keterkaitan dengan kebutuhan sistem, taktik arsitektur, rancangan diagram, serta keputusan desain yang akan digunakan dalam pengembangan Student Learning Platform – Layanan Perpustakaan UMI.


