# Focalboard
![image](https://github.com/mochammadkevin/focalboard/assets/88078382/7e325659-0caa-4c71-9a0e-8af28cad3d1d)

## Sekilas Tentang

Focalboard adalah sebuah platform manajemen proyek yang bersifat sumber terbuka, mendukung berbagai bahasa, dan dapat di-host sendiri, yang menyajikan alternatif yang kuat untuk alat-alat seperti Trello, Notion, dan Asana dalam mengelola tugas-tugas dan proyek-proyek. Dengan Focalboard, Anda dapat dengan mudah mendefinisikan, mengorganisir, melacak, dan mengelola pekerjaan baik untuk individu maupun tim secara efisien.

Focalboard hadir dalam tiga edisi yang berbeda, masing-masing dengan kegunaan yang unik:

* **[Focalboard plugin](https://github.com/mattermost/focalboard/releases)**: Edisi ini mengintegrasikan Focalboard ke dalam instansi Mattermost yang sudah ada, menciptakan penggabungan yang kuat antara alat manajemen proyek dengan komunikasi tim dan kolaborasi dalam berbagai skala, sehingga memudahkan kerja sama tim.

* **[Personal Desktop](https://www.focalboard.com/docs/personal-edition/desktop/)**: Focalboard juga menyediakan aplikasi desktop mandiri yang dapat digunakan oleh individu. Aplikasi ini tersedia untuk platform macOS, Windows, dan Linux, dan memungkinkan pengguna untuk mengatur daftar tugas pribadi dan mengelola proyek-proyek pribadi mereka dengan mudah dan efisien.

* **[Personal Server](https://www.focalboard.com/download/personal-edition/ubuntu/)**: Bagi mereka yang membutuhkan server mandiri untuk pengembangan dan penggunaan pribadi, Focalboard juga menawarkan edisi Personal Server. Dengan ini, Anda dapat mengelola proyek-proyek Anda dan berkolaborasi dengan tim Anda sendiri, memberikan fleksibilitas yang lebih besar dalam mengatur pekerjaan dan tugas-tugas yang perlu diselesaikan.


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
   - **Name**: Beri nama firewall Anda (misalnya, "Custom-Port-3000").
   - **Inbound Rules**: Tambahkan aturan untuk membuka port 3000. Pilih "Add Rule", pilih "TCP" sebagai protokol, dan masukkan "3000" sebagai port.

- Klik "Create Firewall" untuk membuat firewall.

### Langkah 6: Hubungkan Firewall dengan Droplet

- Di halaman Firewall yang baru saja Anda buat, klik tab "Droplets".

- Pilih droplet yang ingin Anda beri izin untuk mengakses port 3000.

- Klik "Apply" untuk menghubungkan firewall dengan droplet tersebut.


### Langkah 7: Instalasi Focalboard via Docker

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

## Konfigurasi (opsional)

Setting server tambahan yang diperlukan untuk meningkatkan fungsi dan kinerja aplikasi, misalnya:
- batas upload file
- batas memori
- dll

Plugin untuk fungsi tambahan
- login dengan Google/Facebook
- editor Markdown
- dll


##  Maintenance (opsional)

Setting tambahan untuk maintenance secara periodik, misalnya:
- buat backup database tiap pekan
- hapus direktori sampah tiap hari
- dll


## Otomatisasi (opsional)

Skrip shell untuk otomatisasi instalasi, konfigurasi, dan maintenance.


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
