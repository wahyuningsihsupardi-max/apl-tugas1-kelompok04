## ADR-001: Penerapan Database Indexing dan Caching Terpusat untuk Kinerja Pencarian Katalog

- **Status:** Diterima
- **Tanggal:** 31-10-2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

### Context
Mahasiswa sering melakukan pencarian buku secara bersamaan berdasarkan judul, penulis, atau ISBN pada jam sibuk perkuliahan[cite: 3]. Berdasarkan **ASR-1 (Skenario Performa)**, sistem dituntut mampu menampilkan hasil pencarian katalog dengan waktu tanggap (*response time*) kurang dari 2 detik dalam kondisi beban normal[cite: 3]. Selain itu, terdapat batasan bahwa data buku dan transaksi harus dikelola secara terstruktur dan terintegrasi[cite: 3].

### Decision
Kami akan menerapkan indeks pencarian (*database indexing*) pada kolom-kolom utama tabel buku (seperti judul, penulis, ISBN, dan kategori) serta mengimplementasikan lapisan *in-memory caching* terpusat (menggunakan Redis/Memcached) untuk menyimpan query pencarian yang sering diakses oleh mahasiswa.

### Consequences
- **Dampak Positif:** 
  - Mempercepat waktu tanggap pencarian katalog hingga di bawah 2 detik sesuai target ASR-1[cite: 3].
  - Mengurangi beban langsung (*read load*) pada basis data utama saat terjadi lonjakan trafik pencarian[cite: 3].
- **Dampak Negatif & Trade-off:** 
  - Membutuhkan konsumsi memori (RAM) tambahan untuk server *cache*.
  - Menambah kompleksitas pemeliharaan sistem (*maintainability*) karena perlunya mekanisme *cache invalidation* agar data buku baru atau perubahan status peminjaman segera terbarui[cite: 3].

### Compliance
- Melakukan pengujian kinerja (*load testing*) menggunakan *tool* simulasi beban (seperti JMeter atau k6) untuk memastikan waktu respon pencarian tetap < 2 detik pada kondisi beban normal.
- Pemantauan otomatis (*monitoring*) tingkat pemanfaatan *cache* (*cache hit ratio*) secara berkala.

---

## ADR-002: Arsitektur Modul Transaksi Terpusat dengan Database Transaction (ACID) untuk Manajemen Peminjaman

- **Status:** Diterima
- **Tanggal:** 31-10-2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

### Context
Berdasarkan hasil wawancara R3 dan R4, ditemukan kendala di mana data katalog di sistem belum selalu sesuai dengan ketersediaan fisik di rak (buku tercantum tersedia padahal sedang dipinjam)[cite: 3, 7, 8]. **ASR-3 (Skenario Keandalan)** dan target **B2 (Functional Correctness)** menuntut minimal 95% ketepatan data transaksi untuk mencegah transaksi peminjaman ganda (*double borrowing*) serta menjaga konsistensi status buku secara otomatis[cite: 3].

### Decision
Kami akan menerapkan komponen **Modul Transaksi Terpusat** yang memanfaatkan transaksi basis data terisolasi (memenuhi prinsip *Atomicity, Consistency, Isolation, Durability* / ACID) serta menerapkan mekanisme *pessimistic locking* saat proses peminjaman/perpanjangan buku berlangsung.

### Consequences
- **Dampak Positif:** 
  - Menjamin integritas data (*Data Integrity*) dan ketepatan informasi (*Functional Correctness*) ketersediaan buku secara konsisten[cite: 3].
  - Mencegah terjadinya peminjaman ganda pada satu eksemplar buku yang sama di waktu bersamaan[cite: 3].
- **Dampak Negatif & Trade-off:** 
  - *Locking* pada level basis data dapat sedikit menurunkan *throughput* transaksi pada saat beban puncak.
  - Membutuhkan penanganan *timeout* transaksi yang ketat agar tidak terjadi kondisi *deadlock*.

