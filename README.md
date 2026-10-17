# physiverse — *Fisika di Sekitar Kita*

> **Understanding the Science Behind Everyday Life.**

Website sains populer statis (HTML + CSS + JavaScript murni, tanpa *framework*) untuk tugas UTS
**Data Raya dan Pemrograman** — Universitas Negeri Jakarta.
Berisi **5 artikel populer** (600–900 kata per artikel), **5 simulasi fisika interaktif**, dan
**6 halaman jelajah**: Science History, Physics News, Physiverse Data Lab, Glosarium Fisika,
Fisika 60 Detik, serta Nobel Fisika.

Beranda dirancang bergaya majalah dengan **hero video layar penuh** (`assets/hero-physiverse.mp4`,
dirender sendiri oleh kelompok) dan gambar *poster* sebagai cadangan bila video gagal dimuat.

---

## 1. Struktur berkas

```
Default Project/
├─ index.html             Beranda: hero video, tagline, sorotan 5 artikel, penyaring, pintasan fitur
├─ touchscreen.html       Artikel 1 — Layar sentuh kapasitif      + Simulator layar sentuh
├─ pelangi.html           Artikel 2 — Optika pelangi              + Simulator jalur cahaya & busur 360°
├─ anc.html               Artikel 3 — Active Noise Cancellation   + Simulator interferensi gelombang
├─ asap-kopi.html         Artikel 4 — Penguapan & kondensasi      + Simulator kabut minuman panas
├─ air-fryer.html         Artikel 5 — Konveksi paksa & Maillard   + Simulator air fryer
├─ science-history.html   Jelajah — garis waktu fisika + "Physicists Who Changed the World"
├─ physics-news.html      Jelajah — 8 kabar riset 2026 (dengan tautan sumber asli)
├─ data-lab.html          Jelajah — Physiverse Data Lab (dasbor cuaca interaktif)
├─ glosarium.html         Jelajah — ± 100 istilah fisika (cari + saring per cabang)
├─ detik-60.html          Jelajah — Fisika 60 Detik (10 kartu infografis)
├─ nobel.html             Jelajah — peraih Nobel Fisika 1901–2026 + penjelasan
├─ tentang.html           Tentang Kami: profil, peran, kontribusi, fitur, pernyataan penggunaan AI
├─ sumber.html            Sumber & Referensi: seluruh rujukan + catatan lisensi aset
├─ log-ai.html            Log Penggunaan AI (20 entri: tujuan, prompt, revisi/verifikasi)
├─ css/style.css          Seluruh desain (palet Coolors, tipografi, tata letak, widget, cetak)
├─ js/main.js             Navigasi + dropdown, progres baca, daftar isi, akordeon, polling, filter,
│                         hero video, dialog film, garis waktu, animasi muncul saat digulir
├─ js/widgets.js          Lima simulasi interaktif (SVG/DOM + Canvas 2D)
├─ js/lab.js              Data Lab: ambil data Open-Meteo, grafik SVG, titik embun, skala Beaufort
├─ js/glossary.js         Glosarium: pencarian, penyaring cabang, penyorotan kata kunci
├─ js/nobel.js            Nobel: pencarian nama/topik + penyaring era
├─ tools/refresh_layout.py     Menyisipkan header & footer yang seragam ke semua halaman
├─ tools/make_hero_video.py    Pembuat video hero (Python + numpy/imageio)
├─ tools/fetch_fallback_data.py Mengambil snapshot data nyata Open-Meteo untuk mode offline
├─ tools/check_links.py        Memeriksa semua tautan lokal, aset, dan jangkar (#id)
├─ tools/count_words.py        Menghitung jumlah kata isi kelima artikel (target 600–900)
├─ tools/build_single_file.py  Membuat versi "satu-berkas" (semua aset ditanam ke dalam HTML)
├─ dist-single/               Hasil versi satu-berkas: 15 berkas .html, tanpa folder pendukung
├─ assets/favicon.svg     Ikon situs (karya sendiri)
├─ assets/hero-physiverse.mp4  Video latar beranda (dirender sendiri, ± 493 KB)
├─ assets/hero-poster.jpg      Gambar poster hero (cadangan bila video tidak dimuat)
├─ assets/data-fallback-jakarta.json  Snapshot data Open-Meteo (cadangan saat offline)
├─ assets/data-fallback-jakarta.js    Salinan snapshot yang sama, dimuat lewat &lt;script&gt;
│                                     agar Data Lab tetap tampil di mode file://
├─ robots.txt             Aturan perayapan
├─ sitemap.xml            Peta situs untuk mesin pencari
└─ README.md              Berkas ini
```

