# 🛡️ DevOps, Security & Deployment (Operational Manual)

Ekosistem Berlimdo dikelola dengan strategi hibrida antara kontainerisasi (Docker) dan manajemen proses native (PM2) untuk memaksimalkan performa dan kemudahan pemeliharaan.

## 🚀 Strategi Deployment

### 1. Kontainerisasi (Docker)
Digunakan terutama untuk Database dan environment isolasi.
- **File**: `vps-deploy/docker-compose.yml`.
- **Layanan**:
    - `db`: MySQL 8.0 dengan volume persisten.
    - `backend`: Node.js environment.
    - `web`: Next.js production build.

### 2. Process Management (PM2)
Digunakan pada server produksi untuk aplikasi Node.js guna mendapatkan monitoring real-time dan fitur *auto-restart* yang lebih ringan.
- **Konfigurasi**: `ecosystem.config.js`.
- **Perintah Utama**: `pm2 start ecosystem.config.js`.

### 3. Otomatisasi Skrip (Python & Bash)
Berbagai skrip dikembangkan untuk mengotomatiskan tugas-tugas kritis:
- `vps_direct_deploy.sh`: Sinkronisasi kode menggunakan `rsync` dan build otomatis di server.
- `master_seed_deploy.py`: Melakukan *force reset* skema database dan injeksi data master wilayah secara otomatis.
- `robust_deploy.py`: Skrip deployment dengan sistem *heartbeat* untuk memantau build yang lama (Next.js build).

---

## 🔒 Protokol Keamanan

### 1. Network Security
- **SSL/TLS**: Semua komunikasi API dienkripsi melalui HTTPS (Nginx sebagai Reverse Proxy).
- **Rate Limiting**: Diterapkan pada level API untuk mencegah serangan Brute Force, terutama pada endpoint `/login`.
- **Troubleshooting Logs**:
    - `Unique Constraint Error (P2002)`: Terdeteksi pada log backend, menandakan upaya double-submit pada laporan restock.
    - `Next.js Server Action Error`: Terjadi saat transisi build di frontend, perlu pembersihan cache browser atau restart PM2.
    - `Insufficient Stock Log`: Mencatat kegagalan transaksi di level logika bisnis saat stok salesman habis.

### 2. Aplikasi & Data Security
- **Device Binding**: Setiap akun Salesman dikunci ke satu `AndroidID`. Reset hanya bisa dilakukan oleh Super Admin.
- **JWT Authentication**: Token dengan masa berlaku terbatas dan mekanisme Refresh Token.
- **Anti-Fraud GPS**: Validasi berlapis:
    1. Deteksi `isMockLocation` di aplikasi mobile.
    2. Perhitungan ulang jarak (Haversine) di sisi server (Server-Side Validation).

### 3. SSH & Access Control
- Akses ke VPS diamankan menggunakan kunci privat RSA ([nagha.pem](file:///home/firman/Downloads/pakwahyu/nagha.pem)).
- Penggunaan port non-standar dan firewall (UFW) untuk membatasi akses ke database dari luar.

---

## 📲 Distribusi APK & OTA (Over-The-Air)
Sistem pembaruan aplikasi mobile dilakukan tanpa melalui Play Store:
1. APK di-build secara lokal.
2. Skrip `deploy_latest.py` mengunggah APK ke folder publik web server (`/var/www/html/`).
3. File `version.json` diperbarui dengan nomor build terbaru.
4. Aplikasi mobile mendeteksi perubahan versi, mengunduh APK baru, dan meminta pengguna melakukan instalasi (Update Overlay).
