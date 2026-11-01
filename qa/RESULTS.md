# Hasil pemeriksaan LARAYA

Tanggal: 1 November 2026 (Asia/Jakarta).

- **24 tes otomatis lulus**: validasi data, jumlah/stok/varian, harga eceran dan seleksi produk, keranjang campuran harga diketahui/belum pasti, WhatsApp, harga berubah, produk ditarik, dan sparse override master.
- **Production build lulus**, termasuk pemeriksaan aset responsive, seluruh referensi master/shared assets, tidak ada salinan foto supplier pada deployment, SDK Auth tidak di-preload pelanggan, dan tidak ada password/secret dalam bundle.
- Akun admin yang diminta berhasil login melalui Supabase Auth dan form browser /admin. Dashboard menampilkan data aktual: **27 produk / 7 kategori / 468 pilihan warna**.
- Pengujian backend online: CRUD produk/kategori, draft tidak dapat dibaca anonim atau toko lain, update/delete lintas tenant ditolak, konflik revision ditolak, kategori terpakai terlindungi, upload foto folder sendiri berhasil, upload lintas tenant ditolak, dan audit tersedia. Record/foto QA sementara dibersihkan. Rincian: hosted-results.json.
- Browser desktop dan HP: logo, hero, kategori, harga, pilihan warna dan keranjang diperiksa. Pencarian Modal Viscose → Beige berhasil; varian yang dipilih masuk keranjang dengan harga **Rp59.000**.
- Checkout nyata berhasil membuka halaman draft api.whatsapp.com ke **6282357873701**, berisi nama brand, produk, warna, jumlah, harga dan subtotal. Tidak ada pesan yang dikirim. Keranjang dan sesi login uji sudah dikosongkan/keluar.
- Editor produk GalleryGeulis memuat master dan seluruh 20 warna Bamboo Spandex. Dashboard kedua brand menggunakan palet dan kontak masing-masing.
- Novola dan master bersama tidak diubah oleh onboarding; masing-masing admin hanya memiliki membership toko sendiri.

## Bukti visual

- home-desktop.jpg
- home-mobile.jpg
- admin-mobile.jpg
- cart-mobile.jpg

## Batas operasional

Beberapa produk Vuyama belum mempunyai referensi harga eceran atau foto yang terverifikasi; aplikasi mempertahankan status konfirmasi/foto belum tersedia. Harga khusus Sugarshawl tidak digunakan.

Seluruh stok awal perlu konfirmasi admin. Checkout WhatsApp tidak mereservasi stok atau menerima pembayaran. Pemeriksaan ini tidak mengklaim skor Lighthouse atau sinkronisasi stok pemasok secara otomatis. Push GitHub bukan bukti deployment hosting; petunjuk Vercel ada di README.

Revisi navbar: latar logo dan navbar disamakan ke #FCF6F1; mode multiply dihapus dan hasil diperiksa melalui browser.

Pemeriksaan responsif terukur: 360, 768, dan 1366 px. Ketiga lebar tidak memiliki overflow horizontal atau gambar rusak pada gambar yang telah dimuat. Lihat responsive-results.json.