Semua halaman berada di **akar folder** dan memakai jalur relatif sederhana (`css/style.css`, `js/main.js`),
sehingga dapat diunggah ke hosting statis tanpa konfigurasi tambahan.

---

## 2. Menjalankan di komputer

Cara paling cepat: klik dua kali `index.html`.

Karena hampir seluruh interaksi berjalan di sisi klien (tanpa server, tanpa basis data), sebagian besar
halaman tetap berfungsi lewat protokol `file://`. **Physiverse Data Lab** juga tetap menampilkan grafik
saat dibuka lewat `file://`: karena browser memblokir `fetch()` pada protokol itu, halaman otomatis
memakai snapshot cadangan yang dimuat lewat `<script src="assets/data-fallback-jakarta.js">`, dan status
di halaman akan menyebutkan bahwa yang tampil adalah **data cadangan** (bukan data langsung).

Agar bisa mencoba **data langsung** (memilih 16 kota), jalankan server statis sederhana:

```powershell
# Python 3
python -m http.server 8123
# lalu buka http://127.0.0.1:8123/ di peramban
```

Bila perangkat tidak terhubung internet atau permintaan API gagal, Data Lab otomatis memakai
`assets/data-fallback-jakarta.json` — snapshot data nyata Open-Meteo yang tersimpan bersama situs — dan
memberi tahu pembaca bahwa yang ditampilkan adalah data cadangan.

Catatan: polling pembaca menyimpan pilihan di `localStorage` peramban, jadi pada mode
`file://` pun tetap tercatat (hanya untuk perangkat itu sendiri).

---

## 3. Publikasi online

Pilih salah satu, semuanya gratis untuk proyek statis:

| Layanan | Langkah singkat |
|---|---|
| **GitHub Pages** | Buat repositori → unggah semua berkas → *Settings → Pages → Deploy from branch* (`main` / root) |
| **Netlify** | *Add new site → Deploy manually* → seret folder ini ke halaman Netlify Drop |
| **Vercel** | *Add New Project → Import* repositori → *Framework preset: Other* → Deploy |

**Setelah dipublikasikan, ganti 4 hal berikut** agar SEO dan peta situs konsisten:

1. `canonical` dan `og:url` di setiap halaman (saat ini: `https://physiverse.netlify.app/...`).
2. Alamat `Sitemap:` pada `robots.txt`.
3. Semua `<loc>` pada `sitemap.xml`.
4. URL di `script type="application/ld+json"` (beranda & setiap artikel).

**Nama berkas pengumpulan** mengikuti format tugas: `UTS_DRP_[Kelas]_Kel[No]_[NamaWebsite]`,
misalnya `UTS_DRP_Fisika_Kel3_physiverse`.

---

## 3b. Kalau platform hanya menerima berkas `.html`

Ada platform (termasuk unggah-lewat-web GitHub, Netlify Drop, dan Vercel) yang **tidak menerima
berkas pendukung** seperti `css/`, `js/`, atau `assets/` — kadang karena folder tidak bisa
dipilih/di-*drag*, sehingga hanya berkas `.html` yang ikut terunggah. Akibatnya situs tampil
tanpa gaya dan tanpa interaksi.

