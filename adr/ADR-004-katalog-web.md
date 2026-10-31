# ADR-004: Penerapan Desain Antarmuka Progressive Disclosure pada Dashboard Pencarian

* **Status:** Diterima
* **Tanggal:** 31-10-2026
* **Penulis:** Andi Tsabitha Hulwah

## Context
Antarmuka pencarian dan katalog perpustakaan sebelumnya menyajikan terlalu banyak informasi detail (seperti metadata MARC, histori eksemplar, dan nomor registrasi internal) secara sekaligus di halaman utama. Hal ini membingungkan pengguna umum (mahasiswa) dan meningkatkan waktu pencarian buku di atas 2 menit, yang melanggar atribut kualitas kemudahan penggunaan (ASR-2 dan target Usability 90%).

## Decision
Kami memutuskan untuk menerapkan pola desain *Progressive Disclosure* pada antarmuka *Katalog Web*, di mana halaman utama hanya menampilkan informasi esensial (Judul, Penulis, Cover, Status Ketersediaan, dan Lokasi Rak Ringkas), sementara detail bibliografi dan metadata teknis lengkap hanya ditampilkan saat pengguna mengeklik atau memilih opsi "Lihat Detail".

## Consequences
* **Keuntungan:** Meningkatkan *learnability* dan tingkat keberhasilan pencarian mahasiswa secara mandiri (mencapai target < 2 menit atau < 3 kali klik), menyederhanakan alur navigasi, serta mengurangi beban muat (*rendering*) antarmuka awal.
* **Kerugian:** Membutuhkan satu langkah/klik tambahan bagi pustakawan atau pengelola data yang memerlukan informasi metadata teknis lengkap secara cepat.

## Compliance
Divalidasi melalui *Task-Based Usability Testing* pada 10 mahasiswa dari berbagai fakultas untuk mengukur tingkat keberhasilan pencarian dan jumlah klik pengguna.
