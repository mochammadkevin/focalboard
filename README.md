# Memos
![image](https://github.com/mochammadkevin/memos/assets/88078382/cf3d9321-4d77-4636-9641-146b9c80ac62)

## Sekilas Tentang

Website Memos adalah platform yang memungkinkan pengguna untuk membuat, menyimpan, dan berbagi catatan online. Dengan fitur-fitur yang intuitif dan user-friendly, Memos membantu pengguna mengatur ide, proyek, dan informasi penting dengan mudah.

### Fitur Utama

- **Pembuatan Catatan**: Buat catatan dengan mudah melalui antarmuka yang sederhana dan ramah pengguna.

- **Penyimpanan Aman**: Data disimpan dengan keamanan terjamin.

- **Berbagi Catatan**: Bagikan catatan dengan orang lain dengan cepat melalui tautan unik.

- **Pencarian Cepat**: Temukan catatan yang dibutuhkan dengan cepat menggunakan fitur pencarian.

- **Dukungan Multi-Platform**: Akses catatan di semua perangkat dengan dukungan multi-platform.


## Instalasi

Sebelum Anda memulai instalasi, pastikan Anda memiliki:

- [Docker](https://www.docker.com/get-started) terinstal pada sistem Anda.
- Dalam contoh ini, kami menggunakan [DigitalOcean](https://www.digitalocean.com/) sebagai tempat untuk menghosting VM yang nantinya akan digunakan.

## Panduan Membuat Droplet di DigitalOcean

### Langkah 1: Buat Akun DigitalOcean

- Buka [DigitalOcean](https://www.digitalocean.com/) dan buat akun jika Anda belum memiliki satu.

### Langkah 2: Buat Droplet

- Masuk ke akun DigitalOcean Anda.

- Klik tombol "Create" dan pilih "Droplets" dari menu drop-down.

- Pilih sistem operasi yang ingin Anda gunakan.

- Pilih plan Droplet sesuai kebutuhan Anda.

- Pilih lokasi server (data center) yang terdekat dengan target audiens Anda.

- (Opsional) Pilih fitur-fitur tambahan seperti monitoring atau backups.

- Klik "Create Droplet" untuk membuat Droplet.

### Langkah 3: Akses Droplet via SSH

- Buka terminal di komputer Anda.

- Gunakan perintah SSH untuk mengakses Droplet Anda. Gantilah `<your-droplet-ip>` dengan alamat IP Droplet Anda:
  
    ```bash
    ssh root@<your-droplet-ip>
    ```

- Anda akan diminta memasukkan kata sandi yang telah Anda terima melalui email. Masukkan kata sandi tersebut.

### Langkah 4: Konfigurasi Droplet

- Setelah berhasil masuk, Anda dapat mengonfigurasi Droplet Anda sesuai kebutuhan, menginstal perangkat lunak, dan mengatur pengguna tambahan.

- Pastikan untuk mengamankan Droplet Anda dengan mengubah kata sandi root dan melakukan langkah-langkah keamanan dasar.

- Untuk memperbarui paket perangkat lunak, jalankan perintah:

    ```bash
    apt update
    apt upgrade
    ```

- (Opsional) Instal perangkat lunak tambahan yang Anda butuhkan.

### Langkah 5: Buat Firewall Baru

- Di panel kontrol DigitalOcean, klik "Networking" dan pilih "Firewalls" dari menu navigasi.

- Klik tombol "Create Firewall".

- Isi informasi berikut:
   - **Name**: Beri nama firewall Anda (misalnya, "Custom-Port-1000").
   - **Inbound Rules**: Tambahkan aturan untuk membuka port 1000. Pilih "Add Rule", pilih "TCP" sebagai protokol, dan masukkan "3000" sebagai port.

- Klik "Create Firewall" untuk membuat firewall.

### Langkah 6: Hubungkan Firewall dengan Droplet

- Di halaman Firewall yang baru saja Anda buat, klik tab "Droplets".

- Pilih droplet yang ingin Anda beri izin untuk mengakses port 1000.

- Klik "Apply" untuk menghubungkan firewall dengan droplet tersebut.


### Langkah 7: Instalasi Memos via Docker

- Buka kembali terminal di komputer Anda.

- Gunakan perintah SSH untuk mengakses Droplet Anda. Gantilah `<your-droplet-ip>` dengan alamat IP Droplet Anda:
  
    ```bash
    ssh root@<your-droplet-ip>
    ```
- Setelah berhasil masuk ke Droplet, buatlah sebuah folder baru dengan menggunakan command berikut:
  
    ```bash
    mkdir namafolder
    ```
- Masuk ke dalam folder yang telah dibuat dengan menggunakan command berikut:
  
    ```bash
    cd namafolder
    ```
- Jalankan Focalboard Personal Server dengan perintah berikut:
    
     ```bash
    docker run -it -p 3000:8000 mattermost/focalboard
    ```
    Catatan: port 3000 bisa disesuaikan dengan port yang ingin dipakai

- Setelah menjalankan perintah di atas, Anda dapat mengakses aplikasi Focalboard melalui alamat IP Droplet yang Anda gunakan beserta port yang telah ditentukan. Contoh: http://your-droplet-ip:3000/


## Cara Pemakaian

- Tampilan aplikasi web
- Fungsi-fungsi utama
- Isi dengan data real/dummy (jangan kosongan) dan sertakan beberapa screenshot


## Pembahasan

- Pendapat anda tentang aplikasi web ini
    - kelebihan
    - kekurangan
- Bandingkan dengan aplikasi web lain yang sejenis


## Referensi

Cantumkan tiap sumber informasi yang anda pakai.
