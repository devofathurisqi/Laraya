# LARAYA

Indah dalam Setiap Momen. Katalog dan dashboard mandiri berbasis React 19, Vite, dan Supabase. Branch produksi: **PROD**.

## Upload foto otomatis — 5 November 2026

Dashboard menerima foto asli JPG, PNG, dan WebP tanpa batas ukuran file atau megapiksel yang ditetapkan aplikasi. Klien tidak perlu mengompres manual. Berlaku untuk galeri produk dan foto setiap varian warna; jumlah galeri tetap delapan foto per produk.

Optimasi berada di aplikasi sebelum transfer ke Supabase, menggunakan background worker jika didukung dan jalur native browser sebagai cadangan. Header dibaca terlebih dahulu agar decoder dapat mengecilkan foto besar; orientasi kamera, rasio aspek dan transparansi dipertahankan. Target sisi terpanjang 2.048 px tanpa memperbesar foto kecil. Encoder mencoba kualitas WebP 90/86/82%; jika masih berat, resolusi disesuaikan otomatis dengan tetap memakai sumber decode yang sama. Browser/perangkat tetap harus mampu membaca file aslinya.

Hanya hasil WebP maksimal 1 MiB yang dikirim ke bucket catalog-images, sehingga batas internal Storage tidak menjadi pekerjaan klien. Tidak ada perubahan database/RLS, bucket, data toko, foto lama, atau master Vuyama. Path UUID baru dan cache satu tahun tetap dipakai. Dashboard menampilkan status penyiapan/pengunggahan dan menahan penyimpanan produk sampai upload selesai.

Engine: src/image-metadata.js, src/image-optimizer.js, src/image-worker.js, src/image-upload.js. Integrasi: src/api.js dan src/Admin.jsx. Uji regresi: tests/image-upload.test.mjs. Lihat [verifikasi upload](qa/UPLOAD-2026-11-05.md).

## Menjalankan

Node.js 22+ diperlukan. Dari root repository:

```powershell
npm ci
npm run dev
npm test
npm run build
npm run preview
```

Katalog: http://127.0.0.1:4186/. Dashboard: /admin. Username **laraya**; password yang diminta pemilik sudah diset dan diverifikasi melalui Supabase Auth. Password dan secret key tidak disimpan di repository atau frontend.

## Data online

Tenant: `3f55db86-cfd0-43e0-ab2d-243fddcd537b`. Slug: `laraya`. Kontak pemesanan: **+6282357873701**. 27 listing Vuyama dengan 468 pilihan warna.

Seluruh harga awal mengikuti referensi eceran Vuyama, bukan harga khusus Sugarshawl. Harga eceran yang belum terverifikasi ditandai Tanyakan harga.

Satu project Supabase dipakai bersama. Akun ini hanya menjadi anggota tenant sendiri; RLS menjaga draft, perubahan data, dan upload. Master/foto Vuyama tetap disimpan satu kali di library bersama. Listing menyimpan referensi, harga, stok, status dan override lokal; tidak mengunggah ulang foto supplier. Jangan menjalankan ulang migration/seed Novola.

## Alur belanja dan dashboard

Pencarian produk/bahan/warna, kategori, filter, urutan, favorit, detail lazy-loaded, foto per warna, pencarian warna, keranjang tersimpan per toko, jumlah, catatan, dan checkout WhatsApp. Harga, stok dan produk diperiksa kembali sebelum membuka draft WhatsApp. Harga belum pasti tidak dianggap gratis. Pesan belum dikirim otomatis; stok, pengiriman dan pembayaran dikonfirmasi admin.

Dashboard menyediakan ringkasan, CRUD produk/kategori, upload galeri/foto warna, stok, draft/publikasi, duplikasi, konflik revisi, aktivitas, pengaturan nama/tagline/WhatsApp, dan picker Katalog Vuyama. Produk dari picker dimulai sebagai draft.

## Brand

Ivory #FDF7F4, beige #F3E5DC, dusty rose #E6C7C1, mauve #D7A995, mocha #B7896F, champagne/soft gold #D4B78F. Font Playfair Display dan Montserrat lokal, dengan lisensi OFL. Logo primer diekstrak dari variasi logo asli pada board pengguna. Monogram L, bintang, garis melengkung, kisah cinta/keluarga/impian dan tagline menjadi arah desain. Kategori pelanggan hanya muncul jika ada produk; kategori ibadah dan hadiah dari board didukung katalog aktual.

## Deployment Vercel

Import repository ini, pilih branch produksi **PROD**, root **.**, preset **Vite**, build **npm run build**, output **dist**. Rewrite untuk /admin sudah ada di vercel.json. Konfigurasi publik project bersama tersedia di src/backend-config.js agar deploy awal dapat berjalan. Override opsional: VITE_SUPABASE_URL, VITE_SUPABASE_PUBLISHABLE_KEY, VITE_STORE_SLUG=laraya. Jangan menaruh secret/service-role key pada variabel VITE.

Perubahan katalog dari admin langsung tersimpan online tanpa deploy ulang. Kode yang di-push tidak otomatis membuktikan hosting Vercel sudah aktif.

## Verifikasi dan sumber

Lihat [hasil pemeriksaan](qa/RESULTS.md), [sumber produk](docs/SOURCES.md), dan [panduan admin](docs/PANDUAN-ADMIN.md). Mode ?preview=1 memakai fixture lokal; /admin?preview=1 hanya mengubah memori sesi. Pemakaian normal tidak memerlukan mode preview.