Solusinya: **versi satu-berkas**. Seluruh CSS, JavaScript, ikon, gambar poster, dan video ditanam
langsung ke dalam setiap halaman, sehingga tidak ada folder pendukung yang perlu diunggah.

```powershell
python tools/build_single_file.py     # menghasilkan folder dist-single/
```

Hasilnya **15 berkas `.html`** (total ± 3 MB; `index.html` ± 1,5 MB karena video ditanam di dalamnya).
Cara mengunggahnya:

| Layanan | Langkah |
|---|---|
| **Netlify Drop** | Buka <https://app.netlify.com/drop> → seret folder `dist-single` |
| **GitHub Pages** | Repositori baru → **Add file → Upload files** → pilih **semua 15 berkas `.html`** (tanpa folder) → Commit → Settings → Pages → `main` / root |
| **Vercel** | Import repositori GitHub di atas (Framework Preset: **Other**, build/output dikosongkan) |

Catatan penting: setelah versi satu-berkas dibuat, **jangan lupa perbarui domain** pada bagian 3
tetap dilakukan pada berkas di akar folder (versi normal), lalu jalankan `build_single_file.py`
sekali lagi agar versi satu-berkas ikut memakai domain yang benar.

---

## 4. Halaman dan fiturnya

**Navigasi** identik di semua halaman: **Beranda · Artikel (dropdown 5 artikel) · Jelajah (dropdown 6 halaman) ·
Tentang (dropdown profil/sumber/log AI)**, plus tombol pintasan *Data Lab*. Menu menandai halaman aktif
otomatis, dapat dibuka-tutup dengan papan ketik, dan berubah menjadi panel di layar sempit (ditutup dengan `Esc`).

**Beranda** — hero **video** layar penuh (dengan tombol jeda dan tombol "Tonton film"), tagline,
bagian sorotan, strip fakta, kartu enam fitur jelajah, kartu sorotan 5 artikel dengan ilustrasi SVG,
penyaring kategori (Semua / Gadget & Teknologi / Fenomena Alam / Dapur & Kalor), ringkasan tema,
serta pintasan ke halaman profil dan sumber.

**Setiap artikel** memuat:

* hero dengan remah roti (*breadcrumb*), nama penulis + peran, tanggal, estimasi waktu baca otomatis;
* ilustrasi diagram SVG buatan sendiri beserta keterangan gambar;
* **polling pembaca** (pilihan tersimpan di perangkat, menampilkan batang persentase);
* penjelasan bertahap dengan subjudul bernomor, kotak rumus, kotak catatan/tips, kotak fakta;
* **satu simulasi interaktif** yang sesuai topik;
* **akordeon FAQ** (dapat dibuka-tutup, bisa diakses papan ketik);
* **daftar sumber** bernomor dengan penanda sitasi `[1]`–`[4]` di dalam teks;
* kotak **catatan verifikasi** yang menjelaskan mana angka yang sesungguhnya dan mana yang ilustratif;
* navigasi "artikel sebelumnya / berikutnya".

**Science History** — garis waktu penemuan besar (240 SM – 2026) yang titiknya menyala saat digulir,
bagian *Physicists Who Changed the World* berisi delapan kisah momen penemuan (Galileo, Newton, Faraday,
Maxwell, Marie Curie, Einstein, Emmy Noether, Feynman) dengan monogram SVG, serta penjelasan tiga pola
yang berulang dalam sejarah sains.

**Physics News** — delapan kabar riset pilihan Januari–Oktober 2026 (Nobel neutrino, jam nuklir,
rekor pulsa sinar-X, Long Shutdown 3 LHC, switchback Matahari, qubit silikon, galaksi tanpa materi gelap,
"sup" alam semesta awal). Tiap berita bertanggal dan bertaut ke halaman penerbit aslinya, dilengkapi
penyaring bidang dan panduan membaca berita sains.

