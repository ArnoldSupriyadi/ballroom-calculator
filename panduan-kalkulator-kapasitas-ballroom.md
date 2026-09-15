# Kalkulator Kapasitas Ballroom

Dokumen dasar, alasan pembuatan, dan panduan penggunaan

| | |
|---|---|
| Aplikasi | `kalkulator-kapasitas-ballroom.html` (web app satu file) |
| Objek utama | Ballroom Lantai 3 (EL +10.500), luas 650 m² (25 × 26 m) |
| Sumber denah | Shop drawing no. 002 "Denah Lantai 3 (EL+10.500)", 29 September 2025, skala NTS |
| Status dokumen | Draf kerja, versi 1.0, 15 September 2026 |
| Status angka | Estimasi perencanaan. Belum diverifikasi pengelola gedung. |

---

## 1. Ringkasan

Web app ini dibuat untuk memprediksi berapa meja bulat, kursi, dan pax yang muat di sebuah ballroom, berdasarkan ukuran ruangan dan pilihan layout. Aplikasi menggambar denah layout secara langsung, menghitung kapasitas, memberi peringatan kalau layout tidak realistis, dan menghasilkan gambar PNG serta teks keterangan yang bisa dipakai untuk proposal.

Hasil acuan untuk ballroom 650 m² dengan panggung 10 × 4 m, tanpa buffet, satu hall:

- Meja Ø 180 cm, jarak standar: **28 meja × 10 kursi = ±280 pax**
- Meja Ø 180 cm, jarak nyaman: **20 meja × 10 kursi = ±200 pax**

Angka ini adalah batas geometri, bukan kapasitas resmi. Kapasitas yang boleh dipublikasikan tetap harus dikonfirmasi ke pengelola gedung.

---

## 2. Latar belakang

### 2.1 Masalah yang ingin diselesaikan

Pertanyaan "ballroom ini muat berapa pax?" selalu muncul saat menjual paket event, pernikahan, atau gala dinner. Jawabannya berubah tergantung banyak faktor: ukuran meja, jarak antar meja, jumlah kursi per meja, ada atau tidaknya panggung dan buffet, serta apakah ruangan dibagi dua dengan partisi.

Tanpa alat bantu, hitungan ini biasanya dilakukan manual dan berisiko:

1. **Tidak konsisten.** Dua orang bisa menghasilkan angka berbeda untuk ruangan yang sama.
2. **Terlalu optimistis.** Layout meja di gambar arsitek sering dikutip sebagai kapasitas, padahal itu hanya ilustrasi.
3. **Lambat saat klien bertanya "kalau begini bagaimana?"** Setiap perubahan (misalnya menambah buffet) harus dihitung dan digambar ulang.
4. **Tidak ada gambar yang bisa langsung dikirim ke klien.**

### 2.2 Temuan dari analisis denah

Analisis awal terhadap shop drawing Lantai 3 menghasilkan beberapa temuan yang menjadi dasar aplikasi:

- **Denah berskala NTS (tidak berskala).** Dimensi ruangan tidak bisa diukur akurat dari gambar, sehingga ukuran 25 × 26 m ditetapkan sebagai input dari pengguna, bukan hasil pengukuran gambar.
- **Layout arsitek menampilkan 30 meja × 8 kursi (±240 pax)** dengan pola zig-zag, tanpa panggung dan tanpa buffet. Arsitek menyisakan ruang kosong ±5–6 m di sisi utara dan selatan.
- **Ada garis partisi di tengah**, sehingga ballroom bisa dipakai sebagai satu hall atau dua ruang terpisah.
- **Bukaan pintu** terbaca di dinding selatan (2 pintu ganda ke foyer), dinding barat (2), dan dinding timur (2, belum pasti pintu).
- **Ada elemen menonjol** di tengah dinding barat dan timur, serta ceruk panel di tengah dinding selatan. Fungsinya belum diketahui.

Karena banyak variabel dan banyak ketidakpastian, pendekatan terbaik adalah alat simulasi yang parameternya bisa diubah, bukan satu angka tetap.

---

## 3. Tujuan dan batasan

### 3.1 Tujuan

1. Memberi estimasi kapasitas round table yang cepat, konsisten, dan bisa dijelaskan cara hitungnya.
2. Menampilkan denah layout yang mudah dibaca oleh tim sales, operasional, dan klien.
3. Membandingkan ukuran meja dan kepadatan dalam satu tampilan.
4. Menandai layout yang tidak realistis sebelum angkanya dipakai untuk jualan.
5. Bisa dipakai untuk venue lain dengan mengganti ukuran ruangan.

