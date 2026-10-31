
# A5. Keputusan Arsitektur dan ADR

**Studi Kasus:** Student Learning Platform (Subsistem II-4) — Layanan Perpustakaan

**Penanggung Jawab :** Annisa Meyti Friska Manurung

Ruang lingkup: pencarian katalog, peminjaman, perpanjangan, dan akses koleksi digital.

_Materi acuan: Pertemuan 5 — Konteks Arsitektur & Keputusan Arsitektur_

---

## 7. Keputusan Arsitektur

Keputusan arsitektur dibuat paling awal dan paling sulit diubah nanti, serta menjadi batasan bagi pekerjaan seluruh tim. Karena itu keputusan perlu dijelaskan alasannya, dicatat, dan disampaikan dengan baik. Semakin ke arah arsitektur, perubahan semakin sulit dilakukan.

### 7.1 Peran Arsitek dalam Keputusan Arsitektur

Membuat keputusan arsitektur adalah tugas inti arsitek. Ada empat langkah yang harus dijalankan:

1. Kumpulkan informasi — pastikan informasi cukup sebelum memutuskan.

1. Jelaskan alasan — tuliskan justifikasi keputusan.

1. Dokumentasikan — catat keputusan agar dapat dibaca kembali kapan saja.

1. Komunikasikan — sampaikan dengan jelas kepada stakeholder yang tepat.

### 7.2 Keputusan Teknologi dan Keputusan Arsitektur

Tidak semua keputusan teknologi merupakan keputusan arsitektur. Sebuah keputusan teknologi menjadi keputusan arsitektur bila memengaruhi karakteristik sistem (misalnya kinerja atau skalabilitas); jika tidak, itu keputusan teknis biasa.

| Keputusan Teknologi | Menjadi Keputusan Arsitektur? | Alasan |
|---|---|---|
| Pemakaian mesin pencari khusus (Elasticsearch) untuk katalog | Ya | Menentukan pencapaian kinerja dan relevansi pencarian, serta menambah komponen infrastruktur baru |
| Penyimpanan berkas digital pada object storage dengan CDN | Ya | Memengaruhi skalabilitas, ketersediaan, dan cara koleksi disajikan kepada pengguna |
| Pemilihan basis data relasional untuk transaksi sirkulasi | Ya | Memengaruhi keandalan dan ketepatan pencatatan peminjaman serta denda |
| Pemilihan pustaka pemindai barcode di meja sirkulasi | Tidak | Tidak mengubah struktur maupun karakteristik sistem secara signifikan |

### 7.3 Menentukan Keputusan yang Signifikan Secara Arsitektural

Suatu keputusan disebut signifikan secara arsitektural bila memengaruhi struktur, karakteristik nonfungsional, dependensi, antarmuka, atau teknik konstruksi.

| Kategori | Maksudnya | Contoh pada Sistem Ini |
|---|---|---|
| Struktur | Memengaruhi pola atau gaya arsitektur | Pemisahan modul katalog (baca-berat) dari modul sirkulasi (transaksional) sehingga keduanya dapat diskalakan terpisah |
| Karakteristik nonfungsional | Mutu penting bagi sistem, seperti kinerja dan keamanan | Pemilihan mesin pencari menentukan kinerja; mekanisme akses koleksi digital menentukan keamanan dan kepatuhan lisensi |
| Dependensi | Cara komponen atau layanan saling bergantung | Ketergantungan pada SIAKAD, SSO UMI, dan penyedia koleksi digital berdampak pada ketersediaan dan pengujian |
| Antarmuka | Cara layanan dan komponen diakses dan diatur | Penetapan API Gateway dan kontrak OAI-PMH untuk pertukaran metadata, termasuk versi dan strategi penghentiannya |
| Teknik konstruksi | Platform, kerangka kerja, alat, dan proses | Penggunaan container dan pipeline deployment otomatis |

## 8. Anti-Pattern Keputusan Arsitektur

