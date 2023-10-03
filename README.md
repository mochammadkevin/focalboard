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


### Langkah 7: Deploy Memos dengan Docker

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
- Menggunakan Docker Run
    
    ```bash
    docker run -d \
      --init \
      --name memos \
      --publish 1000:5230 \
      --volume ~/.memos/:/var/opt/memos \
      ghcr.io/usememos/memos:latest
    ```
    Catatan:

    Perintah ini akan menjalankan Memos di latar belakang dan mengeksposnya pada port 5230. Data akan disimpan di ~/.memos/. Anda dapat mengubah port dan jalur direktori data sesuai kebutuhan. Namun, hanya ubah     port pertama, misalnya 1000:5230, untuk menggunakan port host yang berbeda. Port kedua mengacu pada port yang digunakan oleh Memos di dalam kontainer. Aturan yang sama berlaku untuk jalur direktori: jalur       pertama adalah di sistem host Anda, dan jalur kedua adalah di dalam kontainer.

- Menggunakan Docker Compose

  Untuk mendeploy Memos menggunakan Docker Compose, buat file docker-compose.yml dengan konten berikut:
  ```bash
    version: "3.0"
    services:
      memos:
        image: neosmemo/memos:latest
        container_name: memos
        volumes:
          - ~/.memos/:/var/opt/memos
        ports:
          - 1000:5230
    ```

  Anda dapat memulai Memos menggunakan perintah berikut:
  ```bash
  docker-compose up -d
  ```
  
- Setelah menjalankan perintah di atas, Anda dapat mengakses aplikasi Memos melalui alamat IP Droplet yang Anda gunakan beserta port yang telah ditentukan. Contoh: http://your-droplet-ip:1000/

##  Maintenance

### Memperbarui Versi Memos dengan Docker
Untuk memperbarui Memos ke versi terbaru, Anda perlu menghentikan dan menghapus kontainer lama terlebih dahulu:
```bash
docker stop memos && docker rm memos
```

Disarankan, tetapi opsional, untuk melakukan cadangan database:
```bash
cp -r ~/.memos/memos_prod.db ~/.memos/memos_prod.db.bak
```

Selanjutnya, unduh versi terbaru dengan perintah berikut:
```bash
docker pull ghcr.io/usememos/memos:latest
```

Terakhir, jalankan kembali Memos dengan mengikuti langkah-langkah yang ada pada langkah 7