### 3.2 Bukan tujuan

- **Bukan penentu kapasitas legal.** Kapasitas resmi ditentukan oleh jalur evakuasi, jumlah pintu darurat, dan ketentuan proteksi kebakaran gedung.
- **Bukan alat CAD.** Aplikasi tidak membaca file DWG dan tidak mendeteksi tabrakan dengan kolom atau perabot tetap.
- **Bukan sistem booking.** Tidak ada penyimpanan data, akun pengguna, atau database.

---

## 4. Sumber data dan status verifikasi

| Data | Nilai yang dipakai | Asal | Status |
|---|---|---|---|
| Dimensi ballroom | 25 × 26 m | Input pengguna | Perlu ukur di lokasi |
| Luas | 650 m² | Hasil 25 × 26 | Konsisten dengan dimensi |
| Partisi | Di tengah lebar ruangan | Denah arsitek | Terbaca jelas. Jenis rel (lantai atau plafon) belum diketahui |
| Pintu selatan P1, P2 | 2 pintu ganda, ±2,4 m | Denah arsitek, diskalakan | Terbaca jelas, posisi estimasi |
| Pintu barat P3, P4 | 2 pintu, ±2,0 m | Denah arsitek, diskalakan | Perlu konfirmasi |
| Bukaan timur | 2 bukaan, ±2,0 m | Denah arsitek | Belum pasti pintu |
| Kotak di dinding samping | Menonjol ±1,6 m, panjang ±2 m | Denah arsitek | Fungsi belum diketahui |
| Ceruk dinding selatan | ±4,4 × 1,2 m | Denah arsitek | Fungsi belum diketahui |
| Ruang utara | Kedalaman ±3,9 m | Angka tertulis di denah | Fungsi belum diketahui |
| Kolom struktur | Di 4 sudut dan tengah dinding selatan | Denah arsitek | Klaim "bebas kolom" perlu cek lokasi |

---

## 5. Metodologi perhitungan

### 5.1 Zona meja

Zona tempat meja boleh diletakkan dihitung dari ukuran ruangan dikurangi area yang tidak boleh diisi meja:

- **Jalur keliling** dari dinding. Default 1,5 m di semua sisi.
- **Zona panggung** di sisi depan = jalur keliling + kedalaman panggung + ruang kosong depan panggung. Default 1,5 + 4 + 2 = 7,5 m.
- **Zona buffet** (kalau dipakai) di dinding belakang, kiri, kanan, atau kiri dan kanan. Default kedalaman 3,5 m, sudah termasuk ruang antrean.
- **Jarak bebas rel partisi** saat ruangan dibagi 2. Default 1,5 m di kiri dan kanan rel.

Dengan default di atas, zona meja ballroom 25 × 26 m adalah:

| Mode | Lebar zona | Kedalaman (dengan panggung) | Kedalaman (tanpa panggung) |
|---|---|---|---|
| Satu hall | 22 m | 17 m | 23 m |
| Dibagi 2 | 2 × 9,5 m | 17 m | 23 m |

### 5.2 Jarak antar meja (modul)

```
modul (jarak as ke as) = diameter meja + jarak tepi ke tepi
```

| Pilihan | Jarak tepi ke tepi | Alasan |
|---|---|---|
| Standar | 1,8 m | Kursi ±0,6 m di tiap meja ditambah jalur sempit ±0,6 m |
| Nyaman | 2,2 m | Jalur antar sandaran ±1,0 m, lebih layak untuk waiter |
| Atur sendiri | Bebas | Untuk kebutuhan khusus |

Jalur bersih antar sandaran kursi dihitung sebagai:

```
jalur antar sandaran = jarak tepi ke tepi − 2 × 0,6 m
```

### 5.3 Kursi per meja

```
kursi = keliling meja ÷ lebar per kursi, dibulatkan ke angka genap terdekat
keliling meja = π × diameter
```

Pembulatan ke angka genap dipakai karena meja banquet hampir selalu diisi jumlah kursi genap agar simetris.

| Diameter | Keliling | Nyaman (60 cm/kursi) | Rapat (50 cm/kursi) |
|---|---|---|---|
| 120 cm | 377 cm | 6 | 8 |
| 150 cm | 471 cm | 8 | 10 |
| 160 cm | 503 cm | 8 | 10 |
| 180 cm | 565 cm | 10 | 12 |

Jumlah kursi bisa diisi manual. Kalau melebihi hasil hitungan keliling, aplikasi memberi peringatan.

### 5.4 Pola susunan

