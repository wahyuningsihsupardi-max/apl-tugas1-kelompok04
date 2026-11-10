# U2. Skenario Atribut Kualitas

**BAB II: Skenario**  
**Penanggung jawab:** [isi sesuai pembagian MID]

**Ketentuan dosen:** 3 skenario (1 per karakteristik pendorong), **setiap skenario memiliki 6 komponen**, ukuran respons harus berupa **angka**, dan minimal satu skenario menjawab masalah utama.

## 2.1 Skenario SK-01 — Availability
**Tujuan:** layanan katalog tersedia saat mahasiswa/petugas membutuhkannya.

| Enam komponen skenario | Isi yang harus dilengkapi |
|---|---|
| 1. Sumber stimulus | [siapa/apa yang memicu, misalnya mahasiswa melakukan akses] |
| 2. Stimulus | [kejadian yang memicu respons] |
| 3. Lingkungan (environment) | [kondisi sistem saat stimulus terjadi] |
| 4. Artefak (artifact) | [bagian sistem yang menerima stimulus] |
| 5. Respons (response) | [apa yang dilakukan sistem] |
| 6. Ukuran respons (response measure) | **Target usulan B2: availability >=99%** selama 7 hari pemantauan tiap menit; [sesuaikan agar cocok dengan skenario] |

**Penjelasan kaitan ASR dan taktik:** [isi].

## 2.2 Skenario SK-02 — Functional Correctness
**Tujuan:** informasi status buku pada katalog sesuai catatan koleksi/peminjaman. **Skenario ini diusulkan sebagai jawaban langsung atas masalah utama.**

| Enam komponen skenario | Isi yang harus dilengkapi |
|---|---|
| 1. Sumber stimulus | [misalnya petugas menyimpan transaksi peminjaman] |
| 2. Stimulus | [kejadian/pembaruan status koleksi] |
| 3. Lingkungan (environment) | [sistem beroperasi normal atau kondisi tertentu] |
| 4. Artefak (artifact) | [Layanan Peminjaman / Layanan Katalog / Basis Data] |
| 5. Respons (response) | [status ketersediaan buku ditampilkan sesuai catatan acuan] |
| 6. Ukuran respons (response measure) | **Target usulan B2: >=95 dari 100 sampel data buku cocok** dengan data acuan; [tambahkan toleransi keterlambatan pembaruan bila rancangan memakai event] |

**Bukti pendukung masalah utama:** [cantumkan R# yang benar dan isi ringkasnya].

## 2.3 Skenario SK-03 — Usability
**Tujuan:** mahasiswa mudah mencari informasi koleksi tanpa bantuan.

| Enam komponen skenario | Isi yang harus dilengkapi |
|---|---|
| 1. Sumber stimulus | [misalnya mahasiswa baru mengakses katalog] |
| 2. Stimulus | [tugas mencari judul/status/lokasi rak] |
| 3. Lingkungan (environment) | [prototipe, pengujian pengguna] |
| 4. Artefak (artifact) | [Portal Katalog] |
| 5. Respons (response) | [pengguna menemukan informasi yang diminta] |
| 6. Ukuran respons (response measure) | **Target usulan B2: >=9 dari 10 mahasiswa berhasil <=2 menit tanpa bantuan** |

## 2.4 Matriks keterkaitan
| Skenario | Pendorong B1 | ASR U1 | Taktik U3 | Penilaian U6 |
|---|---|---|---|---|
| SK-01 | Availability | [ASR-..] | [T-01] | [isi] |
| SK-02 | Functional Correctness | [ASR-..] | [T-02] | [isi] |
| SK-03 | Usability | [ASR-..] | [T-03] | [isi] |

**Catatan:** angka 99%, 95%, dan 90% berasal dari **target usulan B2 sebelumnya**, bukan hasil pengukuran sistem UMI.
