# Penilaian Sumatif Akhir Tahun
## Mapil DevOps XI TJKT 1 - Penilaian Praktek
### SMKN 1 Banyumas - TA. 2024 2025


# Cara mendeploy Aplikasi


# Deploy aplikasi psat2425 ke ec2 dan rds
panduan ini digunakan untuk mengatur instance ec2, menghubungkannya dengan rds, dan menjalankan script otomatis deploy aplikasi berbasis php.


## langkah 1: buat instance ec2
1. buka aws dan masuk ke **ec2**
2. klik **launch instance**
3. isi konfigurasi berikut:
   - **name:** awsnovia (bebas)
   - **os:** ubuntu (pilih versi terbaru)
   - **instance type:** t2.micro
   - **key pair:** buat baru atau pilih vockey yang sudah ada 
   - **network settings:** pilih create security group baru
     - izinkan port:  
       - ssh (22)  
       - http (80)  
       - https (443)  
     - source: anywhere (0.0.0.0/0)


## langkah 2: isi user data script otomatis
1. masih di halaman launch instance, klik **advanced details** dibagian paling bawah 
2. lalu pencet tombol seperti segitiga di sebelah kiri tulisan advanced details
3. scroll ke bawah ke bagian **user data**
4. tempel script berikut:

   ```bash
   #!/bin/bash
   echo '#!/bin/bash
   sudo apt update -y
   sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
   sudo rm -rf /var/www/html/{,.}
   sudo git clone https://github.com/noviamawar/psat2425 /var/www/html
   sudo chmod -R 777 /var/www/html
   echo DB_USER=admin > /var/www/html/.env
   echo DB_PASS=P4ssw0rd  >> /var/www/html/.env
   echo DB_NAME=novia >> /var/www/html/.env
   echo DB_HOST=novia.cc2bivfsdhgp.us-east-1.rds.amazonaws.com >> /var/www/html/.env
   sudo apt install -y openssl
   sudo a2enmod ssl
   sudo a2ensite default-ssl.conf
   sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

   chmod +x /home/ubuntu/otomatis.sh

## Penjelasan Singkat Beberapa Baris Script

git clone https://github.com/noviamawar/psat2425 /var/www/html
→ ini pakai repository github milik kita sendiri yang berisi source code aplikasinya.

echo DB_USER=admin > /var/www/html/.env
→ isi dengan username database, bebas tapi umumnya ditulis admin.

echo DB_PASS=P4ssw0rd >> /var/www/html/.env
→ isi dengan password database RDS kita sendiri, bisa ditentukan bebas waktu buat database.

echo DB_NAME=novia >> /var/www/html/.env
→ isi dengan nama database yang kamu buat di RDS, misalnya pakai nama kamu biar unik.

echo DB_HOST=novia.ctqieeo2mefg.us-east-1.rds.amazonaws.com >> /var/www/html/.env
→ isi dengan endpoint dari RDS (bisa kamu lihat di dashboard AWS RDS, bagian endpoint).


## langkah 3: koneksi ke instance
1. setelah instance jalan, klik nama instance di aws
2. klik tombol connect


## langkah 4: jalankan script otomatis
setelah terhubung ke instance lewat terminal:
sudo ./otomatis.sh


## langkah 5: cek hasilnya
1. salin IPv4 Public IP dari halaman instance
2. buka browser dan ketik IP Public 13.218.175.26
3. jika berhasil, halaman aplikasi psat2425 akan muncul