# openXPKI
## Tim Abisun

OpenXPKI (Open eXtensible Public Key Infrastructure) adalah sebuah framework sumber terbuka (open-source) untuk membangun dan mengelola Infrastruktur Kunci Publik (PKI) yang digunakan untuk otentikasi, enkripsi, tanda tangan digital, dan pengelolaan sertifikat digital. PKI sendiri adalah sistem yang digunakan untuk mengelola kunci kriptografi dan sertifikat yang digunakan untuk memastikan komunikasi yang aman melalui jaringan yang tidak aman, seperti internet.

1. Manajemen Sertifikat: OpenXPKI menyediakan fitur untuk pembuatan, pembaruan, pencabutan, dan pengelolaan sertifikat digital berdasarkan standar PKI seperti X.509. Sertifikat ini digunakan untuk mengenkripsi data dan memverifikasi identitas pihak yang terlibat dalam komunikasi.

2. Otentikasi dan Keamanan: Dengan menggunakan PKI, OpenXPKI memastikan bahwa komunikasi yang terjadi antara dua pihak adalah aman dan terverifikasi, menggunakan algoritma enkripsi kuat.

3. Integrasi dengan Sistem Lain: OpenXPKI dapat diintegrasikan dengan sistem lain seperti LDAP untuk penyimpanan informasi sertifikat, sistem manajemen kunci, dan aplikasi lain yang memerlukan pengelolaan sertifikat atau enkripsi.

4. Dukungan untuk Standar dan Protokol: OpenXPKI mendukung berbagai standar dan protokol untuk keamanan, termasuk algoritma enkripsi modern (seperti RSA dan ECDSA) dan protokol seperti OCSP (Online Certificate Status Protocol) untuk memeriksa status sertifikat.

5. Modular dan Dapat Disesuaikan: OpenXPKI dirancang untuk fleksibel dan dapat disesuaikan dengan berbagai kebutuhan, dengan dukungan untuk berbagai jenis penyimpanan sertifikat dan konfigurasi layanan yang dapat disesuaikan.

### Instalasi openXPKI (Menggunakan Docker)

Sebelum menginstal OpenXPKI, pastikan Docker, Docker Compose, dan Make sudah terinstal di sistem Linux. Berikut adalah cara instalasi untuk setiap komponen:

- Instalasi Docker <br>
    Docker adalah platform untuk mengembangkan, mengirim, dan menjalankan aplikasi dalam kontainer. Berikut cara menginstal Docker di Ubuntu:

    ```
    sudo apt update
    sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt update
    sudo apt install -y docker-ce docker-ce-cli containerd.io
    sudo usermod -aG docker $USER
    docker --version
    ```
    Jika Docker terinstall akan seperti ini:

    ![docker-version](/documentation/docker-version.png)

- Instalasi Docker Compose <br>
    Docker Compose memungkinkan untuk mendefinisikan dan menjalankan aplikasi multi-container. Berikut cara menginstalnya:
    ```
    sudo apt update
    sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    docker-compose --version
    ```

    Jika Docker-compose terinstall akan seperti ini:

    ![Docker-compose](/documentation/docker-compose.png)

- Instalasi Make
    Make digunakan untuk mengotomatiskan proses build dan kompilasi. Berikut cara menginstalnya:
    ```
    sudo apt update
    sudo apt install make
    make --version
    ```

    Jika Make terinstall akan seperti ini:

    ![Make](/documentation/make.png)

Setelah Docker, Docker Compose, dan Make terinstal dengan benar, maka siap untuk menginstal OpenXPKI.

Sekarang lanjut ke instalasi OpenXPKI. Berikut adalah langkah-langkahnya:

- Clone Repository Docker OpenXPKI <br>
    Langkah pertama adalah mengunduh (clone) repository Docker OpenXPKI. Gunakan perintah berikut untuk meng-clone repository:
    ```
    git clone https://github.com/openxpki/openxpki-docker.git
    ```

- Masuk ke Direktori Repository yang Sudah Diclone <br>
    Setelah repository berhasil di-clone, masuk ke dalam direktori openxpki-docker:
    ```
    cd openxpki-docker
    ```

- Clone Repository Konfigurasi OpenXPKI <br>
    Langkah berikutnya adalah meng-clone repository konfigurasi OpenXPKI. Gunakan perintah ini:
    ```
    git clone https://github.com/openxpki/openxpki-config.git --single-branch --branch=community
    ```

- Salin Konfigurasi local.yaml <br>
    Untuk mencegah server crash ketika database tidak tersedia, salin konfigurasi local.yaml ke dalam direktori konfigurasi:
    ```
    cp contrib/wait_on_init.yaml openxpki-config/config.d/system/local.yaml
    ```

- Jalankan Docker Compose <br>
    Sekarang jalankan Docker Compose untuk memulai OpenXPKI. Gunakan perintah `docker compose up`:
    ```
    docker-compose up
    ```

    Jika berhasil dijalankan maka akan memberikan output seperti ini pada terminal:

    ![docker-compose-up](/documentation/docker-compose-up.png)

#### Alternatif

Setelah melakukan Clone Repository Docker OpenXPKI & Masuk ke Direktori Repository yang Sudah Diclone bisa langsung jalankan `make compose`:

```
make compose
```

Maka outputnya seperti ini:

![docker-up](/documentation/docker-up.png)

Selanjutnya jangan lupa jalankan script untuk inisialisasi ca-signer dan vault:

```
make sample-config
```

Akan menjalankan script ini:

![sample-config](/documentation/sample-config.png)

#### Instalasi Selesai !!!

Setelah instalasi selesai maka selanjutnya dapat mengakses `https://localhost:8443/` lalu hasilnya akan seperti ini:

![landing-page](/documentation/landing-page.png)

### Penggunaan openXPKI

Pertama-tama terdapat demo account yang telah disiapkan pada inisiasi docker ini, antara lain:

- raop, rose, rob sebagai RA Operator
- alice dan bob sebagai common user
- caop sebagai CA Operator

Semua Credential account ini memiliki password yang sama yaitu openxpki

Untuk Demo penggunaannya adalah sebagai berikut:

Login sebagai Common User untuk melakukan Request Certificate. Mengikuti alur yang ada dan setelah diajukan akan seperti ini:

![request-ca](/documentation/request-ca.png)

Lalu login sebagai RA Operator untuk melakukan approve request yang diajukan, akan seperti ini:

![approve-ca](/documentation/approve-ca.png)

Jika sudah maka bisa login lagi sebagai common user untuk melihat certificate yang sudah ter generate. Dan juga bisa minta revoke jika certificate tersebut bermasalah. Dokumentasinya bisa dilihat di my workflows:

Request Revoke CA:

![revoke-ca](/documentation/revoke-ca.png)

Memantau workflows yang sudah diajukan:

![approve-revoke](/documentation/approve-revoke.png)

Lalu dari RA Operator bisa melakukan approve untuk revokenya dengan tampilan seperti ini sudah di ajukan:

![revoke-approved](/documentation/revoke-ca-approved.png)

Terakhir dari CA Operator juga memiliki beberapa fungsionalitas seperti ini:

![ca-ui](/documentation/ca-ui.png)
