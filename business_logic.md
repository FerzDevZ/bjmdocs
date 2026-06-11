# 🧠 Logika Bisnis, Anti-Fraud & Gamifikasi

Dokumen ini merinci algoritma dan aturan bisnis yang menjadi kecerdasan di balik sistem Berlimdo.

## 📍 1. Algoritma Validasi Lokasi (Geo-fencing)

Sistem menggunakan **Formula Haversine** untuk menghitung jarak lingkaran besar antara dua titik di permukaan bumi berdasarkan koordinat GPS.

- **Input**: `(lat1, lon1)` (Toko), `(lat2, lon2)` (Salesman).
- **Radius Toleransi**: 20 Meter.
- **Logika Server**:
  ```typescript
  // Contoh implementasi di Backend
  const distance = calculateDistance(shopLat, shopLong, userLat, userLong);
  if (distance > shop.radiusMeter) {
      throw new Error("OUT_OF_RANGE");
  }
  ```
- **Optimasi N+1 Query**: Menggunakan teknik *Bulk Fetch* untuk produk dan inventaris sebelum perulangan, yang secara signifikan mengurangi beban database.
- **Integritas Transaksional**: Penggunaan `prisma.$transaction` memastikan atomisitas data; jika satu langkah gagal (misal: stok tidak cukup), seluruh transaksi dibatalkan.
- **Pemisahan Logika Reward**: Modul XP dan Achievement berjalan di blok *asinkron* terpisah agar tidak menghambat proses utama penjualan.

---

## 🚫 2. Strategi Anti-Fraud (Pencegahan Kecurangan)

Berlimdo menerapkan sistem pertahanan berlapis terhadap manipulasi data:

1. **Hardware Check (Mobile)**: Aplikasi mendeteksi penggunaan *Mock Location Provider* dan *Developer Options* di Android.
2. **Device Fingerprinting**: Penguncian `deviceId` memastikan salesman tidak bisa berbagi akun atau login dari perangkat yang telah dimodifikasi (root).
3. **Photo Metadata Audit**: Foto bukti kunjungan diverifikasi timestamp-nya agar tidak menggunakan foto lama dari galeri (Direct Camera Only).
4. **Time-Window Validation**: Transaksi hanya diizinkan pada jam operasional yang telah ditentukan (Server-Time based).

---

## 🎮 3. Sistem Gamifikasi (XP & Leveling)

Untuk meningkatkan produktivitas, sistem menerapkan elemen RPG:

- **XP (Experience Points)**: Didapatkan dari:
    - Check-in toko (+XP).
    - Penjualan produk per unit (+XP).
    - Mencapai target harian (+Bonus XP).
- **Leveling**: Level user naik secara otomatis berdasarkan akumulasi XP. Level yang lebih tinggi dapat membuka akses ke insentif atau reward tertentu.
- **Leaderboard**: Visualisasi peringkat salesman berdasarkan XP bulanan di Dashboard Web.

---

## 📦 4. Aturan Manajemen Inventaris (Stock Rules)

Sistem melacak tiga jenis stok secara simultan:

1. **Stok Toko**: Dipantau melalui input `qty_oos` (Out of Stock). Jika produk tertentu sering OOS, sistem memberikan peringatan restock.
2. **Seller Inventory**: Stok fisik yang ada di kendaraan salesman.
3. **Inventory Movement Type**:
    - `RESTOCK_TAKEN`: Saat salesman mengambil barang dari gudang utama.
    - `SALE`: Pengurangan stok saat terjadi transaksi restock di toko.
    - `ADJUSTMENT`: Koreksi manual oleh admin jika ada barang rusak/hilang.

---

## 💰 5. Struktur Harga Dinamis
Harga produk otomatis berubah berdasarkan `ShopType`:
- **RETAIL**: Harga eceran tertinggi.
- **GROSIR**: Harga partai besar dengan diskon volume.
- **SEMI_GROSIR**: Harga menengah.
- Logika ini dikelola di `restock.service.ts` untuk memastikan akurasi nilai transaksi.
