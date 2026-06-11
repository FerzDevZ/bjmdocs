# Deployment Documentation (Nagha Stock / Berlimdo)

## Infrastruktur VPS
Sistem dideploy pada VPS dengan detail sebagai berikut:
- **IP Publik**: `76.13.22.232` (Digunakan untuk akses API dan Web)
- **IP Management**: `100.74.69.65` (Akses internal/Tailscale)
- **User**: `root`
- **Password**: `BJM@2026sukses`
- **SSH Key**: `nagha.pem` (Digunakan pada skrip deployment terbaru)

## Metode Deployment
Terdapat beberapa evolusi metode deployment yang tercermin dalam skrip:

### 1. Docker Compose (Containerized)
- **Konfigurasi**: Terletak di folder `vps-deploy/docker-compose.yml`.
- **Skrip Terkait**: `deploy_full.sh`, `robust_deploy.py`, `deploy_vps.sh`.
- **Fungsi**: Menjalankan database (MySQL), backend, dan frontend dalam kontainer terpisah.
- **Cara Kerja**: Mengunggah file `.tar.gz`, mengekstrak di VPS, lalu menjalankan `docker compose up --build`.

### 2. PM2 (Industrialized/Production)
- **Skrip Terkait**: `vps_direct_deploy.sh`, `final_prod_deploy.py`.
- **Fungsi**: Menjalankan aplikasi Node.js (Backend & Frontend) langsung di host menggunakan PM2 untuk stabilitas yang lebih baik, sementara Database tetap di Docker.
- **Cara Kerja**: Menggunakan `rsync` untuk sinkronisasi kode, `npm run build` di sisi server, lalu `pm2 reload ecosystem.config.js`.

### 3. Update Aplikasi Mobile (OTA - Over The Air)
- **Skrip Terkait**: `vps_revisi_deploy.sh`, `deploy_latest.py`, `move_apk.py`.
- **Fungsi**: Mendistribusikan pembaruan APK kepada salesman.
- **Lokasi Distribusi**: `/var/www/html/bjm.apk` dan `version.json`.
- **Cara Kerja**: APK hasil build Flutter diunggah ke folder publik web server sehingga aplikasi mobile dapat mendeteksi versi baru dan mengunduhnya secara otomatis.

## Daftar Skrip Penting
- `vps_direct_deploy.sh`: Skrip deployment final paling stabil menggunakan SSH Key.
- `master_seed_deploy.py`: Digunakan untuk reset database dan menjalankan seeding data master.
- `fix_refactor.py`: Skrip utilitas untuk memperbaiki URL API yang hardcoded di frontend.
- `refine_directives.py`: Memperbaiki urutan directive `"use client"` pada komponen Next.js.

## Troubleshooting
- **Database Schema**: Jika kolom hilang, gunakan `npx prisma db push --force-reset` via `master_seed_deploy.py`.
- **CORS/API URL**: Pastikan `NEXT_PUBLIC_API_URL` diatur ke `https://api.berlimdo.com/api/v1` saat build frontend.