Anti-pattern adalah cara yang awalnya tampak baik tetapi justru menimbulkan masalah (Koenig): pola berulang dengan hasil buruk. Richards & Ford (Bab 19) menyebut tiga anti-pattern keputusan arsitektur yang harus dihindari.

| Anti-Pattern | Pengertian | Dampak | Solusi |
|---|---|---|---|
| Covering Your Assets | Arsitek menghindari atau menunda keputusan karena takut salah memilih | Keputusan tertunda; tim menunggu dan kemajuan terhambat | Bekerja sama dengan tim; putuskan pada last responsible moment, yaitu saat informasi sudah cukup untuk mendukung keputusan |
| Groundhog Day | Keputusan dibuat tanpa alasan yang lengkap, sehingga dibahas berulang kali | Keputusan yang sama dibahas ulang dan hasilnya bisa berbeda; anggota baru tidak tahu alasannya | Tulis alasan lengkap setiap keputusan, baik alasan teknis maupun bisnis |
| Email-Driven Architecture | Keputusan disampaikan lewat email, sehingga mudah hilang atau tidak diketahui orang | Orang kehilangan email, lupa, atau tidak tahu bahwa keputusan sudah dibuat | Jangan menaruh keputusan di isi email. Kirim pemberitahuan singkat kepada pihak terkait, lengkap dengan tautan ke catatan terpusat |

### 8.1 Penerapan Pencegahan pada Sistem Ini

| Anti-Pattern | Risiko pada Proyek Ini | Tindakan Pencegahan |
|---|---|---|
| Covering Your Assets | Arsitek menunda penetapan mekanisme penyajian koleksi digital karena menunggu kejelasan negosiasi lisensi dengan penerbit | Tetapkan keputusan pada last responsible moment dengan asumsi yang dinyatakan terbuka, lalu catat di ADR bahwa keputusan dapat ditinjau ulang setelah lisensi final |
| Groundhog Day | Pembahasan ulang pilihan mesin pencari setiap kali ada pustakawan atau anggota tim baru yang mempertanyakan kinerja katalog | ADR-002 memuat konteks, hasil uji, dan alasan lengkap sehingga keputusan tidak perlu dibahas ulang |
| Email-Driven Architecture | Kesepakatan format metadata dengan konsorsium perpustakaan hanya tersimpan di utas email | Seluruh keputusan disimpan di satu direktori ADR terpusat; email hanya berisi pemberitahuan beserta tautan |

## 9. Architecture Decision Record (ADR)

ADR adalah berkas teks pendek (sekitar 1–2 halaman) untuk satu keputusan. Praktik ini diusulkan Michael Nygard (2011) dan ditandai "adopt" pada ThoughtWorks Technology Radar. ADR tidak dihapus; jika diganti, statusnya diubah menjadi superseded. Keputusan yang perlu dicatat adalah keputusan yang penting, berisiko, mahal, atau berdampak jangka panjang.

Format yang digunakan: Title, Status (Proposed, Accepted, Superseded), Context, Decision, Consequences, Compliance, Notes. Seluruh ADR disimpan dalam satu direktori terpusat dan dikelompokkan (misalnya application, integration, enterprise), serta setiap anggota tim harus tahu ke mana mencarinya.

# ADR-001: Penerapan Database Indexing dan Caching Terpusat untuk Kinerja Pencarian Katalog

- **Status:** Diterima (*Accepted*)
- **Tanggal:** 31 Oktober 2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

## Context
Mahasiswa sering melakukan pencarian buku secara bersamaan berdasarkan judul, penulis, atau ISBN pada jam sibuk perkuliahan[cite: 3]. Berdasarkan **ASR-1 (Skenario Performa)** dan target **B2**, sistem dituntut mampu menampilkan hasil pencarian katalog dengan waktu tanggap (*response time*) kurang dari 2 detik dalam kondisi beban normal[cite: 3]. Selain itu, terdapat batasan bahwa data buku dan transaksi harus dikelola secara terstruktur dan terintegrasi[cite: 3].

