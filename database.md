# Database Schema Documentation

## Tabel Utama & Relasi

### Pengguna & Keamanan
- **User**: Menyimpan data salesman dan admin (Role: STAFF, ADMIN, SUPER_ADMIN).
- **RefreshToken**: Manajemen sesi JWT.
- **SecurityLog**: Mencatat anomali keamanan.

### Toko & Wilayah
- **Shop**: Data toko (Nama, Alamat, Koordinat, Tipe Toko).
- **Regency, District, Village**: Struktur wilayah geografis.
- **ShopStatus**: Enum (PENDING, APPROVED, REJECTED).

### Aktivitas Lapangan
- **Visit**: Mencatat setiap kunjungan (Check-in time, Koordinat GPS, Foto).
- **DailyRoute**: Rencana rute harian untuk salesman.
- **DailyRouteShop**: Toko-toko yang harus dikunjungi dalam rute tertentu.

### Produk & Transaksi
- **Product**: Detail produk, harga (Grosir/Retail), dan kategori.
- **Brand & Category**: Pengelompokan produk.
- **RestockReport**: Header transaksi penjualan saat kunjungan.
- **RestockItem**: Detail produk yang terjual dalam satu laporan restock.
- **Unit**: Satuan produk (Bungkus, Slop, Dus).

### Inventaris & Performa
- **SellerInventory**: Stok yang dibawa oleh salesman di kendaraan mereka.
- **InventoryMovement**: Log masuk/keluar stok salesman (diambil dari gudang vs terjual ke toko).
- **Expense**: Pengeluaran operasional salesman (Bensin, Makan, Parkir).
- **UserReward**: Poin dan reward yang didapat salesman.
- **Target**: Target penjualan atau kunjungan yang ditetapkan untuk salesman.

## Diagram Relasi (Ringkasan)
- `User` membuat `Shop`.
- `User` melakukan `Visit` ke `Shop`.
- `Visit` menghasilkan `RestockReport`.
- `RestockReport` berisi banyak `RestockItem` yang merujuk ke `Product`.
- `SellerInventory` diperbarui berdasarkan `InventoryMovement` (Sale dari `RestockItem`).
- `Shop` terikat ke `Village` -> `District` -> `Regency`.
