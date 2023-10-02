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

### Home
Dalam home user dapat membuat memo atau note yang memiliki 3 jenis publikasi yaitu:
- Private : note hanya dapat dilihat oleh user yang menulisnya
- Visible to member : note hanya dapat dilihat oleh member dari sebuah kelompok
- Public to everyone : note dapat diakses oleh semua user dari memos
Dengan menekan button vertical ellipses yang terdapat pada note user dapat melakukan pin, archive, edit, mark, dan delete terhadap note yang telah dipublishnya.
![image](https://github.com/mochammadkevin/memos/assets/118964889/86af3d12-e304-4627-91c7-1010c07adb2c)

### Daily Review
Dalam daily review user dapat melihat semua note yang telah dipublish oleh user pada tanggal tertentu, selain itu user juga dapa mengunduh note yang telah dipublishnya melalui halaman ini.
![image](https://github.com/mochammadkevin/memos/assets/118964889/c0834edf-2265-4a84-ab10-f4a1c30d45dc)

### Resources
![image](https://github.com/mochammadkevin/memos/assets/118964889/768f717c-c34e-4823-9dd4-47ed74a112cf)

### Explore
Pada halaman explore user dapat melihat note yang telah dipublish oleh user lain baik itu notes yang bersifat visible to memeber atau public to everyone
![image](https://github.com/mochammadkevin/memos/assets/118964889/769abecf-1eb7-4c74-9474-3333130c5239)

### Archive
Pada halaman archive ini user dapat melihat note yang telah di archivenya pada halaman home. user kemudian dapat memilih untuk me-restore atau delete note yang telah di archivenya.
![image](https://github.com/mochammadkevin/memos/assets/118964889/3f731334-784d-4343-9922-b3e715adf97b)

### Setting
Pada halaman ini user dapa melakukan kostumisasi terhadap akunnya. Kostumisasi terbagi menjadi 2 yaitu My Account dan Preference. pada My account user dapat melakukan kostumisasi terhadap Username, email, dan nickname dengan menekan button edit. user juga dapat merubah password yang dimilika dengan menekan tombol change password. pada bagian preference user dapat melakukan kostumisisasi terhadap tema, bahasa, default jenis publikasi notenya, daily review time offset, dan user dapat menyalakan preferensi double klik untuk melakukan edit. bagian preferensi ini apa bila user memiliki telegram bot yang biasa digunakan untuk menyimpan note, user dapat memasukan id botnya agar note dapat dikirimkan menuju bot penyimpanan note yang dimiliki oleh user.
![image](https://github.com/mochammadkevin/memos/assets/118964889/c0a3a681-2304-44f6-9694-c44a0f2b4e69)
![image](https://github.com/mochammadkevin/memos/assets/118964889/afd924d4-9d9b-4061-8cbf-031adf90e332)

### Tutorial
![image](https://github.com/mochammadkevin/memos/assets/118964889/8a7759a0-662c-4d36-87bb-a9e2dbaa33cb)
![image](https://github.com/mochammadkevin/memos/assets/118964889/80c2c187-6cf5-40fe-8125-34c133cf2414)
![image](https://github.com/mochammadkevin/memos/assets/118964889/f972ad0a-15d7-4c1c-bf85-2ce71b640643)
![image](https://github.com/mochammadkevin/memos/assets/118964889/26a31d7a-be90-44f4-97fe-625636b7eeed)
![image](https://github.com/mochammadkevin/memos/assets/118964889/3deabc93-90ca-44c1-8325-966fedebecb5)
![image](https://github.com/mochammadkevin/memos/assets/118964889/1a8bc935-a205-44c6-aa47-6d2dc2a0647c)

## Pembahasan

- Pendapat anda tentang aplikasi web ini
    - kelebihan
    - kekurangan
- Bandingkan dengan aplikasi web lain yang sejenis


## Referensi

Cantumkan tiap sumber informasi yang anda pakai.