## Cara Pemakaian
### Login Page
![WhatsApp Image 2023-10-03 at 10 02 58](https://github.com/mochammadkevin/memos/assets/132868704/3c7a12ea-ebff-4f2c-a948-583f21484c4a)
- Sign up : Membuat akun baru bagi pengguna yang belum memiliki akun. Tidak dapat membuat akun dengan username yang sama. Akun dengan username yang sama akan menampilkan pesan "Failed to create user".
- Sign in : Pengguna yang sudah memiliki akun masuk ke dalam akun.
- Language selection : Opsi yang memungkinkan pengguna untuk memilih bahasa yang sesuai dengan kebutuhan.
- Follow system : Opsi yang memungkinkan pengguna untuk mengubah tampilan antarmuka pengguna dari tema yang terang (light mode) ke tema yang gelap (dark mode) atau sebaliknya. 

### Home
Pada halaman home, pengguna dapat membuat memo atau catatan dengan tiga jenis publikasi yang berbeda:
- Private : Note hanya dapat dilihat oleh pengguna yang membuatnya.
- Visible to member : Note hanya dapat dilihat oleh anggota dari sebuah kelompok.
- Public to everyone : Note dapat diakses oleh semua pengguna dari memo.
Dengan menekan tombol titik-titik vertikal yang terdapat pada catatan, pengguna dapat melakukan berbagai tindakan seperti menyematkan (Pin), mengarsipkan (Archive), mengedit (Edit), menandai (Mark), dan menghapus catatan yang telah dipublikasikan (Delete).

![image](https://github.com/mochammadkevin/memos/assets/118964889/86af3d12-e304-4627-91c7-1010c07adb2c)

### Daily Review
Dalam daily review, user dapat melihat semua note yang telah dipublish oleh user pada tanggal tertentu, selain itu user juga dapa mengunduh note yang telah dipublishnya melalui halaman ini.

![WhatsApp Image 2023-10-03 at 09 31 47](https://github.com/mochammadkevin/memos/assets/132868704/26e982d3-b623-4413-8030-fff7bd2c4836)

### Resources
Pada halaman resources, user dapat melihat semua file dalam note yang diunggah pada waktu tertentu. User juga dapat mengunduh file yang telah diunggahnya.

![WhatsApp Image 2023-10-03 at 09 32 47](https://github.com/mochammadkevin/memos/assets/132868704/f7114d53-baa4-4f23-bdfd-a8319744ac6f)

### Explore
Pada halaman explore user dapat melihat note yang telah dipublish oleh user lain baik itu notes yang bersifat visible to member atau public to everyone

![WhatsApp Image 2023-10-03 at 09 43 23](https://github.com/mochammadkevin/memos/assets/132868704/840a1773-3768-4b05-9040-3605e9e8a5e6)

### Archive
Pada halaman archive ini user dapat melihat note yang telah di archivenya pada halaman home. user kemudian dapat memilih untuk me-restore atau delete note yang telah di archivenya.

![WhatsApp Image 2023-10-03 at 09 44 27](https://github.com/mochammadkevin/memos/assets/132868704/2e87d5f8-a5c2-41af-bb8c-54b41d5e1196)

### Setting
Pada halaman ini user dapat melakukan kustomisasi terhadap akunnya. Kustomisasi terbagi menjadi 2, yaitu My Account dan Preference. Pada My Account, user dapat melakukan kustomisasi terhadap Username (Digunakan untuk sign in), Nickname (Nama yang ditampilkan pada website), dan E-mail dengan menekan button edit. User juga dapat merubah password yang dimiliki dengan menekan tombol Change password. Pada bagian Preferences, user dapat melakukan kustomisasi terhadap tema, bahasa, default jenis publikasi notenya, daily review time offset, dan user dapat menyalakan preferensi double klik untuk melakukan edit. Bagian preferensi ini apabila user memiliki telegram bot yang biasa digunakan untuk menyimpan note. User dapat memasukkan id botnya agar note dapat dikirimkan menuju bot penyimpanan note yang dimiliki oleh user. Pada halaman user yang merupakan Admin, Admin dapat mengatur siapa saja yang merupakan member, mengubah password user lain, dan meng-archive user lain.

![image](https://github.com/mochammadkevin/memos/assets/118964889/c0a3a681-2304-44f6-9694-c44a0f2b4e69)
![WhatsApp Image 2023-10-03 at 09 50 40](https://github.com/mochammadkevin/memos/assets/132868704/95dfa9aa-97ca-48b0-814d-adbdd9f1e1fa)
![WhatsApp Image 2023-10-03 at 09 50 52](https://github.com/mochammadkevin/memos/assets/132868704/f5491602-1eda-4ffe-8f5e-fe50f3a76212)
![WhatsApp Image 2023-10-03 at 09 45 00](https://github.com/mochammadkevin/memos/assets/132868704/8af97053-ace3-43d4-8582-8b7de1bf7d6e)
![image](https://github.com/mochammadkevin/memos/assets/118964889/7a0018d6-5a85-4f69-b1bb-ef225d8160c0)

### Tutorial
![image](https://github.com/mochammadkevin/memos/assets/118964889/8a7759a0-662c-4d36-87bb-a9e2dbaa33cb)
![image](https://github.com/mochammadkevin/memos/assets/118964889/80c2c187-6cf5-40fe-8125-34c133cf2414)
![image](https://github.com/mochammadkevin/memos/assets/118964889/f972ad0a-15d7-4c1c-bf85-2ce71b640643)
![image](https://github.com/mochammadkevin/memos/assets/118964889/26a31d7a-be90-44f4-97fe-625636b7eeed)
![image](https://github.com/mochammadkevin/memos/assets/118964889/3deabc93-90ca-44c1-8325-966fedebecb5)
![image](https://github.com/mochammadkevin/memos/assets/118964889/1a8bc935-a205-44c6-aa47-6d2dc2a0647c)
![image](https://github.com/mochammadkevin/memos/assets/118964889/954d615a-a2f9-4aaf-bafd-37497afcfaa5)
![image](https://github.com/mochammadkevin/memos/assets/118964889/d7e9d38c-33f9-4d19-8a8b-9bc422a009c1)
![image](https://github.com/mochammadkevin/memos/assets/118964889/fd6c3d19-5ff7-4db1-aabb-fc368dd0be3a)







## Pembahasan
- Pendapat anda tentang aplikasi web ini
  
- Kelebihan
  1. Privasi, seluruh data disimpan dalam file database SQLite
  2. Teks biasa dengan markdown, Konten disimpan dalam bentuk teks biasa (plain text), bukan HTML. Banyak sintaks       markdown yang berguna didukung.
  3. Ringan, tapi kuat, Menggunakan arsitektur Go + React.js + SQLite. Keseluruhan paket sangat ringan.
  4. Dapat disesuaikan, dapat menyesuaikan nama server, ikon, deskripsi, gaya sistem khusus dan skrip eksekusi, dll.
  5. Sepenuhnya Open Source
  6. Gratis selamanya, Semua fitur gratis selamanya dan tidak akan pernah dikenakan biaya dalam bentuk atau konten apa pun.
      
- Kekurangan
  1. Antar user tidak bisa saling mengirim pesan (tidak ada komunikasi antar user), hanya bisa melihat note yang diunggah user lain.
     ![image](https://github.com/mochammadkevin/memos/assets/118964889/26a31d7a-be90-44f4-97fe-625636b7eeed)
  2. Pengguna lain hanya dapat menyalin note dan membuka file yang diunggah oleh user. Pengguna lain tidak dapat melakukan tindakan lainnya, seperti membalas note yang diunggah.
  3. File Audio dan Video yang diunggah oleh user melalui External Link tidak dapat diakses pengguna lain. File Audio dan Video hanya dapat diakses user itu sendiri melalui Resources.
         
- Bandingkan dengan aplikasi web lain yang sejenis


## Referensi
1. [About memos](https://usememos.com/) - Memos
2. [How to Create a Droplet](https://docs.digitalocean.com/products/droplets/how-to/create/) - DigitalOcean
3. [Install with Docker - Memos](https://usememos.com/docs/install/docker) - Memos
4. [Memos Review](https://www.youtube.com/watch?v=w7aH23Jyg58&t=137s) - TechHut
