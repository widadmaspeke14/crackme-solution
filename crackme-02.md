# 🔑 Writeup Crackme #2: Keygen_Me_Easy

## 1. Informasi Target (Binary Information)
* **Nama File:** Keygen_Me_Easy.exe
* **Ukuran File:** 120 KB
* **Format / Arsitektur:** PE64 (64-bit AMD64)
* **Nilai Hash SHA256:** `8f435b84920438a9248bf94c730415391c49124a9104bc149a149bc3482312d4`
* **Kompiler:** GCC (MinGW)
* **Proteksi/Packer:** Tidak ada (Unpacked)

## 2. Analisis Dinamis & Debugging
Karena analisis string statis tidak menunjukkan adanya password langsung, saya menggunakan teknik analisis dinamis menggunakan **x64dbg**. 

1. Saya memuat biner ke dalam debugger dan mencari string referensi untuk menemukan fungsi percabangan utama.
2. Saya menemukan instruksi pembandingan string (`test eax, eax` atau `cmp`) diikuti oleh instruksi pencabangan bersyarat yaitu **`JZ` (Jump if Zero)** atau **`JE` (Jump if Equal)** yang menuju ke fungsi kegagalan registrasi.



## 3. Metode Penyelesaian (Patching)
Untuk melewati validasi ini, saya menerapkan teknik *Binary Patching* pada instruksi *Assembly*:
* Alamat memori asli: `0x004015AC`
* Instruksi asli: `JZ short 0x004015C0` (Artinya: Jika input salah, lompat ke fungsi "Gagal").
* Saya mengubah (*patch*) instruksi tersebut menjadi kebalikannya, yaitu **`JNZ` (Jump if Not Zero)**, atau menggantinya dengan instruksi **`NOP` (No Operation)** agar program mengabaikan validasi dan terus berjalan ke fungsi sukses.

Setelah dilakukan patching, saya menyimpan biner baru tersebut dengan nama `Keygen_Me_Easy_Patched.exe`.

## 4. Pembuktian Akhir
Saat file `Keygen_Me_Easy_Patched.exe` dijalankan, saya memasukkan serial acak seperti `1111-2222`. Program secara otomatis mengabaikan pengecekan dan langsung menampilkan status: `Registrasi Sukses! Aplikasi Terbuka.`

## 5. Kesimpulan
Aplikasi berhasil ditembus dengan memanipulasi *flag* atau alur logika kontrol instruksi assembly (*conditional branching manipulation*) memanfaatkan debugger.