Aplikasi menghitung dua pola dan bisa memilih otomatis yang menampung meja paling banyak:

- **Grid lurus.** Jumlah kolom = lebar zona ÷ modul, jumlah baris = kedalaman zona ÷ modul, keduanya dibulatkan ke bawah.
- **Zig-zag (heksagonal).** Baris selang-seling bergeser setengah modul. Jarak antar baris = modul × 0,866, sehingga jarak ke meja diagonal tetap sama dengan modul. Pola ini sama dengan yang dipakai arsitek di denah.

Pola zig-zag tidak selalu lebih banyak. Contohnya meja 180 cm jarak nyaman di mode dibagi 2: grid lurus menampung 16 meja, zig-zag hanya 12. Karena itu opsi otomatis dijadikan default.

### 5.5 Buffet dan pintu

Meja buffet digambar di dalam zona buffet, lalu **dipotong di depan setiap pintu** dengan jarak bebas lebar daun pintu ditambah 0,75 m di kiri dan kanan. Buffet juga menghindari kotak di dinding samping dan ceruk di dinding selatan. Segmen yang lebih pendek dari 1,5 m dibuang.

Aturan ini hanya berlaku untuk preset Lantai 3. Untuk ruangan custom, aplikasi tidak tahu posisi pintu dan menampilkan peringatan.

### 5.6 Kapasitas

```
total pax = jumlah meja × kursi per meja
luas per pax = luas ruangan ÷ total pax  (bruto)
```

---

## 6. Hasil acuan untuk Ballroom 650 m²

Semua tabel memakai default: jalur keliling 1,5 m, panggung 10 × 4 m dengan ruang depan 2 m, pola otomatis, kursi nyaman 60 cm. Format sel: **meja / pax**.

### 6.1 Satu hall

| Diameter | Kursi | Dengan panggung, standar | Dengan panggung, nyaman | Tanpa panggung, standar | Tanpa panggung, nyaman |
|---|---|---|---|---|---|
| 120 cm | 6 | 39 / 234 | 30 / 180 | 52 / 312 | 39 / 234 |
| 150 cm | 8 | 30 / 240 | 25 / 200 | 42 / 336 | 35 / 280 |
| 160 cm | 8 | 30 / 240 | 25 / 200 | 39 / 312 | 30 / 240 |
| 180 cm | 10 | 28 / 280 | 20 / 200 | 39 / 390 | 30 / 300 |

### 6.2 Dibagi 2 (total kedua sisi)

| Diameter | Kursi | Dengan panggung, standar | Dengan panggung, nyaman | Tanpa panggung, standar | Tanpa panggung, nyaman |
|---|---|---|---|---|---|
| 120 cm | 6 | 30 / 180 | 20 / 120 | 42 / 252 | 28 / 168 |
| 150 cm | 8 | 20 / 160 | 20 / 160 | 28 / 224 | 28 / 224 |
| 160 cm | 8 | 20 / 160 | 20 / 160 | 28 / 224 | 24 / 192 |
| 180 cm | 10 | 20 / 200 | 16 / 160 | 28 / 280 | 20 / 200 |

### 6.3 Satu hall dengan panggung dan buffet

| Diameter | Kursi | Buffet belakang, standar | Buffet belakang, nyaman | Buffet kiri-kanan, standar | Buffet kiri-kanan, nyaman |
|---|---|---|---|---|---|
| 120 cm | 6 | 33 / 198 | 22 / 132 | 27 / 162 | 20 / 120 |
| 150 cm | 8 | 24 / 192 | 20 / 160 | 20 / 160 | 18 / 144 |
| 160 cm | 8 | 22 / 176 | 15 / 120 | 20 / 160 | 15 / 120 |
| 180 cm | 10 | 22 / 220 | 15 / 150 | 18 / 180 | 12 / 120 |

Panjang meja buffet yang tersisa setelah dipotong pintu: ±8 m untuk posisi belakang dan ±19 m untuk kiri-kanan (meja 180 cm, jarak standar).

### 6.4 Cara membaca tabel

