# Penjelasan Bagian Awal Instalasi Odoo

## 1. Update and upgrade the system
Bagian ini bertujuan untuk memastikan semua paket pada sistem operasi Ubuntu sudah versi terbaru dan tidak ada paket yang tidak diperlukan. Perintah `apt update`, `apt upgrade -y`, dan `apt autoremove -y` digunakan untuk memperbarui daftar paket, meng-upgrade semua paket yang terpasang, serta menghapus paket yang sudah tidak dibutuhkan.

## 2. Disabling password authentication
Bagian ini meningkatkan keamanan server dengan menonaktifkan autentikasi password pada SSH. Dengan menonaktifkan opsi ini, hanya metode autentikasi lain seperti SSH key yang dapat digunakan untuk login ke server, sehingga meminimalisir risiko brute-force password. Konfigurasi dilakukan dengan mengubah beberapa baris pada file `/etc/ssh/sshd_config` dan me-restart layanan SSH.

## 3. Setting up the timezones
Bagian ini mengatur zona waktu server agar sesuai dengan lokasi yang diinginkan, dalam contoh ini adalah `Africa/Kigali`. Pengaturan zona waktu yang benar penting untuk memastikan waktu pada log, jadwal tugas, dan aplikasi berjalan sesuai dengan waktu lokal yang diharapkan.