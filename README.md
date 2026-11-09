# Master Data Nama Wilayah Di Indonesia

Master Data Nama Wilayah di Indonesia Dalam Bentuk File SQL (MySQL / MariaDB).

Data mengacu pada regulasi resmi pemerintah Republik Indonesia terbaru:
**Keputusan Menteri Dalam Negeri (Kepmendagri) No. 300.2.2-2430 Tahun 2025** *(Perubahan atas Kepmendagri No. 300.2.2-2138 Tahun 2025)* tentang Pemberian dan Pemutakhiran Kode, Data Wilayah Administrasi Pemerintahan, dan Pulau.

## Ringkasan Data

Database terdiri dari 4 file master berelasi hirarki:

1. **`master_provinsi.sql`**: **38 Provinsi** (mencakup 4 Daerah Otonom Baru / DOB di Papua: Papua Selatan, Papua Tengah, Papua Pegunungan, dan Papua Barat Daya).
2. **`master_kabupaten.sql`**: **514 Kabupaten / Kota** (416 Kabupaten dan 98 Kota).
3. **`master_kecamatan.sql`**: **7.285 Kecamatan**.
4. **`master_kelurahan.sql`**: **83.762 Kelurahan / Desa**.

## Struktur Tabel & Relasi

### 1. `master_provinsi`
* `kode_provinsi` (VARCHAR(2), PRIMARY KEY)
* `nama_provinsi` (VARCHAR(255))

### 2. `master_kabupaten`
* `kode_kabupaten` (VARCHAR(5), PRIMARY KEY)
* `kode_provinsi` (VARCHAR(2)) -> Relasi ke `master_provinsi.kode_provinsi`
* `nama_kabupaten` (VARCHAR(255))

### 3. `master_kecamatan`
* `kode_kecamatan` (VARCHAR(8), PRIMARY KEY)
* `kode_kabupaten` (VARCHAR(5)) -> Relasi ke `master_kabupaten.kode_kabupaten`
* `nama_kecamatan` (VARCHAR(255))

### 4. `master_kelurahan`
* `kode_kelurahan` (VARCHAR(13), PRIMARY KEY)
* `kode_kecamatan` (VARCHAR(8)) -> Relasi ke `master_kecamatan.kode_kecamatan`
* `nama_kelurahan` (VARCHAR(255))

## Contoh Query JOIN

```sql
SELECT 
    p.kode_provinsi,
    p.nama_provinsi,
    b.kode_kabupaten,
    b.nama_kabupaten,
    c.kode_kecamatan,
    c.nama_kecamatan,
    l.kode_kelurahan,
    l.nama_kelurahan
FROM master_kelurahan l
JOIN master_kecamatan c ON l.kode_kecamatan = c.kode_kecamatan
JOIN master_kabupaten b ON c.kode_kabupaten = b.kode_kabupaten
JOIN master_provinsi p ON b.kode_provinsi = p.kode_provinsi
ORDER BY l.kode_kelurahan ASC;
```

---
Update data terakhir pada : 9 November 2026.
