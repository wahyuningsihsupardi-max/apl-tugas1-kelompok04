Penanggung Jawab : Annisa Meyti Friska Manurung

# BAB I. Pendahuluan

## 1.1 Gambaran layanan inti
Layanan **Perpustakaan** merupakan salah satu sub-layanan dari **Student Learning Platform (II-4)**. Student Learning Platform adalah sebuah sistem informasi berbasis web yang digunakan oleh UMI untuk mendukung kegiatan belajar-mengajar, manajemen perpustakaan, dan interaksi akademik antar mahasiswa, dosen, dan staf perpustakaan. Layanan perpustakaan dalam tugas ini dibatasi pada empat fungsi inti, yaitu: **pencarian katalog**, **peminjaman**, **perpanjangan**, dan **akses koleksi digital**.

Melalui layanan ini, mahasiswa dan dosen dapat mencari judul koleksi secara cepat, melakukan peminjaman dan perpanjangan pinjaman, serta mengakses koleksi digital (jurnal, e-book, dan media digital) sesuai dengan hak akses yang dimiliki. Pengembangan layanan ini didasarkan pada keterbatasan sistem katalog yang masih berbasis manual atau terpisah dari sistem akademik utama (SIAKAD), sehingga menyebabkan informasi ketersediaan koleksi tidak sinkron, peminjaman yang rawan terduplikasi, serta akses koleksi digital yang belum terintegrasi dan sulit diakses dari luar kampus.

Arsitektur layanan ini dirancang menggunakan prinsip **microservices**, di mana setiap fungsi inti dipisahkan menjadi modul layanan tersendiri yang saling berkomunikasi melalui API. Pemisahan ini dilakukan agar setiap layanan dapat dikembangkan, diuji, dan diskalakan secara independen. Contohnya, *catalog service* menangani pencarian dan manajemen biblio-data, *circulation service* menangani peminjaman dan perpanjangan, dan *digital collection service* menangani akses koleksi digital. Arsitektur ini dipilih karena memenuhi kebutuhan **skalabilitas terpisah** (pencarian katalog bisa diskalakan mandiri) dan **berdiri sendiri** (masing-masing modul dapat dirilis tanpa memengaruhi layanan lainnya).

## 1.2 Ruang lingkup (3-5 kasus penggunaan)
Layanan perpustakaan dibatasi pada lima kasus penggunaan utama sebagai berikut:

1. **Pencarian katalog** — Pengguna (mahasiswa/dosen) mencari koleksi berdasarkan judul, penulis, ISBN, atau kata kunci. Hasil pencarian menampilkan daftar katalog beserta status ketersediaan (tersedia/dipinjam) dan tautan ke koleksi digital jika ada.
2. **Peminjaman koleksi fisik** — Pengguna meminjam koleksi fisik dari ruang baca perpustakaan. Sistem mencatat detail peminjaman, tanggal pengembalian, dan menghitung denda (fine) jika terlambat.
3. **Perpanjangan peminjaman** — Pengguna memperpanjang tenggat pengembalian koleksi yang belum dibatalkan (belum diambil lain). Sistem menghitung ulang tenggat dan memberitahu jadwal perpanjangan.
4. **Akses koleksi digital** — Pengguna mengakses berkas koleksi digital (jurnal, e-book, media digital) melalui tautan yang diberikan. Sistem mengendalikan akses berdasarkan lisensi penerbit dan memberikan batas waktu akses.
5. **Pencatatan transaksi** — Sistem mencatat seluruh transaksi (peminjaman, perpanjangan, pengembalian, akses koleksi digital) menjadi riwayat yang dapat ditelusuri oleh pengguna dan admin.
   
## 1.3 Sumber data
Layanan perpustakaan menggunakan beberapa sumber data yang bersumber dari sistem eksternal dan internal, yaitu:

1. **Data katalog koleksi fisik** — Database relasional (PostgreSQL) yang berisi data judul, penulis, penerbit, ISBN, dan informasi ketersediaan koleksi.
2. **Data transaksi peminjaman** — Database relasional (PostgreSQL) yang merekam setiap peminjaman, pengembalian, perpanjangan, dan denda yang terjadi.
3. **Data koleksi digital** — Object storage (AWS S3 / MinIO) yang menyimpan berkas digital jurnal, e-book, dan media digital, bersama metadata akses yang dikendalikan oleh *digital collection service*.
4. **Metadata eksternal (OAI-PMH)** — Data metadata koleksi dari katalog eksternal perpustakaan lain yang tersedia melalui protokol OAI-PMH (Open Archives Initiative Metadata Harvest).
5. **Referensi akademik (SIAKAD)** — Sumber informasi status keanggotaan dan status akademik mahasiswa melalui integrasi REST API dengan sistem SIAKAD. Data ini digunakan untuk validasi hak akses pengguna dan pencegahan aktivitas tidak sah.

