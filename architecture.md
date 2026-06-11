# 🏗️ Arsitektur & Alur Data (System Architecture)

Sistem Berlimdo mengadopsi arsitektur **Distributed Client-Server** yang dirancang untuk skalabilitas dan keandalan di lingkungan dengan konektivitas yang bervariasi.

## 📡 Topologi Ekosistem

Sistem ini terbagi menjadi tiga lapisan utama yang saling berinteraksi secara asinkron dan sinkron:

### 1. Presentation Layer (Mobile & Web)
- **Field Client (Mobile)**: Bertindak sebagai terminal input data utama. Mengelola state lokal (SQLite) untuk mendukung operasional offline dan melakukan sinkronisasi saat koneksi tersedia.
- **Admin Dashboard (Web)**: Terminal visualisasi dan manajemen. Berinteraksi dengan backend untuk agregasi data real-time dan manajemen data master.

### 2. Logic & Gateway Layer (Backend API)
- **API Gateway**: Menangani routing, autentikasi JWT, dan rate limiting.
- **Business Logic Modules**: Komponen modular yang menangani validasi geo-fencing, kalkulasi harga produk (Grosir vs Retail), dan logika reward/XP.
- **Middleware**: Lapangan keamanan (Helmet, CORS) dan penanganan error terpusat.

### 3. Data Layer (Persistence)
- **MySQL Database**: Sumber kebenaran (Source of Truth) untuk semua data transaksional dan master.
- **Prisma ORM**: Lapisan abstraksi database yang memastikan konsistensi tipe data antara database dan aplikasi (TypeScript).
- **File Storage (Uploads)**: Penyimpanan bukti foto kunjungan dan distribusi file APK.

---

## 🔄 Alur Data Transaksional (Transaction Flow)

### Skenario: Kunjungan & Penjualan (Restock)
1. **Inisiasi (Mobile)**: Salesman memilih toko. Aplikasi mengambil koordinat GPS terkini.
2. **Validasi (Backend)**: Aplikasi mengirim `ShopID` dan `GPS`. Backend menghitung jarak menggunakan **Formula Haversine**. Jika jarak < 20m, sesi kunjungan dibuka.
3. **Input Data (Mobile)**: Salesman memasukkan data stok (Lama/Baru) dan jumlah penjualan.
4. **Persistensi (Backend)**: Data dikirim ke `/api/v1/restock`. Backend melakukan:
   - Pengurangan stok di `SellerInventory`.
   - Penambahan log di `InventoryMovement`.
   - Pembuatan `RestockReport` dan `RestockItem`.
   - Penambahan XP ke `User`.
5. **Agregasi (Web)**: Admin Dashboard segera memperbarui grafik penjualan dan heatmap kunjungan berdasarkan data terbaru dari database.

---

## 🔐 Keamanan & Integritas Data
- **JWT (JSON Web Token)**: Digunakan untuk setiap permintaan API setelah login sukses.
- **Device Binding**: Backend mencatat `deviceId` pada login pertama untuk mencegah satu akun digunakan di banyak perangkat tanpa izin.
- **Fraud Detection Logic**: Setiap kunjungan diverifikasi ulang di sisi server untuk memastikan koordinat tidak dimanipulasi oleh aplikasi *Fake GPS*.

---

## 📈 Skalabilitas
Sistem dirancang untuk dapat dipisahkan menjadi microservices di masa depan jika beban kerja meningkat, terutama modul `Analytics` yang berat dan `Transaction` yang intensif.
