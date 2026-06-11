# Berlimdo Enterprise Ecosystem - Knowledge Base

Selamat datang di pusat dokumentasi teknis sistem manajemen stok PT Berlimdo Jaya Makmur (BJM). Dokumen ini dirancang sebagai panduan otoritatif bagi pengembang, arsitek sistem, dan kecerdasan buatan (AI) untuk memahami, memelihara, dan mengembangkan ekosistem Berlimdo.

## Visi Sistem
Sistem Berlimdo bukan sekadar aplikasi pencatatan stok, melainkan platform **Integritas Distribusi** yang mengintegrasikan validasi lokasi tingkat tinggi, manajemen inventaris real-time, dan sistem motivasi tenaga kerja (gamifikasi) untuk memastikan efisiensi operasional di ribuan titik distribusi.

## Peta Dokumentasi

### 1. [Arsitektur & Alur Data](architecture.md)
*Deep dive* ke dalam topologi sistem, interaksi antar komponen (Backend, Web, Mobile), dan bagaimana data mengalir dari lapangan hingga ke laporan eksekutif.

### 2. [Deep Dive Database](database_deep_dive.md)
Analisis struktur data yang kompleks, mencakup skema relasional Prisma, manajemen state geografis, dan strategi integritas transaksi restock.

### 3. [Spesifikasi API (RESTful Spec)](api_spec.md)
Katalog lengkap endpoint API, protokol komunikasi, skema request/response, dan mekanisme autentikasi berlapis.

### 4. [Logika Bisnis & Anti-Fraud](business_logic.md)
Dokumentasi algoritma kritis: Haversine Geo-fencing, Device Binding, Fraud Detection, kalkulasi XP/Level, dan aturan manajemen stok (SMA, BP, OOS).

### 5. [Teknis Aplikasi Mobile (Flutter)](mobile_technical.md)
Bedah arsitektur aplikasi Android: State Management (BLoC), sinkronisasi offline-first, integrasi hardware (Thermal Printer, Camera, GPS), dan Update OTA.

### 6. [DevOps, Security & Deployment](devops_security.md)
Panduan operasional infrastruktur VPS: Konfigurasi Docker vs PM2, manajemen SSL/TLS, strategi backup, dan otomatisasi deployment menggunakan Python/Bash.

### 7. [Audit Kelemahan & Bug](vulnerability_report.md)
Laporan mendalam mengenai celah keamanan (Security), bug nyata (Real Bugs), dan potensi titik kegagalan (Potential Failures) pada seluruh platform.

### 8. [Revisi Fitur & Kebutuhan Client](revisi_requirements.md)
Dokumentasi terstruktur mengenai seluruh permintaan revisi fitur baru, perubahan alur, dan bug report langsung dari client PT BMJ.

### 9. [Analisis Final & Dampak Teknis](final_analysis_revisi.md)
Analisis mendalam tentang dampak teknis, korelasi bug, dan roadmap prioritas implementasi revisi.

### 10. [Data Master & Utilitas (Scratch)](misc.md)
Informasi mengenai pengolahan data awal wilayah, catatan revisi historis, dan skrip pembantu (helper scripts).

---

## Tech Stack Overview

| Komponen | Teknologi | Peran Utama |
| :--- | :--- | :--- |
| **Backend** | Node.js, Express, TypeScript | Business Logic & API Gateway |
| **Database** | MySQL, Prisma ORM | Persistent Data Storage |
| **Web Admin** | Next.js 14 (App Router), Tailwind | Analytics & Management Dashboard |
| **Mobile App** | Flutter (Dart), BLoC | Field Operation Tool |
| **Infrastructure** | VPS (Linux), Docker, PM2, Nginx | Hosting & Process Management |

---
*Dokumentasi ini bersifat dinamis dan mencerminkan status pengembangan terbaru per Juni 2026.*