**Physiverse Data Lab** — dasbor cuaca interaktif: pilih kota dan rentang hari, lihat metrik ringkas
(suhu, hujan, kelembapan, angin), grafik SVG buatan sendiri, tabel harian, perhitungan titik embun,
kelas angin Beaufort, ekspor CSV, penjelasan fisika di balik tiap besaran, tabel Beaufort, dan FAQ.
Data asli dari API Open-Meteo; tersedia jalur cadangan offline.

**Glosarium Fisika** — sekitar 100 istilah dari tujuh cabang fisika, dapat dicari (dengan tautan
pencarian seperti `glosarium.html?q=entropi`) dan disaring per cabang, plus penyorotan kata kunci.

**Fisika 60 Detik** — sepuluh kartu konsep (efek Doppler, langit biru, konduksi listrik logam, entropi,
es mengapung, superkonduktor, oven mikrogelombang, sisi jauh Bulan, neutrino, termometer tembak),
masing-masing dengan infografis SVG dan tautan silang.

**Nobel Fisika** — 37 tahun penghargaan (1901–2026) dengan kutipan resmi panitia Nobel dalam bahasa asli,
penjelasan bahasa Indonesia, tautan ke halaman resmi tiap tahun, pencarian nama/topik, dan penyaring era.

**Tentang Kami** — profil 5 anggota (peran masing-masing), tabel kontribusi, metode kerja
(verifikasi fakta, aset visual, aksesibilitas), ringkasan enam fitur jelajah, **pernyataan penggunaan AI**,
dan peta situs.

**Sumber** — seluruh rujukan dikelompokkan per artikel dan per halaman jelajah, ditandai jenisnya
(artikel jurnal / buku ajar / lembaga resmi / data terbuka / ensiklopedia daring), plus tabel lisensi aset.

**Log AI** — 20 baris catatan penggunaan AI: tujuan, ringkasan prompt, serta revisi dan verifikasi
yang dilakukan manusia.

---

## 5. Lima widget interaktif

| Widget | Halaman | Cara mencoba | Konsep fisika yang ditunjukkan |
|---|---|---|---|
| Simulator layar sentuh | `touchscreen.html` | Pilih objek (jari, kuku, pensil, sarung tangan biasa/touchscreen, stylus), atur tekanan, ketuk area layar | Kapasitansi pelat sejajar, lapisan ITO, konduktor vs isolator |
| Simulator optika pelangi | `pelangi.html` | Klik tahap 1–4 untuk mengikuti jalur cahaya; geser ketinggian pengamat | Refraksi, dispersi, refleksi internal, kerucut pandang 42° |
| Simulator ANC | `anc.html` | Nyalakan bising → anti-noise → ANC; bandingkan suara konsisten vs mendadak | Superposisi gelombang, interferensi destruktif, batas ANC |
| Simulator kabut kopi | `asap-kopi.html` | Ubah suhu ruangan, pilih ruangan ber-AC / panas lembap / tiup permukaan | Penguapan, kejenuhan uap, kondensasi, hamburan cahaya |
| Simulator air fryer | `air-fryer.html` | Panaskan pemanas → aktifkan kipas → mulai memasak | Konveksi paksa, perpindahan panas, reaksi Maillard |

Semua animasi hanya berjalan ketika elemen terlihat di layar (hemat baterai), menghormati pengaturan
`prefers-reduced-motion`, dan menyediakan teks alternatif untuk pembaca layar. Setiap angka pada
simulasi diberi label **ilustratif** dan tidak diposisikan sebagai hasil pengukuran laboratorium.

---

## 6. Panjang artikel

Dihitung dari **badan artikel** (isi `<article class="prose">`), tanpa menghitung label antarmuka
widget, teks pilihan polling, kotak fakta, dan daftar sumber:

| Halaman | Badan artikel | Seluruh halaman artikel |
|---|---|---|
| `touchscreen.html` | ± 860 kata | ± 1.077 kata |
| `pelangi.html` | ± 690 kata | ± 894 kata |
| `anc.html` | ± 701 kata | ± 886 kata |
| `asap-kopi.html` | ± 736 kata | ± 924 kata |
| `air-fryer.html` | ± 794 kata | ± 1.026 kata |

