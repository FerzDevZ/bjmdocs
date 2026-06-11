# Frontend Web Documentation (nagha-stock-web)

## Teknologi
- **Framework**: Next.js 14 (App Router)
- **Styling**: Tailwind CSS
- **Icons**: Lucide React
- **Maps**: Leaflet & React Leaflet
- **Charts**: Chart.js & Recharts
- **Export**: xlsx (Excel), jsPDF (PDF)

## Struktur Folder Utama
- `src/app/`: Menggunakan Next.js App Router:
    - `analytics/`: Dashboard untuk CRM, heatmap kunjungan, dan performa penjualan.
    - `audit/`: Halaman pemantauan kepatuhan salesman.
    - `live-map/`: Peta real-time untuk memantau posisi kunjungan terakhir.
    - `master/`: Manajemen data master wilayah dan produk.
    - `users/`: Manajemen pengguna dan laporan performa individu.
    - `shops/`: Daftar toko dan manajemen status.
- `src/components/`: Komponen UI yang dapat digunakan kembali (AuthGuard, Maps, Filters).

## Fitur Utama
- **Monitoring Real-time**: Memantau aktivitas salesman di lapangan melalui peta.
- **Analitik Penjualan**: Visualisasi data penjualan dalam bentuk grafik dan tabel.
- **Manajemen Data Master**: Memungkinkan admin mengelola produk, harga, dan struktur wilayah.
- **Ekspor Laporan**: Mengunduh data dalam format Excel atau PDF untuk keperluan audit.
- **Sistem Reward**: Mengatur konfigurasi poin dan melihat leaderboard salesman.
