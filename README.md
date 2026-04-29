# Membobol Formulir Login dengan SQL Injection: Eksperimen Nyata dan Cara Mencegahnya

> Eksperimen keamanan web berbasis DVWA (Damn Vulnerable Web Application) untuk memahami mekanisme serangan SQL Injection dan implementasi mitigasinya menggunakan PHP PDO Prepared Statement.

## Identitas

| | |
|---|---|
| **Nama** | Wahyu Andika |
| **NIM** | 312410182 |
| **Program Studi** | Teknik Informatika |
| **Universitas** | Universitas Pelita Bangsa |
| **Mata Kuliah** | Pemrograman Web |

---

## Deskripsi

Repositori ini berisi dokumentasi dan kode eksperimen langsung yang dilakukan untuk memahami SQL Injection secara teknis dan empiris. Eksperimen dilakukan dalam lingkungan lokal yang sepenuhnya terisolasi menggunakan DVWA — aplikasi web yang dirancang khusus sebagai media edukasi keamanan siber.

---

## Lingkungan Eksperimen

| Komponen | Spesifikasi |
|---|---|
| Sistem Operasi | Windows 10/11 |
| Web Server | XAMPP (Apache + MySQL) |
| Bahasa Pemrograman | PHP 8.1 |
| Aplikasi Target | DVWA (Damn Vulnerable Web Application) |
| Browser | Google Chrome / Mozilla Firefox |

---

## Struktur Repositori

```
sql-injection-experiment/
├── README.md              # Dokumentasi utama repositori
├── mitigasi/
│   └── index.php          # Implementasi mitigasi menggunakan PDO Prepared Statement
└── docs/
    └── artikel.md         # Artikel lengkap hasil eksperimen
```

---

## Setup Eksperimen

### 1. Instalasi DVWA

```bash
# Clone repositori DVWA
git clone https://github.com/digininja/DVWA.git

# Pindahkan ke direktori htdocs XAMPP
# Windows: C:\xampp\htdocs\dvwa
# Linux: /var/www/html/dvwa
```

### 2. Konfigurasi Database

```php
// Edit file: dvwa/config/config.inc.php
$_DVWA[ 'db_server' ]   = '127.0.0.1';
$_DVWA[ 'db_database' ] = 'dvwa';
$_DVWA[ 'db_user' ]     = 'root';
$_DVWA[ 'db_password' ] = '';
$_DVWA[ 'db_port']      = '3306';
```

### 3. Inisialisasi

1. Akses `http://localhost/dvwa/setup.php`
2. Klik **Create / Reset Database**
3. Login dengan `admin` / `password`
4. Set Security Level ke **Low** via menu DVWA Security

---

## Payload yang Digunakan

```sql
-- Bypass autentikasi login
' OR '1'='1' --

-- Ekstraksi seluruh data user (pada halaman SQL Injection DVWA)
' OR '1'='1' #
```

---

## Kode Rentan (JANGAN digunakan di produksi)

```php
// BERBAHAYA — input langsung dimasukkan ke query tanpa sanitasi
$query = "SELECT * FROM users
          WHERE username = '$username' AND password = '$password'";
$result = mysqli_query($conn, $query);
```

---

## Mitigasi — Cara Penggunaan

### Instalasi

```bash
# Pastikan XAMPP sudah berjalan
# Salin folder mitigasi ke htdocs

cp -r mitigasi/ C:/xampp/htdocs/mitigasi/
```

### Akses

```
http://localhost/mitigasi/index.php
```

Sistem akan otomatis menguji payload SQL Injection menggunakan Prepared Statement. Output yang diharapkan:

```
Username atau password tidak valid. Injeksi SQL tidak berhasil.
```

---

## Hasil Eksperimen

| Skenario | Input | Hasil |
|---|---|---|
| Login normal | admin / password | Berhasil |
| Password salah | admin / salahpassword | Login failed |
| SQL Injection tanpa mitigasi | `' OR '1'='1' #` | Seluruh data user terdump |
| SQL Injection dengan mitigasi PDO | `' OR '1'='1' #` | Ditolak — payload diperlakukan sebagai string biasa |

---

## Referensi

- Disawal, S., & Suman, U. (2023). An approach to detect and prevent SQL injection and XSS vulnerability in the web application. *IJETT*, 71(8), 216–224. https://doi.org/10.14445/22315381/IJETT-V71I8P219
- DVWA Project. (2023). *Damn Vulnerable Web Application*. https://github.com/digininja/DVWA
- Nasereddin, M., et al. (2023). A systematic review of detection and prevention techniques of SQL injection attacks. *Information Security Journal*, 32(4), 252–265. https://doi.org/10.1080/19393555.2021.1995537
- Nugraha, L. A., et al. (2024). SQL injection: Analisis efektivitas uji penetrasi dalam aplikasi web. *Smatika Jurnal*, 14(01), 111–123. https://doi.org/10.32664/smatika.v14i01.1224
- OWASP Foundation. (2021). *OWASP Top 10:2021 — A03 Injection*. https://owasp.org/Top10/A03_2021-Injection/
- Paul, A., et al. (2024). SQL injection attack: Detection, prioritization & prevention. *Journal of Information Security and Applications*, 85, 103871. https://doi.org/10.1016/j.jisa.2024.103871
- The PHP Group. (2024). *PHP Manual: PDO*. https://www.php.net/manual/en/book.pdo.php

---

## Disclaimer

Seluruh eksperimen dalam repositori ini dilakukan dalam lingkungan lokal yang terisolasi menggunakan aplikasi yang memang dirancang untuk tujuan edukasi keamanan siber (DVWA). Tidak ada sistem, server, atau pihak lain yang menjadi target eksperimen. Repositori ini dibuat semata-mata untuk keperluan akademik.

---

## Lisensi

MIT License — bebas digunakan untuk keperluan edukasi dengan mencantumkan atribusi.
