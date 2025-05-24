# Penilaian Sumatif Akhir Tahun
## Mapil DevOps XI TJKT 1 - Penilaian Praktek
### SMKN 1 Banyumas - TA. 2024 2025

# Cara mendeploy Aplikasi

# deploy aplikasi psat2425 ke ec2 dan rds
panduan ini digunakan untuk mengatur instance ec2, menghubungkannya dengan rds, dan menjalankan script otomatis deploy aplikasi berbasis php.

## langkah 1: buat instance ec2
1. buka aws console dan masuk ke **ec2**
2. klik **launch instance**
3. isi konfigurasi berikut:
   - **name:** bebas
   - **os:** ubuntu (pilih versi terbaru)
   - **instance type:** t2.micro
   - **key pair:** buat baru atau pilih yang sudah ada
   - **network settings:** pilih create security group baru
     - izinkan port:  
       - ssh (22)  
       - http (80)  
       - https (443)  
     - source: anywhere (0.0.0.0/0)

## langkah 2: koneksi ke instance
1. setelah instance jalan, klik nama instance di aws
2. klik tombol connect

## langkah 3: jalankan script otomatis
setelah terhubung ke instance lewat terminal:
nano otomatis.sh #!/bin/bash
echo '#!/bin/bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
sudo rm -rf /var/www/html/{,.}
sudo git clone https://github.com/noviamawar/psat2425 /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=admin > /var/www/html/.env
echo DB_PASS=P4ssw0rd  >> /var/www/html/.env
echo DB_NAME=rdsnovia  >> /var/www/html/.env
echo DB_HOST=rdsnovia.c5lchhfugohi.us-east-1.rds.amazonaws.com >> /var/www/html/.env
sudo apt install -y openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

sudo chmod +x otomatis.sh
sudo ./otomatis.sh

## langkah 5: cek hasilnya
1. salin IPv4 Public IP dari halaman instance
2. buka browser dan ketik/paste IP Public 13.218.175.26
3. jika berhasil, aplikasi psat2425 akan muncul