## Decision
Kami akan menerapkan indeks pencarian (*database indexing*) pada kolom-kolom utama tabel buku (seperti judul, penulis, ISBN, dan kategori) serta mengimplementasikan lapisan *in-memory caching* terpusat (menggunakan Redis) untuk menyimpan kueri pencarian katalog yang sering diakses oleh mahasiswa.

## Consequences
- **Dampak Positif (Keuntungan):**
  - Mempercepat waktu tanggap pencarian katalog hingga di bawah 2 detik sesuai target ASR-1[cite: 3].
  - Mengurangi beban langsung (*read load*) pada basis data utama saat terjadi lonjakan trafik pencarian[cite: 3].
- **Dampak Negatif & Trade-off:**
  - Membutuhkan konsumsi memori (RAM) tambahan pada infrastruktur server *cache*.
  - Menambah kompleksitas pemeliharaan sistem (*maintainability*) karena perlunya mekanisme *cache invalidation* agar data buku baru atau perubahan status peminjaman segera terbarui di lapisan *cache*[cite: 3].

## Compliance
- Melakukan pengujian kinerja (*load testing*) menggunakan *tool* simulasi beban (seperti JMeter atau k6) untuk memastikan waktu respon pencarian tetap < 2 detik pada kondisi beban normal.
- Pemantauan otomatis (*monitoring*) tingkat pemanfaatan *cache* (*cache hit ratio*) secara berkala.

---

# ADR-002: Arsitektur Modul Transaksi Terpusat (ACID) untuk Manajemen Peminjaman

- **Status:** Diterima (*Accepted*)
- **Tanggal:** 31 Oktober 2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

## Context
Berdasarkan hasil wawancara narasumber R3 dan R4, ditemukan kendala di mana data katalog di sistem belum selalu sesuai dengan ketersediaan fisik di rak (buku tercantum tersedia padahal sedang dipinjam)[cite: 3, 7, 8]. **ASR-3 (Skenario Keandalan)** dan target **B2 (Functional Correctness)** menuntut minimal 95% ketepatan data transaksi untuk mencegah transaksi peminjaman ganda (*double borrowing*) serta menjaga konsistensi status ketersediaan buku secara otomatis[cite: 3].

## Decision
Kami akan menerapkan komponen **Modul Transaksi Terpusat** yang memanfaatkan transaksi basis data terisolasi (memenuhi prinsip *Atomicity, Consistency, Isolation, Durability* / ACID) serta menerapkan mekanisme *pessimistic locking* saat proses peminjaman atau perpanjangan buku berlangsung.

## Consequences
- **Dampak Positif (Keuntungan):**
  - Menjamin integritas data (*Data Integrity*) dan ketepatan informasi (*Functional Correctness*) ketersediaan buku secara konsisten[cite: 3].
  - Mencegah terjadinya peminjaman ganda pada satu eksemplar buku yang sama dalam waktu yang bersamaan[cite: 3].
- **Dampak Negatif & Trade-off:**
  - Mekanisme *locking* pada level basis data dapat sedikit menurunkan *throughput* transaksi pada saat beban puncak.
  - Membutuhkan penanganan *timeout* transaksi yang ketat agar tidak menimbulkan kondisi *deadlock*.

## Compliance
- Tinjauan kode (*code review*) wajib memeriksa pengaplikasian blok transaksi basis data pada setiap fungsi peminjaman dan perpanjangan buku.
- Pengujian otomatis (*unit & integration testing*) khusus untuk skenario konkurensi (dua akun meminjam eksemplar buku yang sama secara simultan).
- Audit berkala terhadap kesesuaian data katalog versus catatan fisik transaksi (menguji sampel 100 buku sesuai kriteria B2)[cite: 3].

---

# ADR-003: Penerapan Layanan Notifikasi Terpusat (Asynchronous Message Queue) untuk Status Peminjaman

- **Status:** Diterima (*Accepted*)
- **Tanggal:** 31 Oktober 2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

