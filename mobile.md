# Mobile App Documentation (nagha_stock_app)

## Teknologi
- **Framework**: Flutter
- **State Management**: BLoC / Cubit
- **Networking**: Dio
- **Database Lokal**: SQLite (via sqflite)
- **Hardware Integration**: Camera (QR Scan), GPS (Location), Thermal Printer (Bluetooth).

## Struktur Folder Utama
- `lib/core/`: Konfigurasi API, Database Helper, dan layanan umum (PDF, Sync, Location).
- `lib/features/`: Modul fitur aplikasi:
    - `auth/`: Login dan manajemen sesi.
    - `home/`: Dashboard salesman dan daftar toko.
    - `restock/`: Proses transaksi, keranjang belanja, dan manajemen produk.
    - `inventory/`: Cek stok salesman.
- `lib/ui/screens/`: Berbagai layar aplikasi (Check-in, Input Stok, Riwayat, Profil).
- `lib/data/datasources/`: Menangani komunikasi langsung dengan API backend.

## Alur Kerja Salesman
1. **Login**: Autentikasi dan pengecekan versi aplikasi.
2. **Kunjungan**: Memilih toko dari daftar atau rute harian.
3. **Check-in**: Melakukan scan QR toko dan validasi lokasi GPS.
4. **Operasional**:
    - Jika toko buka: Input stok saat ini dan lakukan restock (penjualan).
    - Jika toko tutup: Ambil foto bukti toko tutup.
5. **Transaksi**: Selesaikan transaksi, pilih metode pembayaran (Cash/Credit), dan cetak struk.
6. **Sync**: Data dikirim ke server secara real-time atau tersimpan lokal jika offline.

## Fitur Khusus
- **Fraud Detection**: Mendeteksi jika salesman menggunakan lokasi palsu (mock location).
- **Offline Support**: Menyimpan data transaksi sementara jika koneksi internet tidak stabil.
- **Thermal Printing**: Mencetak struk penjualan langsung ke printer bluetooth.
- **Update OTA**: Aplikasi dapat mendeteksi versi baru dan mengunduh APK pembaruan.
