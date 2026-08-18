# Twibbon PTA 2026

Website twibbon sederhana (HTML+JS, tanpa build step) yang bisa upload foto atau video, lalu mengunduh hasil komposit dengan frame "I'm Ready For — PTA 2026".

## Isi folder
- `index.html` — seluruh aplikasi (UI + logika canvas/recording)
- `frame.png` — frame dengan area foto sudah dibuat transparan

## Menjalankan lokal
Buka `index.html` langsung di browser, atau jalankan server statis:
```
npx serve .
```

## Deploy ke Vercel
**Cara tercepat (tanpa Git):**
1. Install Vercel CLI: `npm i -g vercel`
2. Di dalam folder ini, jalankan: `vercel`
3. Ikuti prompt (pilih akun, nama project, terima default settings — tidak perlu build command karena ini situs statis)
4. Setelah selesai, Vercel akan memberi URL live

**Lewat Git + Dashboard Vercel:**
1. Push folder ini ke repo GitHub baru
2. Buka https://vercel.com/new, import repo tersebut
3. Framework preset pilih "Other" (statis), tidak perlu build command / output directory
4. Klik Deploy

## Catatan teknis
- Unduh foto: instan, hasil PNG 1080x1080 lewat `<canvas>`.
- Unduh video: video digambar frame-by-frame ke `<canvas>` lalu direkam ulang dengan `MediaRecorder` menjadi file `.webm` (mengandung frame). Proses ini berjalan real-time mengikuti durasi video asli (dibatasi maks 30 detik).
- Semua proses terjadi di browser pengguna — tidak ada server/backend, tidak ada upload data ke mana pun.
