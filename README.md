# Skrip Instalasi VPN Tunnel Premium untuk Regar Store

Skrip ini dirancang untuk mengubah server VPS Ubuntu 20.04 atau 22.04 baru menjadi server VPN dan tunneling yang lengkap dan canggih. Proses instalasi sepenuhnya otomatis dan dilengkapi dengan sistem manajemen pengguna, kuota, dan masa aktif.

## Fitur Utama

- **Protokol Lengkap**:
  - **XRAY**: VLESS, VMess, Trojan (semua melalui WebSocket di port 443/TLS dan 80/non-TLS).
  - **OpenVPN**: TCP (1194) dan UDP (2200).
  - **SSH Tunnel**: OpenSSH (22), Dropbear (109, 143).
  - **SSL Tunnel**: Stunnel4 untuk SSH, Dropbear, dan OpenVPN.
  - **WebSocket Tunnel**: SSH over WebSocket di port 80 & 8080.
  - **Proxy**: Squid Proxy di port 3128 & 8080.
  - **UDP Gateway**: Badvpn untuk gaming (port 7100-7300).

- **Manajemen Pengguna Canggih**:
  - **Menu Interaktif**: Kelola server dengan mudah menggunakan perintah `menu`.
  - **Manajemen via API**: Menambah dan menghapus pengguna XRAY secara dinamis tanpa me-restart layanan.
  - **Database Pengguna**: Menyimpan data pengguna, kuota, dan masa aktif di file database lokal.
  - **Sistem Kuota**: Batasi penggunaan data per pengguna (dalam GB).
  - **Masa Aktif**: Pengguna akan otomatis dihapus setelah tanggal kedaluwarsa tercapai.
  - **Monitoring Otomatis**: Skrip monitor berjalan setiap 5 menit untuk menegakkan aturan kuota dan masa aktif.

- **Keamanan & Performa**:
  - **Sertifikat SSL**: Otomatis mendapatkan dan memperbarui sertifikat dari Let's Encrypt.
  - **Firewall**: UFW dikonfigurasi untuk hanya membuka port yang diperlukan.
  - **Proteksi Brute-Force**: Fail2Ban untuk mengamankan SSH.
  - **Optimasi Kecepatan**: TCP BBR diaktifkan untuk koneksi yang lebih cepat dan stabil.

## Persyaratan

1.  Server VPS dengan sistem operasi **Ubuntu 20.04** atau **22.04**.
2.  Sebuah **nama domain** yang sudah Anda pointing (diarahkan) ke alamat IP VPS Anda.

## Cara Instalasi

1.  Sewa VPS baru dan pastikan Anda memiliki akses `root`.
2.  Login ke VPS Anda melalui SSH.
3.  Jalankan perintah berikut untuk mengunduh dan memulai instalasi:

    ```bash
    wget -O install.sh [URL_RAW_FILE_INSTALL.SH_ANDA] && chmod +x install.sh && ./install.sh
    ```
    > **PENTING**: Ganti `[URL_RAW_FILE_INSTALL.SH_ANDA]` dengan URL *raw* dari file `install.sh` di repositori GitHub Anda.
    >
    > **Contoh**: `https://raw.githubusercontent.com/nama-anda/repo-anda/main/install.sh`

4.  Skrip akan meminta Anda memasukkan nama domain. Pastikan domain sudah benar dan sudah diarahkan ke IP VPS.
5.  Tunggu proses instalasi selesai (sekitar 15-30 menit).
6.  Setelah selesai, informasi login dan port akan ditampilkan. Server akan disarankan untuk di-reboot.

## Cara Penggunaan

Setelah instalasi selesai, semua manajemen server dilakukan melalui perintah `menu`.

1.  Login ke SSH server Anda.
2.  Ketik `menu` dan tekan Enter.
3.  Anda akan melihat daftar opsi seperti:
    - **Add XRAY User**: Untuk membuat akun VLESS/VMess/Trojan baru. Anda akan diminta memasukkan username (email), kuota, batas IP, dan masa aktif.
    - **Delete XRAY User**: Untuk menghapus pengguna XRAY.
    - **List XRAY Users**: Untuk melihat daftar pengguna XRAY beserta kuota dan masa aktifnya.
    - **Add/Delete SSH/OpenVPN User**: Untuk mengelola pengguna SSH dan OpenVPN (tipe lama).
    - **Check Service Status**: Untuk melihat status semua layanan yang berjalan.
    - **Renew SSL Certificate**: Untuk memperbarui sertifikat SSL secara manual.

Sistem kuota dan masa aktif berjalan secara otomatis di latar belakang. Anda tidak perlu melakukan apa-apa setelah membuat pengguna melalui menu.

## Daftar Port Layanan

| Port        | Protokol | Layanan                               | Keterangan                             |
|-------------|----------|---------------------------------------|----------------------------------------|
| 22          | TCP      | OpenSSH                               | Akses utama SSH                        |
| 80          | TCP      | XRAY (VMess) & SSH-WS                 | Tunneling non-TLS                      |
| 109, 143    | TCP      | Dropbear                              | Alternatif SSH yang ringan             |
| 443         | TCP      | XRAY (VLESS, VMess, Trojan)           | Tunneling utama dengan enkripsi TLS    |
| 445         | TCP      | Stunnel                               | SSL Tunnel ke Dropbear (109)           |
| 777         | TCP      | Stunnel                               | SSL Tunnel ke OpenSSH (22)             |
| 1194        | TCP      | OpenVPN                               | OpenVPN melalui TCP                    |
| 2200        | UDP      | OpenVPN                               | OpenVPN melalui UDP                    |
| 3128, 8080  | TCP      | Squid Proxy & SSH-WS                  | Layanan proxy & WebSocket alternatif   |
| 7100-7300   | UDP      | Badvpn UDPGW                          | Untuk panggilan suara atau video game  |
| 8443        | TCP      | Stunnel                               | SSL Tunnel ke OpenVPN (1194)           |
