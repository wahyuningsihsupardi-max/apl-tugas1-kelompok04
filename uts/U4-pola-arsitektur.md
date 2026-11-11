# U4. Pola Arsitektur

**BAB III: Taktik dan Pola (bagian pola)**  
**Penanggung jawab:** Andi Tsabitha Hulwah

**Kategori:** Struktur atau pembagian lapisan (modul).

## 3.2 Pola Arsitektur (U4)

Pola arsitektur adalah solusi teruji untuk masalah desain yang berulang dalam konteks tertentu [1], [5]. Student Learning Platform – Layanan Perpustakaan UMI menerapkan dua pola dari dua kategori berbeda: P-01 Layered dari kategori struktur atau pembagian modul, dan P-02 Publish–Subscribe dari kategori komunikasi berbasis event (komponen-dan-konektor) [1].

### Tabel 3.2 Ringkasan Pola yang Diterapkan

| Pola | Kategori | Skenario / Taktik yang Didukung | Elemen yang Menerapkan (Gambar 4.2) |
|---|---|---|---|
| P-01 Layered | Struktur / pembagian lapisan (modul) | SK-03 dan T-03; menjadi kerangka penempatan T-01 | Presentation, API Gateway, Business Logic, Data Access & Integration (sama dengan struktur modul B3) |
| P-02 Publish–Subscribe | Komunikasi berbasis event (C&C) | SK-02 dan T-02 (masalah utama); juga notifikasi pengingat | Circulation Service (penerbit), Message Queue RabbitMQ, Index Updater dan Notification Service (pelanggan) |

### 3.2.1 P-01 – Layered (Kategori Struktur)

**Definisi**

Sistem dibagi menjadi lapisan yang masing-masing memiliki tanggung jawab tertentu. Lapisan atas hanya boleh menggunakan lapisan tepat di bawahnya [1], [2].

**Alasan dan Cara Diterapkan**

Struktur modul B3 sudah membagi sistem menjadi Presentation, Business Logic, Data Access, dan Integration. Pola ini menjadikannya aturan: Katalog Web / Panel Sirkulasi → API Gateway → Catalog Service / Circulation Service → Repository → PostgreSQL, dengan Elasticsearch sebagai indeks katalog.

Aturan peminjaman (maksimal 5 buku, 7 hari, perpanjangan 1 kali, denda Rp1.000/hari) dikumpulkan di Circulation Service.

| Kelebihan pada Layanan Ini | Kekurangan pada Layanan Ini |
|---|---|
| Tampilan Katalog Web dapat diperbaiki (filter status dan lokasi rak, T-03, untuk keluhan R5) tanpa menyentuh logika peminjaman. | Setiap permintaan melewati beberapa lapisan sehingga ada tambahan latensi; pencarian harus tetap kurang dari 2 detik. |
| Jika kebijakan perpustakaan berubah, cukup Circulation Service yang diubah; perubahan tidak menyebar ke lapisan lain. | Godaan melompati lapisan (misalnya laporan langsung membaca PostgreSQL tanpa Repository) dapat merusak aturan bila tidak diawasi. |
| API Gateway menjadi satu titik untuk health check dan pengalihan ke instans yang sehat (T-01). | Perubahan yang menyentuh semua lapisan (misalnya kolom baru lokasi rak rinci) harus dikerjakan di beberapa lapisan sekaligus. |

### 3.2.2 P-02 – Publish–Subscribe (Kategori Komunikasi)

**Definisi**

Komponen penerbit mengirim event ke perantara tanpa mengetahui siapa penerimanya, sedangkan komponen pelanggan menerima event yang mereka minati [1], [5].

**Alasan dan Cara Diterapkan**

Masalah utama muncul karena status buku di katalog tidak ikut berubah saat transaksi dicatat. Dengan pola ini, setiap kali Circulation Service mengubah status eksemplar (dipinjam, dikembalikan, diperpanjang, dipindah rak), ia menerbitkan event StatusBukuBerubah ke Message Queue (RabbitMQ), yang di B3 sudah dipakai untuk Notification Service.

Index Updater (bagian Catalog Service) berlangganan event tersebut untuk memperbarui Elasticsearch, sedangkan Notification Service berlangganan untuk mengirim pengingat jatuh tempo (kebutuhan R4).

Agar event tidak hilang, status dan event disimpan dalam satu transaksi PostgreSQL, lalu diteruskan ke RabbitMQ. Teknik ini dikenal sebagai Transactional Outbox [6].

| Kelebihan pada Layanan Ini | Kekurangan pada Layanan Ini |
|---|---|
| Status di Katalog Web ikut berubah otomatis maksimal 5 detik setelah transaksi di Panel Sirkulasi, tanpa input ulang oleh petugas (langsung menjawab masalah utama). | Potensi keterlambatan sinkronisasi: ada jeda singkat (target maksimal 5 detik) ketika katalog belum menampilkan status terbaru. |
| Circulation Service tidak perlu tahu siapa pemakai event; menambah pelanggan baru (misalnya katalog FIKOM) tidak mengubah kode sirkulasi. | Beban dan peran RabbitMQ bertambah (dari hanya notifikasi menjadi jalur status), sehingga harus dipantau Tim IT dengan anggaran terbatas. |
| Peminjaman tetap berhasil walaupun Catalog Service atau Elasticsearch sedang gangguan; event menunggu di RabbitMQ (mendukung SK-01). | Alur lebih sulit dilacak dan diuji karena berjalan asinkron; perlu log yang menghubungkan transaksi dengan event-nya. |

### 3.2.3 Perbedaan Gaya dan Pola Arsitektur

Gaya arsitektur (style) menggambarkan karakter umum cara seluruh sistem diorganisasikan: topologi, pembagian komponen, dan cara penerapannya [2]. Pola arsitektur adalah solusi berulang yang lebih spesifik untuk satu masalah pada bagian tertentu sistem. Satu gaya dapat memuat beberapa pola.

Pada kelompok ini, gaya keseluruhannya adalah Service-Based Architecture [2]: beberapa layanan domain berbutir kasar (Catalog Service, Circulation Service, Digital Collection Service, Notification Service, Auth Service) yang berbagi infrastruktur data. P-02 hanya menyelesaikan satu masalah di dalam gaya itu, yaitu penyebaran perubahan status buku.

### Tabel 3.3 Perbedaan Gaya dan Pola pada Student Learning Platform – Layanan Perpustakaan UMI

| Aspek | Gaya: Service-Based Architecture | Pola: P-02 Publish–Subscribe |
|---|---|---|
| Cakupan | Seluruh sistem perpustakaan. | Satu interaksi: penyebaran perubahan status buku. |
| Pertanyaan yang Dijawab | Sistem dibagi menjadi bagian apa saja dan bagaimana dipasang? | Bagaimana katalog tahu status buku berubah tanpa memanggil Circulation Service? |
| Akibat Jika Diganti | Seluruh struktur, tim, dan penerapan ikut berubah (mahal). | Hanya jalur event yang berubah; layanan lain tetap. |

**Hubungan dengan Taktik, Diagram, dan ADR**

P-01 menjadi tempat T-03 (Katalog Web di Presentation) dan T-01 (API Gateway, instans layanan, dan PostgreSQL). P-02 adalah wujud T-02. Keduanya terlihat pada Gambar 4.2 dan diusulkan dicatat sebagai ADR-UTS-001 (P-02) dan ADR-UTS-002 (T-01) di U7.



