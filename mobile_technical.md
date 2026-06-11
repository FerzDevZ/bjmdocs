# Teknis Aplikasi Mobile (Flutter Architecture)

Aplikasi mobile Berlimdo adalah heavy-duty field tool yang dibangun dengan Flutter untuk menangani operasional intensif di lapangan.

## Arsitektur Kode (Clean Architecture Approach)

Aplikasi dibagi menjadi lapisan yang jelas untuk memudahkan pemeliharaan:
- **Core**: Berisi infrastruktur dasar (API Client, Database Helper, Theme).
- **Data Layer**: Implementasi `Repository` dan `Datasource` untuk akses data remote (API) dan lokal (SQLite).
- **State Management (Cubit)**: Menggunakan pola state-driven UI (`StockLoading`, `StockError`, `StockCheckInSuccess`) yang memastikan antarmuka pengguna selalu mencerminkan status proses asinkron.

---

## Strategi Offline-First & Sinkronisasi
Mengingat salesman sering bekerja di area dengan sinyal lemah:
- **SQLite (sqflite)**: Digunakan untuk menyimpan cache data toko, produk, dan transaksi yang belum terkirim.
- **Sync Logic (ID Mapping)**: Menggunakan pemetaan ID sementara (`OFFLINE-ID`) ke ID permanen dari server saat proses sinkronisasi untuk menjaga relasi antara Kunjungan dan Restock tetap utuh meskipun dibuat saat offline.
- **Background Queue**: Antrean sinkronisasi berjalan otomatis saat aplikasi mendeteksi pemulihan koneksi internet.

---

## Integrasi Hardware

### 1. Bluetooth Thermal Printer
- **Library**: `blue_thermal_printer`.
- **Layanan**: `ThermalPrintService`.
- **Fungsi**: Mencetak struk penjualan (invoice) secara instan setelah transaksi selesai. Mendukung format teks dinamis dan logo PT BJM.

### 2. Location & Geo-fencing
- **Library**: `geolocator`.
- **Akurasi**: Menggunakan `LocationAccuracy.high` untuk mendapatkan koordinat presisi tinggi.
- **Validasi**: Mengecek status GPS (aktif/mati) dan mendeteksi penggunaan lokasi palsu.

### 3. QR Scanner
- **Library**: `mobile_scanner`.
- **Fungsi**: Membaca QR Code terenkripsi yang ditempel di setiap toko untuk menginisiasi proses check-in.

---

## State Management (BLoC/Cubit Example)
Aplikasi menggunakan pola **Event -> State**:
- **Loading State**: Menampilkan shimmer effect atau loading spinner.
- **Success State**: Menampilkan data yang berhasil diambil.
- **Error State**: Menampilkan pesan error yang ramah pengguna dan opsi untuk mencoba lagi.

---

## Pembaruan Aplikasi (OTA Update)
Aplikasi memiliki mekanisme pembaruan mandiri:
1. Saat startup, aplikasi memanggil `GET /version.json`.
2. Jika `buildNumber` di server > `buildNumber` lokal, muncul update overlay.
3. Aplikasi mengunduh APK baru menggunakan `ota_update` dan mengarahkan pengguna ke proses instalasi.
