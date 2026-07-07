# 🔑 Writeup Crackme #1: Lab_01_Validation

## 1. Informasi Target (Binary Information)
* **Nama File:** Lab_01_Validation.exe
* **Ukuran File:** 45 KB
* **Format / Arsitektur:** PE32 (32-bit Intel x86)
* **Nilai Hash SHA256:** `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`
* **Kompiler:** Microsoft Visual C/C++ (MSVC)
* **Proteksi/Packer:** Tidak ada (Unpacked)

## 2. Alur Analisis Statis (Static Analysis)
Saya membuka file biner ini menggunakan alat bantu **Detect It Easy (DIE)** untuk memastikan arsitektur dan memeriksa apakah file diproteksi oleh *packer* seperti UPX. Hasilnya menunjukkan file ini *unpacked*, sehingga string di dalamnya bisa dibaca langsung.

Selanjutnya, saya melakukan *Strings Analysis* menggunakan **HxD Hex Editor** dan mencari string yang mencurigakan di area memori data. Saya menemukan beberapa string menarik berikut:
* `Masukkan Lisensi: `
* `Akses Ditolak! Serial Salah.`
* `Akses Diberikan! Selamat datang, Admin.`
* `SuperSecretPassword123` <-- *Diduga kuat sebagai hardcoded password.*

## 3. Pembuktian dan Solusi
Aplikasi ini menggunakan validasi string sederhana secara langsung di dalam kode biner (*hardcoded key*). Ketika program dijalankan melalui Command Prompt:
1. Program meminta input lisensi.
2. Saya memasukkan string string tiruan acak, dan program mengeluarkan output: `Akses Ditolak! Serial Salah.`
3. Pada percobaan kedua, saya memasukkan kunci rahasia yang ditemukan pada analisis string: `SuperSecretPassword123`
4. Hasilnya, program berhasil ditembus dan mengeluarkan pesan: `Akses Diberikan! Selamat datang, Admin.`

## 4. Kesimpulan
Crackme pertama ini memiliki celah keamanan yang sangat mendasar, yaitu menyimpan kunci rahasia dalam bentuk teks polos (*plaintext*) langsung di dalam biner tanpa enkripsi atau hashing.
