
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

### ADR-001: Pemisahan Modul Katalog dan Sirkulasi

| Field | Isi |
|---|---|
| Title | Pemisahan Modul Katalog dan Modul Sirkulasi |
| Status | Accepted |
| Context | Beban kerja kedua fungsi sangat berbeda. Pencarian katalog bersifat baca-berat dan melonjak tajam menjelang ujian, sedangkan peminjaman dan perpanjangan bersifat transaksional dengan volume jauh lebih kecil namun menuntut ketepatan pencatatan. Menyatukan keduanya dalam satu unit membuat lonjakan pencarian berisiko mengganggu transaksi sirkulasi. |
| Decision | Memisahkan sistem menjadi modul Katalog, Sirkulasi, Koleksi Digital, Notifikasi, dan Autentikasi, dengan API Gateway sebagai pintu masuk tunggal. Modul Katalog dapat diskalakan terpisah dari modul Sirkulasi. |
| Consequences | Perlu pengelolaan dua jalur skalabilitas dan pemantauan terpusat. Sebagai imbalannya, lonjakan pencarian pada musim ujian tidak lagi mengancam transaksi peminjaman, dan kegagalan modul Katalog tidak menghentikan layanan sirkulasi di meja perpustakaan. |
| Compliance | Diperiksa melalui uji beban terpisah: beban pencarian dinaikkan hingga batas puncak sementara transaksi sirkulasi harus tetap berhasil. |

### ADR-002: Mesin Pencari Khusus untuk Katalog

| Field | Isi |
|---|---|
| Title | Penggunaan Mesin Pencari Khusus (Elasticsearch) untuk Katalog |
| Status | Accepted |
| Context | Katalog UMI diperkirakan mencapai ratusan ribu entri dan akan terus bertambah melalui impor metadata. Pencarian berbasis kueri basis data relasional dengan pola pencocokan teks tidak mampu memberikan hasil yang relevan maupun waktu respons di bawah 2 detik, terutama ketika pengguna mengetikkan kata kunci yang tidak persis sama dengan data katalog. |
| Decision | Menggunakan Elasticsearch sebagai indeks pencarian katalog, dengan pengindeksan ulang otomatis setiap kali data bibliografi berubah, serta dukungan pencarian berdasarkan judul, pengarang, subjek, nomor panggil, dan kata kunci bebas. |
| Consequences | Bertambah satu komponen infrastruktur yang harus dipantau dan dipelihara, serta kebutuhan menjaga konsistensi antara basis data utama dan indeks pencarian. Keuntungannya, pencarian menjadi cepat dan toleran terhadap kesalahan pengetikan, serta dapat menampilkan penyaringan hasil berdasarkan tahun, jenis koleksi, dan ketersediaan salinan. |
| Compliance | Diperiksa melalui uji kinerja: hasil pencarian tampil di bawah 2 detik pada katalog berisi 100.000 judul dengan 100 pengguna bersamaan. |

### ADR-003: Object Storage dan CDN untuk Koleksi Digital

| Field | Isi |
|---|---|
| Title | Penyimpanan Koleksi Digital pada Object Storage dengan CDN |
| Status | Accepted |
| Context | Koleksi digital berupa berkas PDF dan gambar berukuran besar. Menyimpannya pada sistem berkas server aplikasi akan membebani server, menyulitkan pencadangan, dan membuat unduhan lambat bagi mahasiswa yang mengakses dari luar kampus dengan bandwidth terbatas. |
| Decision | Menyimpan berkas koleksi digital pada object storage dan menyajikannya melalui CDN dengan tautan berbatas waktu (signed URL) yang hanya diberikan kepada pengguna berhak. Berkas tidak pernah disalin ke perangkat pengguna secara massal; pembacaan daring diutamakan. |
| Consequences | Perlu pengelolaan kebijakan akses dan masa berlaku tautan, serta biaya penyimpanan dan lalu lintas CDN yang harus diperhitungkan dalam anggaran. Keuntungannya, server aplikasi tetap ringan, waktu unduh lebih singkat, dan kontrol akses terpusat sehingga kepatuhan terhadap lisensi lebih mudah dibuktikan. |
| Compliance | Diperiksa dengan menguji bahwa tautan kedaluwarsa dan pengguna tanpa hak menerima penolakan akses. |

### ADR-004: Integrasi SSO UMI dan Data Keanggotaan dari SIAKAD