## Context
Mahasiswa (R4) membutuhkan fitur pengingat batas waktu pengembalian buku untuk menghindari denda keterlambatan[cite: 8]. **ASR-4 (Skenario Penelusuran dan Keandalan)** mengharuskan notifikasi konfirmasi peminjaman, perpanjangan, dan tenggat waktu terkirim secara otomatis kepada mahasiswa dalam waktu kurang dari 5 detik setelah pemicu/perubahan status transaksi berhasil diproses[cite: 3].

## Decision
Kami akan menggunakan arsitektur pemrosesan latar belakang berbasis antrean pesan (*asynchronous message queue* menggunakan RabbitMQ/Redis Queue) untuk mengelola **Layanan Notifikasi Terpusat** yang mengirimkan pemberitahuan via notifikasi aplikasi maupun email.

## Consequences
- **Dampak Positif (Keuntungan):**
  - Eksekusi transaksi peminjaman utama tidak terhambat (*non-blocking*) oleh proses pengiriman email/notifikasi yang lambat.
  - Target penyampaian notifikasi < 5 detik setelah perubahan status transaksi dapat dicapai secara konsisten[cite: 3].
- **Dampak Negatif & Trade-off:**
  - Menambah komponen infrastruktur berupa *message broker* yang perlu dikelola dan dipantau oleh tim IT[cite: 3].

## Compliance
- Melakukan pemantauan waktu antrean (*queue latency monitoring*) dan *log* kegagalan pengiriman notifikasi.
- Pengujian otomatis (*automated testing*) untuk memastikan perubahan status peminjaman memicu *worker* pengirim notifikasi dengan tepat.

---

# ADR-004: Perancangan Antarmuka Berbasis Dashboard Terpusat Berprinsip Progressive Disclosure untuk Usability

- **Status:** Diterima (*Accepted*)
- **Tanggal:** 31 Oktober 2026
- **Pengambil keputusan:** Annisa Meyti Friska Manurung, Besse Armelya Cindy Damayanti M, Wahyuningsih S., Andi Tsabitha Hulwah (Kelompok II-04)

## Context
Pengguna (R5) kerap merasa bingung dengan tampilan katalog digital jika terlalu banyak informasi yang disajikan sekaligus di satu layar[cite: 9]. Di sisi lain, **ASR-2 (Skenario Kebergunaan)** dan target **B2 (Usability)** menetapkan bahwa minimal 90% mahasiswa harus dapat menemukan informasi buku (judul, status, dan lokasi rak) dalam waktu kurang dari 2 menit (atau maksimal 3 kali klik) tanpa bantuan petugas[cite: 3].

## Decision
Kami akan merancang antarmuka pengguna (UI) berbasis **Dashboard Terpusat** dengan menerapkan pola desain *Progressive Disclosure*, yaitu menampilkan informasi utama (judul, penulis, status ketersediaan ringkas) terlebih dahulu di halaman pencarian utama, dan menempatkan detail lengkap (lokasi rak rinci, deskripsi buku) pada tampilan sekunder/pop-up.

## Consequences
- **Dampak Positif (Keuntungan):**
  - Menyederhanakan alur pencarian, meningkatkan *learnability* dan *operability* sesuai standar ISO/IEC 25010[cite: 3].
  - Membantu mahasiswa menemukan lokasi dan ketersediaan buku dalam batas waktu < 2 menit dan < 3 kali klik[cite: 3].
- **Dampak Negatif & Trade-off:**
  - Trade-off Usability vs Functional Correctness: Penyederhanaan tampilan mewajibkan pengguna melakukan klik ekstra untuk melihat detail spesifik posisi rak buku[cite: 3].

## Compliance
- Pelaksanaan pengujian kebergunaan berbasis tugas (*task-based usability testing*) terhadap 10 sampel mahasiswa sesuai skenario uji pada dokumen B2[cite: 3].
- Evaluasi desain antarmuka melalui tinjauan prototipe UI/UX sebelum tahap implementasi frontend[cite: 3].