Semuanya berada dalam rentang **600–900 kata** sesuai ketentuan tugas.

---

## 7. Sumber dan verifikasi

Rujukan utama (semuanya diperiksa lewat DOI/data penerbit, bukan sekadar keluaran AI):

| Topik | Rujukan | DOI |
|---|---|---|
| Layar sentuh | Gray, T. (2019), *Projected Capacitive Touch*, Springer | `10.1007/978-3-319-98392-9` |
| Pelangi | Adam, J. A. (2002), *Physics Reports* 356, 229–365 | `10.1016/S0370-1573(01)00076-X` |
| ANC | Kuo, S. M., & Morgan, D. R. (1999), *Proc. IEEE* 87(6) | `10.1109/5.763310` |
| Air fryer | Teruel dkk. (2015), *J. Food Sci.* 80(4) | `10.1111/1750-3841.12753` |
| Reaksi Maillard | Martins dkk. (2000), *Trends Food Sci. Technol.* 11 | `10.1016/S0924-2244(01)00022-X` |

Ditambah buku ajar (Serway & Jewett; Halliday, Resnick & Walker; Wallace & Hobbs; Çengel & Boles),
buku optika atmosfer (Greenler, 1980), serta materi lembaga resmi (NASA, NOAA, USGS).

Rujukan untuk halaman jelajah:

| Halaman | Rujukan utama | DOI / tautan |
|---|---|---|
| Science History | EHT Collaboration (2019), *ApJL* 875, L1 (foto lubang hitam) | `10.3847/2041-8213/ab0ec7` |
| Science History | Abbott dkk. (2016), *PRL* 116, 061102 (gelombang gravitasi) | `10.1103/PhysRevLett.116.061102` |
| Science History | Hubble (1929), *PNAS* 15, 168–173 | `10.1073/pnas.15.3.168` |
| Science History | Bardeen, Cooper & Schrieffer (1957), *Phys. Rev.* 108, 1175 | `10.1103/PhysRev.108.1175` |
| Data Lab | Open-Meteo — *Free Weather Forecast API* (CC BY 4.0) | `open-meteo.com/en/docs` |
| Data Lab | Alduchov & Eskridge (1996), *J. Appl. Meteorol.* 35, 601–609 | `10.1175/1520-0450(1996)035<0601:IMFAOS>2.0.CO;2` |
| Glosarium / 60 Detik | OpenStax, *University Physics* (buku ajar terbuka) | `openstax.org/details/books/university-physics-volume-1` |
| Nobel | Kutipan resmi per tahun + fakta & angka | `nobelprize.org/prizes/lists/all-nobel-prizes-in-physics/` |
| Physics News | 8 berita dari penerbit aslinya (IceCube, TU Wien, European XFEL, RIKEN, ESA, Quanta, MIT News, CERN) | lihat `physics-news.html` |

Seluruh DOI di atas diverifikasi melalui **API Crossref** (judul dan tahun dicocokkan satu per satu),
dan seluruh tautan diuji pada 17 Oktober 2026. Tautan DOI penerbit Wiley, PNAS, dan IEEE membalas
`403`/`202` kepada perayap otomatis, tetapi tetap valid saat dibuka di peramban biasa. Satu tautan
ensiklopedia yang gagal kami periksa (`britannica.com`) kami ganti dengan halaman lembaga resmi.

Dua skrip bantu disediakan agar pemeriksaan ini bisa diulang kapan saja:

```powershell
python tools/check_links.py   # memastikan semua tautan lokal, aset, dan jangkar tidak putus
python tools/count_words.py   # memastikan setiap artikel tetap 600-900 kata
```

---

## 8. Aset dan lisensi

* **Ilustrasi, diagram, ikon, logo, dan favicon** — dibuat sendiri oleh kelompok dalam format SVG asli
  (termasuk 10 infografis Fisika 60 Detik dan monogram potret fisikawan).
