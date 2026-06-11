# 🔌 Spesifikasi API (RESTful API Documentation)

Backend Berlimdo menyediakan API berbasis REST yang menggunakan JSON sebagai format pertukaran data. Semua permintaan ke endpoint yang dilindungi harus menyertakan `Authorization: Bearer <JWT_TOKEN>`.

## 📍 Base URL
- **Production**: `https://api.berlimdo.com/api/v1`
- **Development**: `http://76.13.22.232:3000/api/v1`

---

## 🔑 1. Modul Autentikasi (`/auth`)

### POST `/login`
Melakukan autentikasi pengguna dan mengikat perangkat (Device Binding).
- **Request Body**:
  ```json
  {
    "username": "NIK_SALESMAN",
    "password": "PASSWORD",
    "deviceId": "UNIQUE_ANDROID_ID"
  }
  ```
- **Response (200 OK)**:
  ```json
  {
    "token": "JWT_ACCESS_TOKEN",
    "refreshToken": "JWT_REFRESH_TOKEN",
    "user": { "id": "UUID", "name": "John Doe", "role": "STAFF" }
  }
  ```

---

## 🏪 2. Modul Toko & Kunjungan (`/shops`, `/visits`)

### GET `/shops`
Mengambil daftar toko terdekat atau berdasarkan wilayah.
- **Query Params**: `lat`, `long`, `radius`, `districtId`.

### POST `/visits/check-in`
Membuka sesi kunjungan dengan validasi geo-fencing.
- **Request Body**:
  ```json
  {
    "shopId": "UUID",
    "lat": -2.12345,
    "long": 106.12345,
    "photo": "BASE64_OR_URL"
  }
  ```
- **Business Logic**: Server menghitung jarak. Jika > `radiusMeter` (default 20m), permintaan ditolak (403).

---

## 📦 3. Modul Restock & Inventaris (`/restock`, `/inventory`)

### POST `/restock`
Mencatat transaksi penjualan dan memperbarui stok.
- **Request Body**:
  ```json
  {
    "visitId": "UUID",
    "items": [
      {
        "productId": "UUID",
        "qty": 10,
        "type": "SALE"
      }
    ],
    "paymentType": "CASH"
  }
  ```

### GET `/inventory/me`
Mengambil data stok yang saat ini dibawa oleh salesman (Seller Inventory).

---

## 📊 4. Modul Dashboard & Laporan (`/dashboard`, `/reports`)

### GET `/dashboard/analytics`
Mengambil agregasi data penjualan untuk grafik dashboard (Hanya Admin).

### GET `/reports/export-excel`
Menghasilkan file Excel untuk laporan harian/bulanan.

---

## 🌍 5. Modul Geodata (`/geodata`)
Digunakan untuk sinkronisasi data master wilayah (Provinsi -> Kabupaten -> Kecamatan -> Desa).

---

## ⚠️ Penanganan Error (Standardized Error Response)
Semua error mengikuti format berikut:
```json
{
  "status": "error",
  "code": "ERROR_CODE_NAME",
  "message": "Human readable message",
  "details": {} 
}
```
*Kode Error Umum: `UNAUTHORIZED`, `FORBIDDEN_LOCATION`, `INSUFFICIENT_STOCK`, `DEVICE_MISMATCH`.*