1. **Mode dibagi 2 kehilangan kapasitas cukup besar**, karena jalur 1,5 m di kiri dan kanan rel partisi memakan ±78 m².
2. **Meja 180 cm jarak standar paling efisien** untuk acara buffet. Untuk plated service, gunakan kolom jarak nyaman.
3. **Buffet di dinding belakang terlihat lebih hemat kapasitas**, tetapi panjang mejanya hanya ±8 m. Untuk acara 200 pax ke atas, panjang itu kemungkinan kurang dan antrean akan menumpuk. Kebutuhan panjang buffet per pax perlu dikonfirmasi ke tim katering.
4. **Kolom "tanpa panggung" adalah batas atas.** Layout arsitek sendiri hanya 30 meja × 8 kursi karena menyisakan ruang di depan dan belakang.
5. **Luas per pax di skenario default ±2,3 m²/pax.** Acuan umum industri untuk round table banquet sering disebut ±1,2–1,5 m²/pax [medium confidence, bervariasi antar operator dan biasanya belum memperhitungkan panggung atau jalur partisi]. Angka aplikasi ini tergolong konservatif.

---

## 7. Fitur aplikasi

| Kelompok | Fitur |
|---|---|
| Ruangan | Nama, lebar, panjang, jalur keliling, preset Lantai 3 (pintu, kolom, ruang utara, kotak dinding, ceruk) |
| Pembagian ruang | Satu hall atau dibagi 2, jarak bebas rel partisi |
| Panggung | Aktif atau tidak, lebar, kedalaman, ruang kosong depan panggung |
| Buffet | Tanpa, belakang, kiri, kanan, kiri dan kanan, kedalaman zona. Otomatis dipotong di depan pintu |
| Meja dan kursi | Diameter preset 120/150/160/180 atau bebas, jarak standar/nyaman/bebas, lebar per kursi, kursi manual, pola otomatis/zig-zag/grid |
| Tampilan denah | Nomor meja, ikon kursi yang menghadap meja, dimensi, label sisi A/B, jalur pintu |
| Ringkasan | Total pax, jumlah meja, kursi per meja, m² per pax, jalur antar sandaran |
| Peringatan | Jalur sempit, kepadatan tinggi, kursi berlebih, panggung terlalu lebar, panggung melintasi partisi, buffet tidak muat, pintu ruangan custom tidak diketahui |
| Perbandingan | Tabel 4 diameter × jarak standar/nyaman sesuai pengaturan aktif |
| Keluaran | Title block di bawah denah, unduh PNG, cetak, teks keterangan untuk brosur, tautan skenario |

---

## 8. Cara menggunakan

1. **Buka file** `kalkulator-kapasitas-ballroom.html` di browser, atau buka versi yang sudah di-deploy.
2. **Atur ruangan.** Isi nama, lebar, dan panjang. Matikan preset Lantai 3 kalau menghitung venue lain.
3. **Pilih mode ruang.** Satu hall atau dibagi 2.
4. **Atur panggung dan buffet** sesuai jenis acara.
5. **Pilih meja.** Diameter, jarak antar meja, dan kepadatan kursi.
6. **Baca ringkasan dan peringatan.** Jangan pakai angka yang memunculkan peringatan merah.
7. **Bandingkan** di tabel perbandingan ukuran meja.
8. **Simpan atau kirim.** Unduh PNG untuk klien, salin teks keterangan untuk proposal, atau salin tautan skenario untuk tim.

### Tips skenario per jenis acara

| Jenis acara | Pengaturan yang disarankan |
|---|---|
| Pernikahan dengan prasmanan | Panggung aktif, buffet kiri-kanan atau belakang, meja 180 cm jarak standar |
| Gala dinner plated | Panggung aktif, tanpa buffet, jarak nyaman |
| Seminar makan siang | Panggung aktif, buffet belakang, meja 150–160 cm |
| Dua acara bersamaan | Mode dibagi 2, cek kapasitas per sisi |

---

## 9. Aturan penggunaan angka

1. **Angka di aplikasi adalah estimasi perencanaan.** Selalu tulis "hingga ±" atau "estimasi" di materi internal.
2. **Jangan publikasikan kapasitas** sebelum dikonfirmasi pengelola gedung dan dicek jalur evakuasinya.
3. **Gunakan skenario dengan panggung sebagai angka jual**, bukan skenario tanpa panggung.
4. **Jangan pakai kepadatan rapat sebagai angka jual.** Kepadatan rapat membuat tamu berdesakan dan menyulitkan pelayanan.
5. **Contoh keterangan yang aman** setelah diverifikasi:

> Ballroom 650 m² (25 × 26 m). Kapasitas round table hingga ±280 pax. Dapat dibagi menjadi 2 ruang @ 325 m².

---

## 10. Keterbatasan dan risiko