* **Grafik Data Lab** — digambar langsung oleh kode JavaScript (SVG), bukan gambar pihak ketiga.
* **Video hero** (`assets/hero-physiverse.mp4`, 1280×720, 30 fps, 12 detik, tanpa suara) dan
  **poster** (`assets/hero-poster.jpg`) — **dirender sendiri oleh kelompok** memakai Python
  (`tools/make_hero_video.py`). Tidak ada rekaman atau musik pihak ketiga.
* **Data cadangan** — `assets/data-fallback-jakarta.json` dan salinan `assets/data-fallback-jakarta.js`
  (agar tetap terbaca di mode `file://`) berisi snapshot nyata dari API Open-Meteo
  (lisensi data CC BY 4.0), diambil sekali dengan `tools/fetch_fallback_data.py`.
* **Jenis huruf** — Fraunces, Plus Jakarta Sans, Inter, JetBrains Mono, dimuat melalui Google Fonts
  (SIL Open Font License).
* Jika di kemudian hari kelompok menambahkan foto dari luar, sumber dan lisensinya **wajib**
  dicantumkan pada tabel lisensi di `sumber.html`.

---

## 8b. Cara kerja Data Lab dan hero video

**Hero video** dibangun dengan elemen `<video autoplay muted loop playsinline>` yang diberi `poster`
sebagai cadangan. Ada tombol jeda, dan tombol "Tonton film" membuka `<dialog>` berisi video yang sama
dengan suara. Bila pembaca mengaktifkan *prefers-reduced-motion*, video otomatis dijeda.

**Data Lab** memanggil `https://api.open-meteo.com/v1/forecast` (tanpa kunci API) untuk kota yang dipilih.
Grafiknya digambar manual sebagai SVG. Karena data ini prakiraan model, bukan pengukuran stasiun resmi,
halaman menyarankan penggunaan data BMKG untuk keperluan resmi dan menampilkan status sumber
(langsung / cadangan tersimpan).

---

## 9. Penggunaan AI

AI generatif dipakai sebagai alat bantu (kerangka kode, usulan struktur narasi, penyuntingan bahasa).
Seluruh fakta, angka, dan referensi tetap diverifikasi ke jurnal/buku/lembaga resmi.
Catatan lengkapnya ada di `log-ai.html`, dan ringkasan pernyataannya ada di `tentang.html`.

---

## 10. Anggota kelompok

| Nama | NIM | Peran | Artikel yang ditulis |
|---|---|---|---|
| Alifah Salwa Susanto | 1306624032 | Project Manager | Headphone ANC |
| Muhammad Lutfi | 1306624001 | Prompt Engineer & Developer | Layar sentuh |
| Rianti Fauziah | 1306624045 | Content Writer | Pelangi |
| Samqifah Muti Alisia | 1306624037 | Researcher & Fact-Checker | Air fryer |
| Dyah Dajinta Najla Gutomo | 1306624029 | Designer & Publisher | “Asap” minuman panas |

Dosen pengampu: Mega Ayu Permatasari, M.Si.

---

## 11. Kesesuaian dengan ketentuan tugas

