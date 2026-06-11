# 📋 Revisi Fitur & Bug Report (PT BMJ)

Dokumen ini mencatat seluruh kebutuhan revisi dan perbaikan bug yang diminta oleh client (Pak Wahyu/FerzPedia) pada tanggal 11 Juni 2026.

---

## ⚠️ PENTING: NOT AWALAN
**JANGAN MENGHAPUS SEMUA DATA TOKO DI DATABASE** sampai fitur baru selesai diperbaiki.

---

## 1. Fitur & Perbaikan: APLIKASI MOBILE (Flutter)

### 1.1 Manajemen Toko
- **Penanda Siapa yang Membuat/Menandai Toko**: Tambahkan kolom `createdBy` dan `updatedBy` (Salesman ID) dan `markDate` untuk setiap toko.
- **Print Toko**: Tambahkan nama perusahaan **"PT. BERLIMDO JAYA MAKMUR (BJM)"** pada struk print.
- **Filter History Toko**: Hanya tampilkan toko yang dibuat/ditandai oleh salesman yang sedang login ("FILTER TOKO SENDIRI").
- **Verifikasi GPS**:
  - Sistem (Backend) harus memverifikasi jarak **100 METER**.
  - Tampilkan di layar aplikasi: Tampilkan jarak real-time dengan batas **10 METER** (untuk kejelasan user).

### 1.2 Nota (Struk Penjualan)
- **Jeda Print 5 Detik**: Waktu jeda minimal 5 detik antara print NOTA ASLI dan NOTA COPY.
- **Perhatikan Bug Print**:
  - **BUG 1**: Print dari History menunjukkan harga dan total yang berbeda dengan print langsung, padahal totalnya benar.
  - **BUG 2**: Untuk toko GROSIR, laporan/struk menampilkan harga RETAIL tanpa potongan harga grosir.

### 1.3 Penjualan
- **Search di BP**: Tambahkan fitur pencarian pada kategori BP (Barang Promosi).
- **Preferensi Nota (Titip & Cash)**: Perbarang, pilih tampilan mode CASH atau TITIP, dan tampilkan kedua status ini.

### 1.4 Stok (MAJOR OVERHAUL)
- **Filter Tanggal**: Tambahkan filter range tanggal (Per Hari - Per Minggu).
- **Alur Stok Baru**:
  - `Stok Akhir` hari ini otomatis menjadi `Stok Awal` hari esok.
  - **MATIKAN** tombol `UPDATE` stok manual untuk salesman.
  - **HAPUS** menu "Input Stok Baru" dari halaman ini.
- **TAB BARU (LOADING)**:
  - Judul: Input Stok Awal.
  - Ada tracking tanggal dan hari ketika stok di-input.
  - Fitur PRINT untuk TTD Salesman dan Gudang.

---

## 2. Fitur & Perbaikan: WEBSITE ADMIN (Next.js)

### 2.1 Toko & Wilayah
- **Menu Toko (Cash/Credit)**: Tambahkan filter dan kolom status pembayaran (Cash/Credit).
- **Edit GPS Manual**: Izinkan admin mengedit koordinat GPS toko secara manual di web ("edit manual di GPS di web dibuka. nambah dari..").
- **Hapus Modal**: Hapus fitur konfirmasi modal untuk aksi cepat? Atau mungkin "Hapus Master Wilayah" (Tentukan).
- **Filter Toko Per Wilayah**: Tambahkan filter data tabel toko berdasarkan Kabupaten, Kecamatan, dan Desa.

### 2.2 CALL SHEET (Fitur Baru Utama)
- **Filter Lengkap**:
  - Hari & Tanggal.
  - Nama Salesman.
  - Nama Produk.
- **Status Toko Tutup**: Otomatis menandai toko sebagai Tutup berdasarkan data terhadap SEMUA produk.
- **Hapus Kolom Nilai Transaksi**: Hilangkan kolom nominal dari tampilan Call Sheet.
- **Print Format Excel**:
  - Bentuk print sesuai format Excel `00-COPYAN CS.xlsx`.
  - Satu file, tiap produknya menjadi SHEET tersendiri (misal: SHEET 1 = REXO, SHEET 2 = DUO).
- **Grouping Status**:
  - NO OOSC RO.
  - SMA BP (munculkan catatannya).
  - OOS (tercentang).

### 2.3 Menu Baru: Keberadaan Barang
- Fitur untuk melihat ketersediaan barang (Availability).

---

## 3. Peta Kebutuhan ke Kode (Implementation Guide)

### 3.1 Backend (nagha-stock-backend)
1. **Model Toko (schema.prisma)**:
   - Tambahkan `createdBy`, `updatedBy`, `markDate`.
   - Ubah logika validasi GPS di `VisitService` ke 100 meter.
2. **Service Restock & Stok**:
   - Logika rollover stok (Akhir → Awal).
   - Endpoint baru untuk Call Sheet.
3. **Service Print & History**:
   - Perbaiki perhitungan harga Grosir/Retail di history print.

### 3.2 Mobile App (nagha_stock_app)
1. **Halaman Toko & Print**: Ubah `pdf_service.dart` atau `thermal_print_service.dart`.
2. **Halaman Stok**: Buat UI tab baru dan edit alur di `StockCubit`.
3. **Sinkronisasi**: Perbarui logika sinkronisasi stok di `sync_service.dart`.

### 3.3 Frontend Web (nagha-stock-web)
1. **Halaman Toko & Call Sheet**: Tambahkan komponen baru di `src/app/`.
2. **Fitur Export Excel**: Gunakan `xlsx` package untuk membuat multi-sheet.

---

## 4. Lampiran Referensi
- File Excel: `/home/firman/Downloads/pakwahyu/RevisianResearch/00-COPYAN CS.xlsx`.
- Gambar referensi di `RevisianResearch/`.
