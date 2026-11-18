# 📷 Lensa Studio - Sistem Informasi Manajemen Fotostudio Berbasis Web

[![PHP CI](https://github.com/user/Lensa-Studio/actions/workflows/ci.yml/badge.svg)](.github/workflows/ci.yml)
[![PHP Version](https://img.shields.io/badge/PHP-8.2%20%7C%208.3%20%7C%208.4-777BB4?logo=php&logoColor=white)](https://www.php.net)
[![Database](https://img.shields.io/badge/MySQL-8.0%2B%20%2F%20MariaDB-4479A1?logo=mysql&logoColor=white)](https://www.mysql.com)
[![Architecture](https://img.shields.io/badge/Architecture-Custom%20MVC%20(Pure%20PHP)-orange)](https://github.com)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

### 👥 KELOMPOK 2:
1. **Rizka Dwi Zaliyanti** (`F5512510005`)
2. **Muhammad Fariid Faqih** (`F5512510007`)
3. **Kasih Nurlianita** (`F5512510032`)

---

**Lensa Studio** adalah platform sistem informasi manajemen operasional fotostudio modern berbasis web yang dibangun menggunakan arsitektur **Pure PHP (MVC)** dan **MySQL/MariaDB**. Aplikasi ini dirancang untuk mempermudah alur kerja studio foto, mulai dari pemesanan jadwal pemotretan oleh pelanggan secara real-time, verifikasi pembayaran bertahap (DP & Pelunasan), penugasan fotografer, pengunggahan serta pengelolaan galeri privat beresolusi tinggi, hingga laporan keuangan dan audit aktivitas admin.

---

## 📑 Daftar Isi

- [Fitur Utama](#-fitur-utama)
  - [1. Pelanggan (Customer)](#1-pelanggan-customer)
  - [2. Administrator](#2-administrator)
  - [3. Fotografer](#3-fotografer)
- [Arsitektur & Keunggulan Teknis](#-arsitektur--keunggulan-teknis)
  - [Pencegahan Double-Booking & Serialisasi Transaksi](#pencegahan-double-booking--serialisasi-transaksi)
  - [Keamanan Penyimpanan Berkas Privat](#keamanan-penyimpanan-berkas-privat)
  - [Lapisan Keamanan Aplikasi](#lapisan-keamanan-aplikasi)
- [Struktur Direktori](#-struktur-direktori)
- [Kebutuhan Sistem](#-kebutuhan-sistem)
- [Panduan Instalasi](#-panduan-instalasi)
  - [Langkah 1: Kloning / Letakkan Repositori](#langkah-1-kloning--letakkan-repositori)
  - [Langkah 2: Konfigurasi Environment (.env)](#langkah-2-konfigurasi-environment-env)
  - [Langkah 3: Migrasi & Seed Basis Data](#langkah-3-migrasi--seed-basis-data)
  - [Langkah 4: Menjalankan Server Web](#langkah-4-menjalankan-server-web)
- [Akun Demo Bawaan](#-akun-demo-bawaan)
- [Pengujian Otomatis (Automated Testing)](#-pengujian-otomatis-automated-testing)
- [Integrasi CI/CD](#-integrasi-cicd)
- [Lisensi](#-lisensi)

---

## ✨ Fitur Utama

Sistem ini menerapkan **Role-Based Access Control (RBAC)** dengan tiga peran pengguna utama:

### 1. Pelanggan (Customer)
* **Katalog Layanan & Paket:** Menjelajahi berbagai paket foto tematik (Studio Signature, Keluarga, Wisuda, Produk) beserta rincian fasilitas dan durasi.
* **Pengecekan Ketersediaan Real-Time:** Memeriksa ketersediaan jam sesi studio (rentang 09.00 – 18.00) secara instan tanpa tumpang tindih waktu.
* **Reservasi Online Fleksibel:** Mengajukan booking dengan sistem batas waktu konfirmasi otomatis (*hold expiration*).
* **Pembayaran Bertahap:** 
  * Mendukung skema **Uang Muka (DP minimal 30%)** atau pembayaran penuh.
  * Form unggah bukti transfer manual dengan validasi berkas ketat.
* **Pelacakan Status Reservasi:** Transparan memantau status pemesanan (*Menunggu Pembayaran*, *Menunggu Verifikasi*, *Dikonfirmasi*, *Sedang Foto*, *Selesai*, *Dibatalkan*).
* **Galeri Digital Privat:** Mengakses dan mengunduh foto digital beresolusi tinggi hasil pemotretan khusus reservasi milik pelanggan bersangkutan.
* **Kelola Profil:** Pembaruan data diri dan penggantian kata sandi berotentikasi.

### 2. Administrator
* **Dashboard Metrik & Analitik:** Ringkasan statistik omzet pendapatan, reservasi aktif, verifikasi pending, dan jadwal hari ini.
* **Manajemen Master Data (Katalog):**
  * CRUD Layanan satuan (misal: Sesi Studio, Retouch Digital, Cetak Foto, Backdrop).
  * CRUD Paket Foto dengan sistem relasi many-to-many ke layanan satuan (*composite key*).
* **Manajemen & Penjadwalan Reservasi:**
  * Meninjau detail reservasi masuk.
  * Mengonfirmasi atau menolak pengajuan jadwal.
  * Penugasan fotografer resmi ke reservasi dengan deteksi bentrok jadwal otomatis.
* **Verifikasi Pembayaran:** Meninjau gambar bukti transfer, menyetujui atau menolak dengan catatan alasan verifikasi.
* **Manajemen Akun Pengguna:** Pengelolaan akun pelanggan, fotografer, dan administrator (aktivasi/non-aktivasi akun).
* **Pengaturan Akses Galeri:** Mengatur status aksesibilitas foto (Publik untuk portofolio studio atau Privat hanya untuk pelanggan terkait).
* **Audit Trail (Log Aktivitas):** Pencatatan riwayat setiap aksi krusial sistem dengan rekaman rincian JSON (`log_aktivitas`).
* **Laporan Keuangan & Operasional:** Rekapitulasi laporan pemesanan dan transaksi keuangan berfilter periode.

### 3. Fotografer
* **Jadwal Kerja Personal:** Tinjauan daftar reservasi dan penugasan sesi foto yang ditugaskan kepada fotografer bersangkutan.
* **Pembaruan Status Sesi:** Mengubah status pemotretan secara langsung di studio (*sedang pemotretan* hingga *sesi selesai*).
* **Unggah Hasil Foto:** Mengunggah file foto beresolusi tinggi (JPG, PNG, WebP) ke galeri reservasi pelanggan dengan batasan ukuran yang terkontrol.

---

## 🛡️ Arsitektur & Keunggulan Teknis

Aplikasi dibangun mengutamakan performa tinggi tanpa dependensi vendor eksternal yang membengkak (*zero external vendor bloat*), aman, dan mudah dipelihara.

```
       Browser (Client)
              │
       [public/index.php] (Front Controller & Routing)
              │
    ┌─────────┴─────────┐
    ▼                   ▼
Controllers        Middlewares / Helpers
    │             (CSRF, Auth & RBAC Check)
    ▼
Services (Business Logic & Validation)
 ├── BookingService   ──> Mutex Locking & Conflict Checking
 ├── PaymentService   ──> DP & Payment Proof Workflow
 ├── UploadService    ──> MIME & Image Verification
 └── AuthService      ──> Bcrypt & Session Regeneration
    │
    ▼
Repositories (PDO Prepared Statements)
    │
    ▼
MySQL Database (InnoDB ACID Transactions & studio_mutex)
```

### Pencegahan Double-Booking & Serialisasi Transaksi
Untuk mencegah kondisi persaingan (*race condition*) ketika dua pengguna memesan slot waktu yang beririsan secara bersamaan:
1. Memanfaatkan tabel kunci **`studio_mutex`** dengan kueri `SELECT ... FOR UPDATE` dalam transaksi ACID InnoDB.
2. Setiap pengajuan divalidasi dengan logika `BookingRules::overlaps` yang memperhitungkan jam mulai dan durasi menit paket, baik untuk ketersediaan ruang studio maupun jadwal fotografer.

### Keamanan Penyimpanan Berkas Privat
* Direktori `storage/uploads/` (berisi bukti transfer dan hasil foto privat) dilindungi file `.htaccess` dengan aturan `Require all denied`.
* Berkas **tidak dapat diakses langsung melalui URL browser publik**.
* Akses unduhan foto (`index.php?r=download`) dan bukti bayar (`index.php?r=proof`) wajib melewati *controller terotorisasi* yang memverifikasi kepemilikan akun atau peran pengguna sebelum melakukan *streaming* biner berkas.
* Nama berkas asli dienkripsi secara acak menggunakan *cryptographic random token* (`bin2hex(random_bytes(20))`) untuk mencegah *file enumeration*.

### Lapisan Keamanan Aplikasi
* **Anti-SQL Injection:** 100% menggunakan *PDO Prepared Statements* dengan *parameterized queries*.
* **Anti-CSRF:** Setiap formulir POST dilindungi token `_csrf` yang divalidasi menggunakan `hash_equals()`.
* **Proteksi XSS:** Seluruh output dinamis disanitasi menggunakan fungsi `e()` (`htmlspecialchars` dengan flag `ENT_QUOTES | ENT_SUBSTITUTE`).
* **Keamanan Sesi:** Cookie sesi dikonfigurasi dengan flag `HttpOnly`, `SameSite=Lax`, dan otomatis `Secure` saat menggunakan HTTPS, disertai `session_regenerate_id(true)` saat proses masuk akun.
* **HTTP Security Headers:** Header `X-Frame-Options: SAMEORIGIN`, `X-Content-Type-Options: nosniff`, dan `Referrer-Policy: strict-origin-when-cross-origin` diterapkan pada web server.

---

## 📁 Struktur Direktori

```text
Lensa-Studio/
├── .github/
│   └── workflows/
│       └── ci.yml             # Konfigurasi pengujian CI otomatis GitHub Actions
├── app/
│   ├── Core/
│   │   └── Database.php       # Singleton koneksi database PDO
│   ├── Support/
│   │   ├── BookingRules.php   # Aturan validasi waktu sesi & rumus pembayaran
│   │   └── helpers.php        # Kumpulan fungsi utilitas (e, csrf, session, auth)
│   ├── views/                 # Template antarmuka pengguna (View)
│   │   ├── admin/             # Tampilan panel kendali administrator
│   │   ├── auth/              # Halaman login, register, profil
│   │   ├── customer/          # Tampilan reservasi, pembayaran & galeri pelanggan
│   │   ├── errors/            # Halaman kesalahan kustom (403, 404)
│   │   ├── partials/          # Header, footer, navigasi, dan flash alert
│   │   ├── photographer/      # Panel kerja fotografer
│   │   ├── home.php           # Landing page beranda
│   │   └── layout.php         # Master layout HTML5
│   ├── bootstrap.php          # Inisialisasi dependensi aplikasi & session
│   ├── Controllers.php        # Kumpulan class Controller aplikasi
│   ├── Repositories.php       # Abstraksi akses data basis data (Model)
│   └── Services.php           # Logika bisnis (Booking, Payment, Upload, Auth)
├── config/
│   └── config.php             # Pemuat konfigurasi dari file .env
├── database/
│   └── schema.sql             # Skema DDL tabel & data benih (seed) demo
├── public/                    # Direktori web publik (Web Root)
│   ├── assets/                # Aset statis frontend (CSS, JS, gambar/vektor)
│   ├── .htaccess              # Header keamanan & aturan direktori
│   └── index.php              # Front controller & router rute web
├── storage/
│   ├── uploads/
│   │   ├── gallery/           # Penyimpanan berkas foto resolusi tinggi
│   │   └── payments/          # Penyimpanan gambar bukti transfer
│   └── .htaccess              # Blokir akses HTTP langsung ke direktori storage
├── tests/
│   └── run_checks.php         # Script pengujian unit & integrasi mandiri
├── .env.example               # Contoh template variabel lingkungan
├── .gitignore                 # Berkas pengabaian Git
└── README.md                  # Dokumentasi proyek
```

---

## 💻 Kebutuhan Sistem

Sebelum menjalankan aplikasi, pastikan perangkat Anda memenuhi syarat berikut:

| Komponen | Spesifikasi Minimum | Rekomendasi |
| :--- | :--- | :--- |
| **PHP** | 8.2 | **PHP 8.4** |
| **Ekstensi PHP** | `pdo_mysql`, `fileinfo`, `mbstring` | Aktif secara default di Laragon/XAMPP |
| **Database Server**| MySQL 8.0+ / MariaDB 10.4+ | MySQL 8.0+ (InnoDB Engine) |
| **Web Server** | Apache 2.4+ (`mod_rewrite` & `mod_headers`) | Laragon / Apache Server |
| **Peramban Web** | Chrome, Firefox, Edge, Safari terbaru | Versi modern dengan dukungan ES6 & CSS Grid |

---

## 🚀 Panduan Instalasi

### Langkah 1: Kloning / Letakkan Repositori
Tempatkan folder proyek ke dalam direktori web server lokal Anda:
* **Laragon:** `C:\laragon\www\Lensa-Studio`
* **XAMPP:** `C:\xampp\htdocs\Lensa-Studio`

### Langkah 2: Konfigurasi Environment (`.env`)
Salin file `.env.example` menjadi `.env`:
```bash
git clone https://github.com/farridfaqih-beep/lensa-studio-management-system.git
# Di PowerShell atau Command Prompt
copy .env.example .env
```

Buka file `.env` dan sesuaikan parameter koneksi basis data Anda:
```env
APP_NAME="Lensa Studio"
APP_ENV=local
APP_DEBUG=true
APP_TIMEZONE=Asia/Makassar

DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=fotostudio
DB_USER=root
DB_PASS=

UPLOAD_MAX_MB=12
```

### Langkah 3: Migrasi & Seed Basis Data
Pastikan layanan MySQL di Laragon atau XAMPP sudah berjalan, lalu impor berkas skema basis data:
* **Menggunakan CLI:**
  ```bash
  mysql -u root -p < database/schema.sql
  ```
* **Atau menggunakan HeidiSQL / phpMyAdmin:**
  1. Buka aplikasi phpMyAdmin atau HeidiSQL.
  2. Buat database baru bernama `fotostudio` (opsional, karena di dalam file SQL sudah terdapat sintaks `CREATE DATABASE IF NOT EXISTS fotostudio`).
  3. Buka tab **Import / Execute SQL File**, pilih berkas `database/schema.sql`, lalu jalankan.

### Langkah 4: Menjalankan Server Web

#### Opsi A: Menggunakan Laragon (Sangat Direkomendasikan)
1. Buka aplikasi **Laragon**.
2. Klik tombol **Start All**.
3. Akses sistem melalui peramban pada URL:
   ```text
   http://localhost/Lensa-Studio/public/
   # atau jika menggunakan virtual host otomatis Laragon:
   http://lensa-studio.test/public/
   ```

#### Opsi B: Menggunakan PHP Built-in Server
Anda juga dapat menjalankan aplikasi secara instan tanpa web server eksternal:
```bash
# Jalankan langsung dari folder root proyek dengan mengarahkan docroot ke public
php -S 127.0.0.1:8000 -t public
```
Akses di peramban pada: `http://127.0.0.1:8000`

---

## 👥 Akun Demo Bawaan

Basis data telah dilengkapi dengan akun demonstrasi untuk mempermudah evaluasi fitur di setiap peran:

| Peran | Username | Email | Kata Sandi | Deskripsi Akses |
| :--- | :--- | :--- | :--- | :--- |
| **Administrator** | `adminlensa` | `admin@fotostudio.test` | `Admin123!` | Akses penuh ke seluruh menu kelola, verifikasi bayar, laporan, dan master data. |
| **Fotografer** | `argafoto` | `foto@fotostudio.test` | `Foto123!` | Akses jadwal tugas pemotretan dan upload hasil foto sesi pelanggan. |
| **Pelanggan** | `nadiademo` | `pelanggan@fotostudio.test` | `Pelanggan123!` | Akses pemesanan jadwal, upload bukti transfer DP/lunas, dan galeri hasil foto. |

> ⚠️ **Catatan Keamanan:** Akun dan kata sandi di atas ditujukan hanya untuk keperluan pengujian dan demonstrasi lingkungan lokal (*development*). Pastikan mengganti seluruh kredensial sebelum digunakan pada server produksi.

---

## 🧪 Pengujian Otomatis (Automated Testing)

Proyek ini dilengkapi dengan suite pengujian otomatis mandiri yang menguji integritas aturan reservasi, tumpang tindih waktu (*slot overlap*), formula DP, kelengkapan berkas, validitas skema basis data, dan verifikasi hash password demo.

Untuk menjalankan pengujian, jalankan perintah berikut di terminal:
```bash
php tests/run_checks.php
```

Contoh keluaran pengujian yang berhasil:
```text
[LULUS] Slot masa depan dinormalisasi dengan durasi yang benar
[LULUS] Tanggal lampau ditolak
[LULUS] Jam sebelum 09.00 ditolak
[LULUS] Sesi yang selesai setelah 18.00 ditolak
[LULUS] Rentang sesi bertumpang tindih terdeteksi
[LULUS] Sesi berurutan tanpa tumpang tindih diperbolehkan
[LULUS] DP awal minimal 30 persen dihitung benar
[LULUS] Status lunas dihitung dari total pembayaran disetujui
[LULUS] File utama tersedia: public/index.php
[LULUS] Kontrol peran tersedia pada sisi server
[LULUS] Upload dan unduhan melewati layanan penyimpanan privat
[LULUS] Password demo ter-hash valid: admin@fotostudio.test
...
Hasil: 29 lulus, 0 gagal.
```

---

<<<<<<< HEAD
<p align="center">
  <b>Lensa Studio</b>
  <br>
  Photography Studio Management System
  <br><br>
  <sub>Designed to simplify photography studio operations.</sub>
</p>
=======
## 🔄 Integrasi CI/CD

Proyek ini telah dikonfigurasi dengan pipeline **GitHub Actions** (`.github/workflows/ci.yml`) yang berjalan otomatis pada setiap aksi *push* dan *pull request* ke branch `main`, `develop`, atau `fitur/**`.

Pipeline menjalankan tahapan:
1. Validasi sintaks seluruh berkas PHP (`php -l`).
2. Pemeriksaan ketersediaan ekstensi PHP krusial (`fileinfo`, `pdo_mysql`).
3. Eksekusi pengujian otomatis `tests/run_checks.php`.

>>>>>>> 39cdf09 (Penjelasan Sistem dan Perkenalan Kelompok)