| Ketentuan | Status | Keterangan |
|---|---|---|
| Beranda: nama website, tagline, deskripsi, sorotan artikel | ✅ | `index.html` |
| Minimal 3 artikel populer, 600–900 kata per artikel | ✅ | 5 artikel (empat penulis berbeda-beda) |
| Satu artikel per anggota | ✅ | Lihat tabel bagian 10 |
| Ilustrasi/gambar pada setiap artikel | ✅ | Diagram SVG asli + keterangan gambar |
| Daftar sumber tiap artikel + halaman sumber terpadu | ✅ | Bagian “Daftar sumber” dan `sumber.html` |
| Halaman Tentang Kami: profil, peran, pernyataan AI | ✅ | `tentang.html` |
| Tampilan responsif | ✅ | Titik henti 960 px / 900 px / 860 px / 760 px |
| Navigasi konsisten di semua halaman | ✅ | Header dan footer seragam |
| SEO dasar: judul + meta deskripsi tiap halaman | ✅ | `title`, `description`, `canonical`, Open Graph, JSON-LD |
| Gambar berlisensi bebas / karya sendiri | ✅ | Seluruhnya SVG buatan sendiri + video dirender sendiri |
| Beranda bergaya majalah dengan hero video | ✅ | `index.html` + `assets/hero-physiverse.mp4` |
| Science History + "Physicists Who Changed the World" | ✅ | `science-history.html` |
| Physics News dengan sumber terverifikasi | ✅ | `physics-news.html` (8 berita 2026) |
| Physiverse Data Lab interaktif | ✅ | `data-lab.html` (API Open-Meteo + cadangan offline) |
| Glosarium fisika yang bisa dicari | ✅ | `glosarium.html` (± 100 istilah) |
| Fisika 60 Detik | ✅ | `detik-60.html` (10 kartu) |
| Nobel Fisika | ✅ | `nobel.html` (37 tahun, kutipan resmi) |
| Dipublikasikan online | ⬜ | Ikuti langkah bagian 3 |
| Log penggunaan AI | ✅ | `log-ai.html` |
| Bebas referensi fiktif | ✅ | Seluruh DOI & tautan diverifikasi |

---

## 12. Yang masih perlu dilengkapi kelompok

1. **Publikasikan** ke GitHub Pages/Netlify/Vercel, lalu perbarui domain pada `canonical`, `og:url`,
   `robots.txt`, `sitemap.xml`, dan JSON-LD. Kalau platformnya hanya menerima `.html`, pakai
   **versi satu-berkas** (bagian 3b) dan jalankan ulang `tools/build_single_file.py` setelah
   domain diperbarui.
2. **Ulangi nama berkas** pengumpulan sesuai format `UTS_DRP_[Kelas]_Kel[No]_[NamaWebsite]`.
3. **Perbarui tanggal** (`datePublished` / tanggal pada halaman) jika tanggal unggah berubah.
4. **Hapus berkas pengembangan** `assets/_preview-hero.jpg` sebelum dikumpulkan (hanya dipakai saat
   menyusun video hero).
5. **Lengkapi log AI versi panjang** (tanggal dan tangkapan layar percakapan) sebagai lampiran laporan,
   karena tabel di `log-ai.html` sengaja dibuat ringkas.
6. **Isi lampiran tambahan** bila dosen meminta laporan tertulis, notulensi rapat, atau tangkapan layar uji ponsel.
   Sertakan bukti uji responsif (mis. tangkapan layar pada lebar 390 px dan 768 px).

---

## 13. Keterbatasan yang kami sadari

* Simulasi adalah **model yang disederhanakan** untuk tujuan edukasi, bukan hasil pengukuran laboratorium.
  Setiap widget diberi catatan ini secara eksplisit.
* Polling tidak memakai server: pilihan disimpan di peramban masing-masing pembaca, sehingga angkanya
  bersifat ilustratif (nilai awal diberikan agar batang grafik tidak kosong sebelum ada yang menjawab).
* Beberapa tautan lembaga resmi hanya menunjuk ke halaman utama (bukan subhalaman spesifik) karena
  struktur situs lembaga tersebut sering berubah.
* **Data Lab bukan alat prakiraan resmi.** Angkanya berasal dari model prakiraan Open-Meteo, bukan
  pengukuran stasiun BMKG; untuk keperluan penerbangan, pertanian, atau kebencanaan gunakan data resmi.
  Titik embun dan kelas Beaufort dihitung dengan rumus yang dicantumkan pada halaman, sehingga bisa saja
  berbeda sedikit dari tabel lembaga lain.
* **Kabar riset cepat berubah.** Halaman Physics News adalah potret pada 17 Oktober 2026; tautan resmi
  selalu lebih baru daripada ringkasan kami, dan kami hanya memakai berita yang bisa dibuka di penerbitnya.
