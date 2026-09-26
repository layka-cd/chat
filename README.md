# PeerChat

Tampilan: sidebar tetap di layar besar (PC), jadi drawer geser (☰) di layar kecil (HP). Tema warna ungu gelap.

## PENTING: Supaya bisa konek WiFi <-> Data Seluler

Tanpa server TURN, chat cuma jalan kalau kedua orang di jaringan WiFi yang "ramah"
(rumah/kantor biasa). Begitu salah satu pakai **data seluler/4G/5G**, koneksi
hampir pasti gagal karena operator seluler pakai CGNAT yang tidak bisa
ditembus cuma pakai STUN.

Cara aktifkan TURN server (gratis, 20GB/bulan):

1. Daftar di https://dashboard.metered.ca/signup
2. Di dashboard, buat "App" baru — nanti dapat nama app (mis. `namaapp`) dan API Key
3. Copy `.env.example` jadi `.env` di folder proyek ini, isi:
   ```
   VITE_METERED_APP_NAME=namaapp
   VITE_METERED_API_KEY=xxxxxxxxxxxxx
   ```
4. Jalankan `npm run build` lagi (env var di-"bake" ke hasil build saat ini,
   jadi WAJIB build ulang setelah isi .env)
5. Drag & drop folder `dist` yang baru ke Netlify Drop lagi (menimpa yang lama)

Tanpa langkah ini, aplikasi tetap jalan tapi cuma andal di jaringan WiFi yang sama.

Chat real-time peer-to-peer (WebRTC) pakai React + Vite + PeerJS.
Tidak ada server Node.js/Socket.IO sendiri, tidak ada database — pesan hanya
lewat langsung antar browser dan hilang begitu tab ditutup.

## Cara jalankan di komputer

```
npm install
npm run dev
```

Buka http://localhost:5173 di dua tab/browser berbeda untuk mencoba.

## Cara deploy ke Netlify

```
npm run build
```

Lalu drag & drop folder `dist` ke https://app.netlify.com/drop

## Cara pakai (grup, maks 9 orang)

1. Semua orang sepakati satu kode ruang bebas (mis. `rahasia123`)
2. Setiap orang buka aplikasi, ketik nama + kode yang sama, klik "Masuk Ruang" — langsung masuk ke layar chat, tidak ada layar menunggu terpisah
3. Orang PERTAMA yang masuk kode itu otomatis jadi "hub" penghubung; semua yang masuk berikutnya (sampai total 9 orang) otomatis terhubung dan bisa saling chat real-time
4. Kalau orang pertama (hub) keluar/tutup tab, room ikut berakhir untuk semua — sisanya perlu bikin room baru dengan kode lain

Catatan: PeerJS memakai server broker publik gratis (`0.peerjs.com`) hanya
untuk proses "perkenalan" awal antar dua browser (signaling). Server itu
tidak pernah melihat atau menyimpan isi pesan chat kamu.
