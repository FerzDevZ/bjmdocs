# Backend Documentation (nagha-stock-backend)

## Teknologi
- **Runtime**: Node.js
- **Framework**: Express.js
- **ORM**: Prisma
- **Database**: MySQL
- **Bahasa**: TypeScript

## Struktur Folder Utama
- `src/main.ts`: Entry point aplikasi dan konfigurasi middleware (CORS, Helmet, Rate Limit).
- `src/modules/`: Berisi modul-modul logika bisnis:
    - `auth/`: Autentikasi JWT dan login limiter.
    - `shops/`: Manajemen data toko dan status persetujuan.
    - `visits/`: Log kunjungan salesman dengan validasi GPS.
    - `restock/`: Logika transaksi penjualan dan pembaruan stok.
    - `inventory/`: Manajemen stok yang dibawa oleh salesman.
    - `geodata/`: Manajemen data wilayah (Regency, District, Village).
- `prisma/schema.prisma`: Definisi skema database dan relasi antar tabel.
- `uploads/`: Direktori penyimpanan file (foto kunjungan, APK aplikasi).

## Fitur Teknis
- **Keamanan**: Implementasi Helmet, Rate Limiting, CORS, dan hashing password dengan bcrypt.
- **Audit**: Log keamanan (`SecurityLog`) untuk mencatat aktivitas mencurigakan.
- **Geo-fencing**: Validasi jarak kunjungan berdasarkan koordinat GPS toko dan salesman.
- **Background Jobs**: Skrip untuk audit excel, pembersihan database, dan impor data geografis.

## API Endpoints Utama
- `/api/v1/auth`: Login, Refresh Token.
- `/api/v1/shops`: CRUD Toko, verifikasi QR.
- `/api/v1/visits`: Check-in, Check-out, Fraud detection.
- `/api/v1/restock`: Transaksi restock produk ke toko.
- `/api/v1/master`: Data master produk, brand, kategori, dan unit.