| Field | Isi |
|---|---|
| Title | Integrasi Single Sign-On UMI dan Sinkronisasi Keanggotaan dari SIAKAD |
| Status | Accepted |
| Context | Hak akses perpustakaan hanya berlaku bagi mahasiswa dan dosen yang berstatus aktif. Data status akademik sudah dikelola di SIAKAD, sedangkan autentikasi terpusat sudah tersedia melalui SSO UMI. Mengelola data keanggotaan secara manual di perpustakaan akan menimbulkan ketidaksesuaian data dan beban kerja tambahan bagi pustakawan. |
| Decision | Mengintegrasikan autentikasi dengan SSO UMI melalui OAuth 2.0 / SAML, dan melakukan sinkronisasi berkala status keanggotaan dari SIAKAD. Peran (mahasiswa, dosen, pustakawan, admin) diambil dari data akademik dan digunakan untuk otorisasi. |
| Consequences | Terdapat ketergantungan pada ketersediaan layanan autentikasi dan data akademik kampus: bila layanan tersebut terganggu, akses perpustakaan ikut terdampak. Untuk mengurangi risiko, data keanggotaan disimpan sebagai salinan lokal dengan masa berlaku terbatas sehingga akses koleksi digital tetap dapat dilayani saat sinkronisasi tertunda. |
| Compliance | Diperiksa melalui uji peran dan uji penonaktifan: mahasiswa yang statusnya tidak aktif harus kehilangan hak akses setelah masa berlaku salinan lokal berakhir. |

### ADR-005: Message Queue untuk Notifikasi Jatuh Tempo dan Reservasi

| Field | Isi |
|---|---|
| Title | Message Queue untuk Notifikasi Jatuh Tempo, Keterlambatan, dan Ketersediaan Reservasi |
| Status | Accepted |
| Context | Notifikasi dikirim secara berkala kepada banyak pengguna sekaligus dan bergantung pada layanan email pihak ketiga yang dapat lambat atau gagal. Bila pengiriman dilakukan secara sinkron di dalam alur peminjaman, transaksi di meja sirkulasi berisiko ikut melambat pada jam sibuk. |
| Decision | Menggunakan message queue (RabbitMQ) agar modul Notifikasi mengonsumsi permintaan pengiriman secara asinkron dan dapat mencoba ulang ketika gagal. |
| Consequences | Perlu clustering dan pemantauan antrean agar tersedia tinggi, serta mekanisme pencegahan pengiriman ganda ketika percobaan ulang terjadi. Pesan internal tidak melewati pemeriksaan keamanan pada lapisan API — risiko ini diterima sebagai trade-off oleh dewan peninjau arsitektur (ARB) karena pesan hanya beredar di jaringan internal. Keuntungan: transaksi sirkulasi tetap responsif dan notifikasi yang gagal dapat diulang otomatis. |
| Compliance | Diperiksa melalui simulasi kegagalan layanan email: antrean harus tetap menampung pesan dan mengirimkannya setelah layanan pulih, tanpa menghasilkan notifikasi ganda. |

### ADR-006: Standar Metadata dan Interoperabilitas Katalog

| Field | Isi |
|---|---|
| Title | Standar Metadata Dublin Core / MARC dan Pertukaran Data melalui OAI-PMH |
| Status | Accepted |
| Context | Katalog lama UMI tidak konsisten: sebagian entri lengkap, sebagian hanya memuat judul dan pengarang. Di sisi lain, kerja sama dengan konsorsium perpustakaan menuntut pertukaran data bibliografi yang terstandar agar koleksi dapat ditemukan lintas perpustakaan. |
| Decision | Menetapkan Dublin Core sebagai format metadata internal minimum dan mempertahankan pemetaan ke MARC untuk data lama, serta menyediakan antarmuka OAI-PMH untuk memanen dan membagikan metadata dengan katalog eksternal. |
| Consequences | Perlu tahap pembersihan dan pemetaan metadata katalog lama, yang menambah pekerjaan awal dan memerlukan keterlibatan pustakawan. Sebagai imbalannya, sistem dapat bertukar data dengan perpustakaan lain, memudahkan impor massal, dan menyiapkan jalan untuk pemanfaatan bersama koleksi antar institusi. |
| Compliance | Diperiksa dengan menguji panen metadata melalui OAI-PMH dan memverifikasi bahwa hasil pemetaan dapat dibaca oleh sistem katalog eksternal. |

### 9.1 Siklus Hidup Status ADR

| Status | Arti | Kapan Digunakan |
|---|---|---|
| Proposed | Diusulkan, belum disetujui | Saat keputusan diajukan untuk ditinjau |
| Accepted | Disetujui dan berlaku | Setelah disepakati tim dan dewan peninjau arsitektur |
| Superseded | Digantikan oleh ADR lain | Ketika muncul keputusan baru yang menggantikan keputusan lama |

ADR yang telah digantikan tidak dihapus, melainkan diberi status Superseded agar jejak alasan keputusan lama tetap dapat ditelusuri. Praktik ini secara langsung mengatasi anti-pattern Groundhog Day dan Email-Driven Architecture.