### Compliance
- Tinjauan kode (*code review*) wajib memeriksa pengaplikasian blok transaksi database pada setiap pustaka fungsi peminjaman dan perpanjangan.
- *Unit testing* dan *integration testing* khusus untuk skenario konkurensi (dua akun meminjam buku yang sama secara simultan).
- Audit kesesuaian data katalog versus data transaksi secara periodik (menguji sampel 100 buku sesuai kriteria B2)[cite: 3].

---

## ADR-003: Penerapan Layanan Notifikasi Terpusat (Asynchronous Event Notification) untuk Status Peminjaman

- **Status:** Diterima
- **Tanggal:** 31-10-2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

### Context
Mahasiswa (R4) membutuhkan fitur pengingat batas waktu pengembalian buku untuk menghindari denda keterlambatan[cite: 8]. **ASR-4 (Skenario Penelusuran dan Keandalan)** mengharuskan notifikasi konfirmasi peminjaman, perpanjangan, dan tenggat waktu terkirim secara otomatis kepada mahasiswa dalam waktu kurang dari 5 detik setelah pemicu/perubahan status transaksi[cite: 3].

### Decision
Kami akan menggunakan arsitektur pemrosesan latar belakang berbasis antrean pesan (*asynchronous message queue*) untuk mengelola **Layanan Notifikasi Terpusat** yang mengirimkan pemberitahuan via notifikasi aplikasi maupun email.

### Consequences
- **Dampak Positif:** 
  - Eksekusi transaksi peminjaman utama tidak terhambat (*non-blocking*) oleh proses pengiriman email/notifikasi yang lambat.
  - Target penyampaian notifikasi < 5 detik setelah perubahan status transaksi dapat dicapai secara konsisten[cite: 3].
- **Dampak Negatif & Trade-off:** 
  - Menambah infrastruktur layanan berupa *message broker* (seperti RabbitMQ/Redis Queue) yang perlu dikelola oleh tim IT[cite: 3].

### Compliance
- Melakukan pemantauan waktu antrean (*queue latency monitoring*) dan *error log* pengiriman notifikasi.
- Pengujian otomatis (*automated testing*) untuk memastikan event peminjaman/perpanjangan memicu *worker* pengirim notifikasi dengan tepat.

---

## ADR-004: Perancangan Antarmuka Berbasis Dashboard Terpusat Berprinsip Progressive Disclosure untuk Usability

- **Status:** Diterima
- **Tanggal:** 31-10-2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

### Context
Pengguna (R5) kerap merasa bingung dengan tampilan katalog digital jika terlalu banyak informasi yang disajikan sekaligus[cite: 9]. Di sisi lain, **ASR-2 (Skenario Kebergunaan)** dan target **B2 (Usability)** menetapkan bahwa minimal 90% mahasiswa harus dapat menemukan informasi buku (judul, status, dan lokasi rak) dalam waktu kurang dari 2 menit (atau maksimal 3 kali klik) tanpa bantuan petugas[cite: 3].

### Decision
Kami akan merancang antarmuka pengguna (UI) berbasis **Dashboard Terpusat** dengan menerapkan pola desain *Progressive Disclosure*, yaitu menampilkan informasi utama (judul, penulis, status ketersediaan ringkas) terlebih dahulu di halaman pencarian utama, dan menempatkan detail lengkap (lokasi rak rinci, deskripsi) pada tampilan sekunder/pop-up.

### Consequences
- **Dampak Positif:** 
  - Menyederhanakan alur pencarian, meningkatkan *learnability* dan *operability* sesuai standar ISO/IEC 25010[cite: 3].
  - Membantu mahasiswa menemukan lokasi dan ketersediaan buku dalam batas waktu < 2 menit dan < 3 kali klik[cite: 3].
- **Dampak Negatif & Trade-off:** 
  - Trade-off Usability vs Functional Correctness: Jika penyederhanaan terlalu ekstrem, pengguna harus melakukan klik ekstra untuk melihat detail lokasi rak buku[cite: 3].

### Compliance
- Pelaksanaan pengujian kebergunaan berbasis tugas (*task-based usability testing*) terhadap 10 sampel mahasiswa sesuai skenario uji pada dokumen B2[cite: 3].
- Evaluasi desain antarmuka melalui tinjauan prototipe UI/UX sebelum tahap implementasi frontend[cite: 3].


