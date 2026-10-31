# BAB IV. Penutup

**Penanggung Jawab:** Andi Tsabitha Hulwah 

## 4.1 Kesimpulan

Berdasarkan hasil analisis, pengelompokan kebutuhan, perancangan arsitektur, dan evaluasi kualitas pada pengembangan *Student Learning Platform — Layanan Perpustakaan UMI*, dapat ditarik beberapa kesimpulan utama sebagai berikut:

1. **Integrasi Konteks Arsitektur dan Kebutuhan Pemangku Kepentingan:**
   Analisis komprehensif terhadap empat konteks arsitektur (teknis, bisnis, siklus hidup proyek, dan profesional) serta pemetaan 6 kelompok *stakeholder* (Pengelola Perpustakaan UMI, Tim Pengembang, Staf PUBA, Staf FIKOM, Mahasiswa, dan Penyedia Koleksi Digital) berhasil mengidentifikasi akar permasalahan utama pada layanan perpustakaan saat ini. Hal tersebut mencakup belum lengkapnya pendataan katalog di tingkat fakultas (FIKOM), keterbatasan infrastruktur server/jaringan yang memicu kendala akses, serta belum terintegrasinya status peminjaman secara *real-time*.

2. **Karakteristik Pendorong dan Target Kualitas Terukur:**
   Tiga karakteristik pendorong utama (*architecturally significant characteristics*) yang disepakati untuk mengarahkan rancangan sistem adalah **Availability** (Ketersediaan Layanan), **Functional Correctness** (Ketepatan Fungsi dan Informasi), dan **Usability** (Kemudahan Penggunaan). Seluruh karakteristik ini telah dipetakan ke dalam standar ISO/IEC 25010 (*Reliability*, *Functional Suitability*, dan *Usability*) dan memiliki target atribut mutu terukur (B2), yaitu:
   * **Availability:** Ketersediaan layanan pencarian katalog dan peminjaman minimal 99% dalam pemantauan 7 hari berturut-turut.
   * **Functional Correctness:** Tingkat kesesuaian data katalog (judul, status peminjaman, dan lokasi rak fisik) minimal 95% dari 100 sampel buku yang diuji.
   * **Usability:** Minimal 90% mahasiswa berhasil menemukan informasi buku dan lokasi rak secara mandiri dalam waktu maksimal 2 menit (atau < 3 kali klik) tanpa bantuan petugas.

3. **Keputusan Arsitektur Terstruktur (ADR) dan Pencegahan Anti-Pattern:**
   Guna memenuhi Kebutuhan Signifikan Arsitektur (ASR A4) dan batasan sistem, empat Keputusan Arsitektur (*Architecture Decision Records*) telah ditetapkan secara transparan:
   * **ADR-001 (*Database Indexing & Caching*):** Menerapkan indeks tabel dan Redis *in-memory cache* untuk menjamin waktu tanggap pencarian katalog < 2 detik pada jam sibuk perkuliahan (ASR-1).
   * **ADR-002 (*Modul Transaksi Terpusat ACID*):** Menggunakan transaksi basis data terisolasi dan *pessimistic locking* untuk mencegah peminjaman ganda (*double borrowing*) serta menjaga integritas data ketersediaan koleksi (ASR-3).
   * **ADR-003 (*Asynchronous Message Queue*):** Memanfaatkan antrean pesan RabbitMQ/Redis Queue untuk pengiriman notifikasi pengingat tenggat peminjaman secara otomatis < 5 detik tanpa mengganggu transaksi utama (ASR-4).
   * **ADR-004 (*UI Dashboard Progressive Disclosure*):** Merancang antarmuka terpusat yang menyederhanakan alur pencarian guna meningkatkan *learnability* dan *operability* pengguna (ASR-2).
   Dokumentasi terpusat pada folder `/adr` ini secara langsung mencegah timbulnya tiga *anti-pattern* perancangan (*Covering Your Assets*, *Groundhog Day*, dan *Email-Driven Architecture*).

4. **Keseimbangan dan Strategi Pengelolaan Trade-off:**
   Analisis *trade-off* membuktikan bahwa ketiga karakteristik pendorong saling memengaruhi secara dinamis. Upaya meningkatkan ketersediaan (*Availability*) dan ketepatan data (*Functional Correctness*) membutuhkan sumber daya server, jaringan, dan proses validasi data yang lebih ketat, yang berpotensi membebani operasional staf. Di sisi lain, penyederhanaan antarmuka (*Usability*) yang berlebihan berisiko mengaburkan informasi rinci lokasi rak buku. Strategi pengelolaan *trade-off* yang disepakati berhasil menyelaraskan ketiga kebutuhan tersebut secara seimbang agar sistem tetap stabil, akurat, dan mudah digunakan.

---

## 4.2 Saran

Untuk memastikan rancangan arsitektur *Student Learning Platform — Layanan Perpustakaan UMI* dapat diimplementasikan dengan sukses dan berkelanjutan, disarankan beberapa langkah teknis dan operasional sebagai berikut:

1. **Rekomendasi Teknis dan Pengujian Sistem:**
   * **Pelaksanaan *Load & Stress Testing* Kinerja:** Tim pengembang wajib melakukan pengujian beban (*load testing*) menggunakan *tools* seperti Apache JMeter atau k6 untuk menguji ketahanan lapisan *caching* (Redis) dan server basis data saat diakses oleh ratusan pengguna secara simultan pada jam puncak perkuliahan.
   * **Pengujian Transaksi Konkurensi:** Melakukan *integration testing* terisolasi pada Modul Transaksi untuk memvalidasi bahwa mekanisme *pessimistic locking* berhasil menggagalkan simulasi dua akun yang mencoba meminjam eksemplar buku yang sama di detik yang sama.
   * **Pengujian Kebergunaan Berbasis Tugas (*Task-Based Usability Testing*):** Melibatkan sampel 10 mahasiswa dari berbagai fakultas untuk menyelesaikan skenario pencarian buku pada prototipe antarmuka, guna mengukur ketercapaian target waktu (< 2 menit) dan jumlah klik (< 3 kali).

2. **Rekomendasi Manajerial dan Operasional:**
   * **Validasi dan Pembersihan Data Koleksi (*Data Cleansing*):** Pihak pengelola perpustakaan pusat (PUBA) dan tim IT fakultas (FIKOM UMI) disarankan untuk segera menyelesaikan pendataan dan verifikasi data fisik koleksi sebelum proses migrasi data ke sistem baru, guna menjamin ketepatan data katalog mencapai target 95% sejak hari pertama penerapan.
   * **Integrasi Katalog Sentralisasi Fakultas-Pusat:** Mengembangkan antarmuka integrasi (API) yang menghubungkan pustaka fakultas dengan basis data pusat PUBA agar mahasiswa FIKOM tidak perlu lagi melakukan pencarian secara terpisah atau manual.

3. **Pemeliharaan Dokumentasi dan Tata Kelola Repositori:**
   * **Pengelolaan ADR Kontinu:** Tim IT disarankan untuk tetap memperbarui dan mencatat setiap perubahan keputusan teknis di masa mendatang ke dalam folder `/adr` di repositori GitHub kelompok, dengan tetap menerapkan perubahan status (*Proposed* $\rightarrow$ *Accepted* $\rightarrow$ *Superseded*) untuk menjaga transparansi perancangan.
   * **Evaluasi Berkala Pasca-Penerapan:** Menggunakan masukan berkala dari mahasiswa dan petugas perpustakaan sebagai bahan evaluasi atribut kualitas terukur setiap semester.
