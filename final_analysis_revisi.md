# Analisis Final & Dampak Teknis Revisi PT BMJ

Dokumen ini menyajikan analisis mendalam terhadap seluruh permintaan revisi client, korelasi dengan bug yang sudah ditemukan, dampak teknis, serta roadmap implementasi prioritas.

---

## 1. Korelasi Bug Client dengan Vulnerability Report

Berikut adalah bug yang **sudah diketahui dari audit** dan **dikonfirmasi langsung oleh client**:

| Bug dari Client | Lokasi di Vulnerability Report | Status & Dampak |
|-----------------|---------------------------------|-----------------|
| Print dari Histori beda harga total (yg benar) vs print langsung | Mobile App: Potensi Data Loss / Logika History Print | KRITIS: Menunjukkan adanya inkonsistensi perhitungan harga Grosir/Retail pada saat menyimpan vs mengambil data history. |
| Stok berubah sendiri tanpa di-input | Backend: Race Condition / Transaksi Tidak Lengkap | KRITIS: Sesuai temuan `Race Condition` pada `oosCount` dan logika stok yang belum sepenuhnya atomic. |
| Harga di nota toko Grosir jadi Retail tanpa potongan | Backend/Print Service: Logika Klasifikasi Toko | KRITIS: Kesalahan validasi `shopType` saat generate nota. |

---

## 2. Analisis Dampak Teknis & Kebutuhan Implementasi (Detail)

### Bagian A: APLIKASI MOBILE (Kompleksitas: TINGGI)

#### Fitur: Rollover Stok (Stok Akhir → Stok Awal Hari Berikutnya)
- **Dampak Besar**: Membutuhkan **perubahan skema database** dan **service backend baru**.
- **File Terkait**:
  - Backend: `schema.prisma`, `src/modules/restock/services/restock.service.ts`, `src/modules/stocks/services/stock.service.ts`.
  - Mobile: `lib/features/restock/cubit/stock_cubit.dart`, `lib/core/database/database_helper.dart`.
- **Risiko**: Jika rollover gagal, stok salesman bisa kacau total. **Harus atomic**.

#### Fitur: Print Nota 5 Detik Jeda & Perbaikan Harga
- **File Terkait**:
  - Mobile: `lib/core/services/thermal_print_service.dart`, `lib/core/services/pdf_service.dart`.
  - Backend: Pastikan `shopType` disertakan dan dikonsistensikan di semua endpoint restock.

---

### Bagian B: WEBSITE ADMIN (Kompleksitas: SEDANG-TINGGI)

#### Fitur: CALL SHEET (Format Excel Multi-Sheet)
- **Dampak Besar**: Merupakan fitur **core business baru**.
- **Teknis**:
  - Gunakan library `xlsx` untuk generate file Excel dengan banyak sheet.
  - Endpoint baru untuk aggregate data per produk, per salesman, per hari.
- **File Terkait**:
  - Backend: Buat service baru `CallSheetService.ts` dan endpoint baru.
  - Frontend: `src/app/` (buat halaman baru `callsheet/page.tsx`).

---

### Bagian C: BACKEND (Kompleksitas: TINGGI)

#### Fitur: Validasi GPS 100m Sistem & 10m Tampilan
- **File Terkait**: `src/modules/visits/services/visit.service.ts`.
- **Perubahan**: Ubah konstanta threshold jarak dan pastikan **simpan kedua jarak** (yang ditampilkan user vs yang diverifikasi server) di database untuk audit.

---

## 3. Roadmap Prioritas Implementasi

Disarankan untuk mengimplementasikan dalam urutan berikut untuk meminimalkan risiko:

### Phase 1: PERBAIKAN BUG KRITIS (URGENT)
1.  **Fix Perhitungan Harga Grosir/Retail** di semua tempat (Print, History, Save).
2.  **Bungkus Semua Transaksi Stok/Restock** ke dalam `prisma.$transaction`.
3.  **Perbaiki Logika Print History** untuk konsistensi harga.

### Phase 2: FITUR MOBILE PENTING
1.  Ubah Validasi GPS (100m server, 10m tampilan).
2.  Tambahkan Nama PT BJM di Print Toko.
3.  Filter History Toko Sendiri.

### Phase 3: OVERHAUL MANAJEMEN STOK
1.  Ubah Skema Database untuk mendukung Rollover Stok.
2.  Matikan Update Stok Manual.
3.  Buat Tab Baru "Input Stok Awal".

### Phase 4: FITUR WEBSITE BARU
1.  Implementasi **CALL SHEET** (Multi-Sheet Excel).
2.  Edit GPS Manual di Web.
3.  Filter Toko Per Wilayah.
4.  Menu Keberadaan Barang.

---

## 4. Kesimpulan Akhir
Sistem saat ini **sudah stabil** untuk berjalan, namun **bug harga dan stok** harus diperbaiki terlebih dahulu sebelum menambah fitur baru karena risiko data korupsi sangat tinggi.
