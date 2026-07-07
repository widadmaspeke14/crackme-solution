# 🔑 Writeup Crackme #3: Serial_Check_Basic

## 1. Informasi Target (Binary Information)
* **Nama File:** Serial_Check_Basic.exe
* **Ukuran File:** 78 KB
* **Format / Arsitektur:** PE32 (32-bit Intel x86)
* **Nilai Hash SHA256:** `5a8d438910bc439814a74cb912419a4e47bc9242d9302148293bc149a84210df`
* **Kompiler:** Microsoft Visual C++
* **Proteksi/Packer:** Tidak ada

## 2. Rekonstruksi Kode (Decompilation)
Pada tantangan ketiga ini, saya menggunakan **Ghidra** untuk melakukan dekompilasi kode mesin kembali menjadi representasi kode pseudo-C tingkat tinggi. Di dalam fungsi `main()`, terdapat algoritma matematika sederhana untuk memvalidasi *Username* dan *Serial Key*:

```c
// Representasi Pseudo-Code dari Ghidra
int check_serial(char *username, int serial_input) {
    int total = 0;
    for (int i = 0; username[i] != '\0'; i++) {
        total += (int)username[i]; // Menjumlahkan nilai ASCII dari username
    }
    int serial_valid = total * 1337; // Algoritma validasi rahasia
    if (serial_input == serial_valid) {
        return 1; // Sukses
    }
    return 0; // Gagal
}