| Keterbatasan | Dampak | Mitigasi |
|---|---|---|
| Denah sumber berskala NTS | Posisi pintu, kolom, dan elemen dinding bisa meleset | Ukur di lokasi, perbarui preset |
| Tidak ada deteksi tabrakan dengan kolom dan kotak dinding | Meja bisa tampak menempel ke elemen tetap | Periksa gambar, tambah jalur keliling bila perlu |
| Posisi pintu hanya dikenal untuk preset Lantai 3 | Buffet di ruangan custom bisa menutup pintu | Tambahkan input pintu manual di versi berikutnya |
| Jenis rel partisi belum diketahui | Kapasitas mode dibagi 2 bisa lebih rendah atau lebih tinggi | Konfirmasi ke pengelola gedung |
| Kapasitas evakuasi tidak dihitung | Angka bisa melebihi batas legal | Wajib cek ke pengelola gedung atau konsultan |
| Tidak ada penyimpanan data | Skenario hanya tersimpan lewat tautan | Versi lanjutan dengan database |
| Ekspor PNG memakai font Arial | Tampilan PNG sedikit berbeda dari browser | Tidak berdampak pada angka |

---

## 11. Riwayat keputusan dan perubahan

| Urutan | Keputusan atau perubahan | Alasan |
|---|---|---|
| 1 | Istilah diluruskan menjadi denah lantai, bukan site plan | Site plan mencakup kavling dan akses, file sumber hanya satu lantai |
| 2 | Hitungan awal memakai asumsi 28 × 27 m dari jarak grid | Belum ada ukuran dinding |
| 3 | Posisi pintu dikoreksi dari 3 menjadi 4+ bukaan | Koreksi dari pengguna dan pembacaan ulang denah detail |
| 4 | Jumlah meja arsitek dikoreksi menjadi 30 meja × 8 kursi | Denah detail lebih jelas dari gambar awal |
| 5 | Dimensi ditetapkan 25 × 26 m = 650 m² | Input pengguna |
| 6 | Opsi satu hall atau dibagi 2 ditambahkan | Pengaruh terbesar terhadap kapasitas |
| 7 | Pola otomatis dijadikan default | Zig-zag tidak selalu menghasilkan meja terbanyak |
| 8 | Perbaikan bug: buffet menutup pintu | Buffet sekarang dipotong di depan pintu dan menghindari kotak dinding serta ceruk |
| 9 | Ikon kursi mengganti titik lingkaran | Arah duduk lebih mudah dibaca, ukuran mengikuti skala |

---

## 12. Rencana pengembangan

1. **Input pintu dan halangan manual** untuk venue selain Lantai 3.
2. **Deteksi tabrakan** antara meja dan kolom, kotak dinding, atau jalur pintu.
3. **Layout lain** seperti theater, classroom, cocktail, dan U-shape.
4. **Perhitungan panjang buffet** berdasarkan jumlah pax dan standar katering.
5. **Penyimpanan skenario** per venue dan per klien, misalnya dengan Next.js dan Supabase.
6. **Ekspor PDF** dengan kop perusahaan.
7. **Pemindahan meja manual** dengan drag and drop.

---

## 13. Daftar verifikasi lapangan

Isi daftar ini sebelum angka kapasitas dipakai untuk materi promosi.

- [ ] Ukuran dinding bersih ballroom (lebar × panjang)
- [ ] Posisi dan lebar setiap pintu, termasuk pintu darurat
- [ ] Status bukaan di dinding timur
- [ ] Fungsi dan ukuran kotak di tengah dinding barat dan timur
- [ ] Fungsi ceruk di tengah dinding selatan
- [ ] Fungsi ruang utara dan jalur servis ke pantry
- [ ] Jenis rel partisi (lantai atau plafon) dan lebar bebas yang dibutuhkan
- [ ] Posisi kolom struktur di dalam ruangan (konfirmasi bebas kolom)
- [ ] Batas kapasitas resmi dari pengelola gedung
- [ ] Ukuran meja dan model kursi yang tersedia di inventaris
- [ ] Standar panjang buffet per pax dari tim katering

---

## Lampiran: istilah

| Istilah | Arti |
|---|---|
| NTS | Not to scale, gambar tidak berskala |
| Shop drawing | Gambar kerja detail untuk pelaksanaan |
| Round table | Meja bulat untuk acara banquet |
| Modul | Jarak dari pusat satu meja ke pusat meja berikutnya |
| Jarak tepi ke tepi | Jarak dari tepi satu meja ke tepi meja sebelahnya |
| Jalur keliling | Area kosong di sepanjang dinding |
| Plated service | Makanan disajikan waiter per piring ke meja |
| Buffet / prasmanan | Tamu mengambil makanan sendiri di meja saji |
| Pax | Jumlah orang atau tamu |
| Luas per pax bruto | Luas total ruangan dibagi jumlah pax |
