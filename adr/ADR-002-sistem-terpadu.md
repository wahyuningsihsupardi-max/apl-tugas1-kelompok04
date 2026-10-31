# ADR-002: Transaksi Terpusat Berbasis ACID dan Pessimistic Locking untuk Sirkulasi

* **Status:** Diterima
* **Tanggal:** 31-10-2026
* **Penulis:** Andi Tsabitha Hulwah

## Context
Pada sistem peminjaman buku perpustakaan saat ini, sering terjadi kendala *race condition* (peminjaman ganda) ketika beberapa pengguna mencoba meminjam atau memesan eksemplar buku fisik yang sama secara bersamaan. Hal ini memicu ketidakcocokan data antara status buku di sistem dengan ketersediaan fisik di rak perpustakaan, yang melanggar kebutuhan *Functional Correctness* (ASR-3).

## Decision
Kami memutuskan untuk menerapkan modul transaksi basis data terpusat yang memenuhi kriteria ACID (*Atomicity, Consistency, Isolation, Durability*) dan mengimplementasikan mekanisme *Pessimistic Locking* pada tabel data ketersediaan eksemplar buku selama proses transaksi peminjaman berlangsung.

## Consequences
* **Keuntungan:** Menjamin integritas data ketersediaan buku secara 100%, mencegah terjadinya *double borrowing* (peminjaman ganda pada satu eksemplar fisik), serta memastikan status peminjaman terekam secara tepat dan *real-time*.
* **Kerugian:** Sedikit meningkatkan waktu antrean transaksi saat terjadi peminjaman berulang pada eksemplar buku yang sangat populer di waktu yang sama.

## Compliance
Divalidasi melalui pengujian integrasi transaksi konkurensi (*concurrency integration testing*) dengan mensimulasikan dua akun yang mencoba meminjam eksemplar buku yang sama pada milidetik yang sama.
