# Twibbon PTA 2026

Website twibbon (HTML+JS, tanpa build step) untuk unggah foto atau video, mengatur posisinya di dalam bingkai "I'm Ready For — PTA 2026", lalu mengunduh hasil akhirnya. Tersedia dua varian bingkai — **Peserta** dan **Panitia** — yang dipilih di awal sebelum mengunggah media.

## Isi folder
- `index.html` — seluruh aplikasi (UI, pemilihan peran, logika kanvas, perekaman, konversi MP4)
- `frame-peserta.png` — bingkai varian Peserta (area foto sudah dibuat transparan)
- `frame-panitia.png` — bingkai varian Panitia (area foto sudah dibuat transparan)
- `vercel.json` — konfigurasi minimal untuk deploy statis
- `.gitignore`

## Menjalankan lokal
Buka `index.html` langsung di browser, atau jalankan server statis:
```
npx serve .
```
Fitur ekspor video memuat pustaka konverter dari CDN saat pertama dipakai, jadi perlu koneksi internet aktif.

## Deploy ke Vercel
**Tanpa Git:**
1. `npm i -g vercel`
2. Di dalam folder ini: `vercel`
3. Ikuti prompt, terima default (situs statis, tidak perlu build command)

**Lewat Git + Dashboard Vercel:**
1. Push folder ini ke repo GitHub
2. Buka https://vercel.com/new, import repo tersebut
3. Framework preset: "Other", tanpa build command / output directory
4. Deploy

## Fitur
- Pilih peran (Peserta atau Panitia) terlebih dahulu — tombol unggah foto/video terkunci sampai salah satu dipilih. Peran bisa diganti kapan saja tanpa kehilangan media yang sudah diunggah.
- Unggah foto atau video, geser dan perbesar langsung pada pratinjau untuk memposisikan di dalam bingkai. Pratinjau digambar dengan mesin canvas yang sama persis dengan proses ekspor, jadi hasil akhirnya taat sama seperti yang terlihat saat mengatur posisi — bingkai selalu diam, hanya media yang berpindah.
- Tombol unduh baru aktif setelah media benar-benar selesai dimuat dan didekode browser (bukan langsung setelah file dipilih), sehingga hasil ekspor tidak pernah menangkap data yang belum utuh.
- Unduh foto: instan, hasil PNG lewat `<canvas>` dalam resolusi penuh sesuai bingkai yang dipilih
- Unduh video: direkam frame-by-frame ke `<canvas>` (mengikuti durasi asli, maks 30 detik), lalu dikonversi menjadi **MP4 asli (H.264)** di browser menggunakan `ffmpeg.wasm` — hasilnya kompatibel diputar di HP, WhatsApp, dan pemutar video pada umumnya
- Nama file unduhan menyertakan peran yang dipilih (mis. `twibbon-pta2026-peserta.png`)
- Semua proses berjalan di perangkat pengguna sendiri; tidak ada file yang dikirim ke server

## Catatan teknis
- Kedua bingkai menggunakan resolusi asli 3375×3375 tanpa kompresi sehingga hasil unduhan foto/video tetap tajam dalam resolusi penuh.
- Posisi lubang tiap bingkai didefinisikan sebagai koordinat relatif di objek `FRAMES` dalam `index.html` (properti `hole` per peran) — sesuaikan di sana jika mengganti salah satu file bingkai. Bingkai Panitia lubangnya dideteksi lewat tepi geometris (bukan deteksi warna putih) karena area placeholder-nya berisi ilustrasi langit/awan, bukan kotak putih polos.
- Konversi MP4 memuat `@ffmpeg/core` (~25 MB) dari CDN unpkg saat tombol unduh video pertama kali ditekan; percobaan pertama akan terasa lebih lambat dari percobaan berikutnya.
