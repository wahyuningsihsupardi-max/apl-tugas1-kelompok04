# A3. Batasan Sistem
**Penanggung Jawab :** Wahyuningsih S 
## Batasan Sistem

| Kategori | Batasan | Dampak pada Arsitektur |
|---|---|---|
| **Teknologi** | Server dan jaringan sering terganggu, sehingga layanan digital dan pencarian katalog terhambat. | Perlu rancangan yang tahan gangguan: cache untuk katalog, penanganan saat layanan tidak tersedia (graceful degradation), serta pemantauan ketersediaan. |
| **Teknologi** | Sistem yang sudah ada (OPAC, MyLibrary, Moco PUBA UMI) harus tetap dipakai, dan informasi lokasi rak serta status peminjaman di pencarian belum lengkap. | Platform baru berperan sebagai lapisan integrasi, bukan pengganti total, dengan adapter ke sistem lama dan sinkronisasi status ketersediaan. |
| **Teknologi** | Koleksi fisik dan digital tidak selalu sejalan, karena ada buku cetak yang tidak punya e-book. | Model data koleksi memisahkan format fisik dan digital, dan antarmuka menampilkan ketersediaan masing-masing. |
| **Bisnis** | Aturan peminjaman PUBA bersifat tetap: maksimal 5 buku, masa pinjam 1 minggu, denda Rp1.000 per hari, perpanjangan maksimal 1 kali. | Aturan bisnis dipisah dari kode dalam modul kebijakan yang bisa dikonfigurasi, agar perubahan aturan tidak mengubah sistem inti. |
| **Bisnis** | Kebijakan FIKOM berbeda dari PUBA: buku hanya dibaca di tempat, belum ada peminjaman bawa pulang dan belum ada denda. | Arsitektur mendukung banyak unit perpustakaan dengan kebijakan masing-masing (multi-branch), tanpa menyamakan aturan keduanya. |
| **Bisnis** | Prioritas layanan antar stakeholder berbeda: kemudahan penggunaan , ketersediaan layanan, dan informasi stok yang akurat. | Perlu analisis trade-off atribut kualitas dan pemilihan taktik arsitektur yang menyeimbangkan usability, availability, dan akurasi data. |
| **Operasional** | Peminjaman buku fisik masih dilayani langsung oleh petugas, termasuk pemberian stempel. | Sistem tidak boleh mengasumsikan peminjaman mandiri penuh, dan perlu antarmuka petugas yang mencatat transaksi dan tersinkron dengan data. |
| **Operasional** | Katalog FIKOM belum lengkap karena pendataan koleksi belum selesai. | Data katalog harus dimigrasikan bertahap, dan arsitektur harus toleran terhadap data yang belum lengkap serta menunjuk ke PUBA bila perlu. |
| **Operasional** | Fasilitas pendukung layanan di perpustakaan fakultas masih terbatas. | Sistem dibuat ringan dan berbasis web yang bisa dipakai dari perangkat umum, tanpa bergantung pada perangkat khusus di lokasi. |


