# 🧪 Data Master, Analisis & Utilitas Eksperimental

Dokumen ini merinci komponen-komponen pendukung yang digunakan selama fase pengembangan dan perbaikan data sistem Berlimdo.

## 📊 1. Pengolahan Data Wilayah (`scratch/`)

Folder ini merupakan laboratorium data untuk memastikan validitas database geografis.
- **Workflow Ekstraksi**:
    1. Membaca file master [Area Bangka Dan Belitung.xlsx](file:///home/firman/Downloads/pakwahyu/perbaikan/Area Bangka Dan Belitung.xlsx) menggunakan `read_area.py`.
    2. Parsing struktur kolom untuk memisahkan data Kabupaten, Kecamatan, dan Desa.
    3. Konversi ke format JSON menggunakan `excel_to_json.ts`.
    4. Injeksi data ke MySQL menggunakan skrip `import_geo_data.ts` yang ada di backend.
- **Output**: File `geo_data.json` yang menjadi dasar dari semua validasi alamat toko di sistem.

---

## 📝 2. Log Revisi & Feedback Stakeholder (`perbaikan/`)

Dokumen di folder ini mencatat evolusi kebutuhan bisnis:
- **`ferdinand 3 bini.txt`**: Dokumen krusial yang berisi permintaan optimasi sistem:
    - **UX Refactoring**: Perubahan terminologi menu (misal: "Stok Bawaan Sales" menjadi "Input Stok Sales").
    - **Storage Optimization**: Keputusan untuk menonaktifkan wajib foto pada beberapa jenis input guna menghemat ruang penyimpanan server (mitigasi "JEBOL Storage").
    - **Report Accuracy**: Perbaikan logika laporan SMA (Stok Masa Lalu) yang sebelumnya tidak muncul di nota/laporan excel.
- **`Area Bangka Dan Belitung.xlsx`**: Sumber data primer wilayah operasional.

---

## 🖼️ 3. Dokumentasi Visual Bug & UI (`RevisianBaru/`, dll)

Berbagai folder gambar (`image.png`, `image copy.png`) dikategorikan berdasarkan fase revisi:
- **Fungsi**: Digunakan sebagai bukti visual (Proof of Concept) saat melakukan diskusi teknis mengenai perbaikan layout pada dashboard admin atau alur navigasi di aplikasi Flutter.
- **Pentingnya**: Membantu tim pengembang memahami konteks error yang terjadi di lapangan (seperti error GPS atau masalah tampilan pada perangkat Android tertentu).

---

## 📦 4. Infrastruktur Kontainer (`vps-deploy/`)

Meskipun sistem produksi dapat menggunakan PM2, folder ini menyediakan konfigurasi **High-Availability (HA)** dasar:
- **`docker-compose.yml`**: Mendefinisikan jaringan internal (`nagha-network`) yang mengisolasi database agar tidak bisa diakses langsung dari internet, meningkatkan keamanan data perusahaan.

---

## 🛠️ 5. Skrip Utilitas Root (Helper Scripts)

| Skrip | Fungsi Kompleks |
| :--- | :--- |
| `fix_refactor.py` | Menggunakan **Regular Expressions (Regex)** untuk melakukan penggantian URL API secara massal di ribuan baris kode frontend Next.js. |
| `refine_directives.py` | Memastikan urutan directive `"use client"` di Next.js tetap konsisten untuk mencegah error SSR (Server Side Rendering). |
| `master_seed_deploy.py` | Skrip otomatisasi tingkat lanjut yang menggabungkan SSH command, Docker execution, dan Prisma CLI untuk sinkronisasi database dalam satu klik. |
