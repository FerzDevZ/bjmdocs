# Deep Dive Database (Prisma Schema Analysis)

Database Berlimdo dirancang untuk menangani integritas data transaksional yang tinggi dengan relasi yang kompleks antara pengguna, lokasi geografis, dan pergerakan stok.

## Model Relasi Utama

### 1. Entitas Pengguna (`User`)
- **Peran (Role)**: `STAFF` (Salesman), `ADMIN`, `SUPER_ADMIN`.
- **Atribut Gamifikasi**: Memiliki field `xp` dan `level` untuk melacak performa.
- **Relasi**: Membawahi `Visit`, `Expense`, `SellerInventory`, dan `DailyRoute`.
- **Security**: Menyimpan `deviceId` untuk penguncian perangkat.

### 2. Struktur Geografis (Geo-Hierarchy)
Relasi hirarkis untuk akurasi data wilayah:
- `Regency` (Kabupaten) -> `District` (Kecamatan) -> `Village` (Desa).
- Setiap `Shop` (Toko) wajib terikat pada level `Village`.
- `User` (Salesman) dapat ditugaskan ke beberapa `District` tertentu.

### 3. Logika Kunjungan & Fraud Prevention (`Visit`)
Setiap baris di tabel `Visit` mencatat:
- `gpsLat` & `gpsLong`: Koordinat aktual saat check-in.
- `metadataFraud`: Flag otomatis jika sistem mendeteksi anomali.
- `simulatedMock`: Flag jika terdeteksi aplikasi Fake GPS.
- `isClosed`: Menandai jika toko sedang tutup saat dikunjungi.

### 4. Transaksi & Stok (`RestockReport` & `RestockItem`)
Ini adalah jantung dari sistem penjualan:
- **RestockReport**: Header transaksi yang terikat pada satu `Visit`.
- **RestockItem**: Detail item yang terjual. Memiliki field kuantitas yang sangat detail:
    - `qty_no`: Quantity New (Stok Baru).
    - `qty_ro`: Repeat Order.
    - `qty_oos`: Out of Stock.
    - `qty_bp`: Barang Promosi.

### 5. Inventaris Salesman (`SellerInventory` & `InventoryMovement`)
- **SellerInventory**: Saldo stok saat ini yang dibawa oleh salesman di motor/mobil.
- **InventoryMovement**: Log setiap kali stok bertambah (ambil dari gudang) atau berkurang (terjual ke toko). Ini memastikan audit stok bisa dilakukan kapan saja.

---

## Enums (Konstanta Database)

- **ShopType**: `RETAIL`, `GROSIR`, `SEMI_GROSIR`. Digunakan untuk membedakan struktur harga.
- **PaymentType**: `CASH`, `CREDIT`.
- **Role**: `STAFF`, `ADMIN`, `SUPER_ADMIN`.

---

## Optimasi Database
- **Indexing**: Indeks diletakkan pada field yang sering dicari seperti `userId`, `shopId`, `date` pada `DailyRoute`, dan koordinat geografis.
- **Cascading**: Penghapusan user akan menghapus `RefreshToken` secara otomatis (Cascade) tetapi tetap mempertahankan log transaksi untuk audit.
