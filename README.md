## Praktikum Modul 3 Jaringan Komputer

**Kelompok A16 :**

| Nama | NRP |
| ----------- | ----------- |
| Clarissa Luna Maheswari | 5025211033 |
| Heru Dwi Kurniawan | 5025211055 

## Soal:
Perjalanan selanjutnya akan menggunakan peta berikut:

![image](https://github.com/herukurniawann/Jarkom-Modul-2-A16-2023/assets/93961310/475e4570-d30b-441f-922f-60069b9326fe)

dengan ketentuan sebagai berikut:
![image](https://github.com/herukurniawann/Jarkom-Modul-2-A16-2023/assets/93961310/3e4bbde9-db85-4dd8-a72c-91ff4cc135f2)
![image](https://github.com/herukurniawann/Jarkom-Modul-2-A16-2023/assets/93961310/15cb905f-783a-4d55-888f-03ff51b3374f)

## Soal 0

**Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.**

Pastikan domain riegel.canyon.a16.com diarahkan ke worker Laravel bernama Fern dengan alamat IP tetap `192.184.4.1`. Selain itu, atur domain `granz.channel.a16.com` agar mengarah pada worker PHP yang disebut Lugner dengan alamat IP tetap `192.184.3.1.` Semua klien harus mengadopsi konfigurasi dari DHCP Server agar terhubung dengan lancar.

**Heiter (DNS Server) Disimpan script.sh pada root, berisi :**

Isi file `/etc/bind/named.conf.local`

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/05c852c9-61de-4762-a954-6cd14f5e75b9)

```bash
zone "canyon.a16.com" {
        type master;
        file "/etc/bind/jarkom/canyon.a16.com";
};

zone "channel.a16.com" {
        type master;
        file "/etc/bind/jarkom/channel.a16.com";
};
```

Isi file `/etc/bind/riegel/riegel.canyon.a16.com` :

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/1434db3b-126e-4a5d-b0d1-c22db986b931)

```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     canyon.a16.com. root.canyon.a16.com. (
                        2023141101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      canyon.a16.com.
@       IN      A       10.7.2.1 ;ip load balancer
riegel  IN      A       10.7.3.1 ;p lawine
@       IN      AAAA    ::1
```
Isi file `/etc/bind/riegel/riegel.channel.a16.com` :

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/74573161-2d9d-4020-bb09-04a1e0e52439)

```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     canyon.a16.com. root.canyon.a16.com. (
                        2023141101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      canyon.a16.com.
@       IN      A       10.7.2.1 ;ip load balancer
riegel  IN      A       10.7.4.1 ;p lawine
@       IN      AAAA    ::1
```

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/1434db3b-126e-4a5d-b0d1-c22db986b931)

## Soal 1

(1) Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/91f2dbda-5d69-44d5-ad84-3038f4078c73)

**Aura ( Router DHCP Relay )**
```bash
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.7.0.0/16

auto eth1
iface eth1 inet static
	address 10.7.1.0
	netmask 255.255.255.0
auto eth2
iface eth2 inet static
	address 10.7.2.0
	netmask 255.255.255.0
auto eth3
iface eth3 inet static
	address 10.7.3.0
	netmask 255.255.255.0
auto eth4
iface eth4 inet static
	address 10.7.4.0
	netmask 255.255.255.0
```

**Himmel ( DHCP Server )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.1.1
	netmask 255.255.255.0
	gateway 10.7.1.0
```

**Heiter ( DNS Server )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.1.2
	netmask 255.255.255.0
	gateway 10.7.1.0
```

**Denken ( Database Server )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.2.1
	netmask 255.255.255.0
	gateway 10.7.2.0
```

**Eisen ( Load Balancer )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.2.2
	netmask 255.255.255.0
	gateway 10.7.2.0
```

**Frieren ( Laravel Worker )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.4.1
	netmask 255.255.255.0
	gateway 10.7.4.0
```

**Flamme ( Laravel Worker )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.4.2
	netmask 255.255.255.0
	gateway 10.7.4.0
```

**Fern ( Laravel Worker )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.4.3
	netmask 255.255.255.0
	gateway 10.7.4.0
```

**Lawine ( PHP Worker )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.3.1
	netmask 255.255.255.0
	gateway 10.7.3.0
```

**Linie ( PHP Worker )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.3.2
	netmask 255.255.255.0
	gateway 10.7.3.0
```

**Lugner ( PHP Worker )**
```bash
auto eth0
iface eth0 inet static
	address 10.7.3.3
	netmask 255.255.255.0
	gateway 10.7.3.0
```

**Revolte ( Client )**
```bash
auto eth0
iface eth0 inet dhcp
Richter ( Client )
auto eth0
iface eth0 inet static
	address 10.7.3.70
	netmask 255.255.255.0
	gateway 10.7.3.0
```
**Sein ( Client )**
```bash
auto eth0
iface eth0 inet dhcp
```

**Stark ( Client )**
```bash
auto eth0
iface eth0 inet dhcp
```

**Register Domain**

Heiter (DNS Server) Disimpan script.sh pada root, berisi :

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/e153b4cc-c17f-4ce5-8f80-bc6a94d9e1ab)

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update

wait

apt-get install bind9 -y

mkdir /etc/bind/jarkom

cp named.conf.local /etc/bind/

cp canyon.a16.com /etc/bind/jarkom/

cp channel.a16.com /etc/bind/jarkom/

cp ./named.conf.options /etc/bind/

service bind9 restart
```


**Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)**



## Soal 2,3 & 5

**- Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)**

**- Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)**

**- Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)**

Langkah Pertama, kita perlu menginstal `isc-dhcp-server` di node Himmel. Selanjutnya, letakkan konfigurasi ini ke jalurnya masing-masing.

Kemudian set-up `INTERFACESv4` ke `eth0` karena terhubung melaluinya


Isi dari `/etc/default/isc-dhcp-server` :

```bash
# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)

# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf

# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid

# Additional options to start dhcpd with.
#       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACESv4="eth0"
INTERFACESv6=""
```
Isi dari `/etc/dhcp/dhcpd.conf` :

```bash
subnet 10.7.1.0 netmask 255.255.255.0 {
}

subnet 10.7.3.0 netmask 255.255.255.0 {
    range 10.7.3.16 10.7.3.32;
    range 10.7.3.64 10.7.3.80;
    option routers 10.7.3.0;
    option broadcast-address 10.7.3.255;
    option domain-name-servers 10.7.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.7.4.0 netmask 255.255.255.0 {
    range 10.7.4.12 10.7.4.32;
    range 10.7.4.160 10.7.4.168;
    option routers 10.7.4.0;
    option broadcast-address 10.7.4.255;
    option domain-name-servers 10.7.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
```

isi File `/etc/default/isc-dhcp-relay`

Langkah Berikutnya, menyiapkan DNS Relay di node Aura sehingga server DHCP dapat mendistribusikan IP ke semua klien

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/a0549c4e-0452-4c19-92b8-3a973547341e)

```bash
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.7.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

## Soal 4
**Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)**

Semua klien akan mendapatkan akses internet dari DNS Server Heiter. Langkah pertama perlu menyiapkan forwarder di sana. Kita sudah setting DNS servernya di `dhcpd.conf`, sekarang kita bisa setting konfigurasi DNS servernya agar bisa meneruskan koneksi internet.

Isi dari file `/etc/bind/named.conf.options`

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/b523375b-2035-4d9c-8972-358bc9718959)


```bash
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

      // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};
```
Kemudian, mulai ulang `bind9`. Ketika kita memeriksa `/etc/resolv.conf` pada setiap node klien, mereka harus menunjuk pada alamat IP Heiter

## Soal 6
**Berjalannya waktu, petualang diminta untuk melakukan deployment.
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)**

Langkah pertama yaitu menginstal `wget` dan `unzip` untuk mendownload aset situs web dari Google Drive. Lalu `nginx`, `php`, dan `php-fpm` untuk membuat situs online.

```bash
apt update
apt install wget unzip nginx php php-fpm -y
```

Selanjutnya kita download file zip dari `Google Drive`, `unzip`, lalu pindahkan ke direktori `/var/www`. Kemudian, buat file konfigurasi untuk website, dan letakkan di `/etc/nginx/sites-available/modul-3`

## Soal 7

**Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)**

Langkah untuk menguji penyeimbang Eisen, kita dapat menggunakan utilitas benchmarking yang disediakan oleh Apache. Pertama, instal paket Apache2-utils

`apt install apache2-utils`

Langkah selanjutnya, untuk membuat 1000 permintaan dengan 100 permintaan/detik, gunakan perintah berikut

`ab -n 1000 -c 100 "http://10.7.2.2/"`

## Soal 8
**Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
-Nama Algoritma Load Balancer
-Report hasil testing pada Apache Benchmark
-Grafik request per second untuk masing masing algoritma. 
-Analisis (8)**

Langkah pertama yang perlu dilakukan benchmarking menggunakan berbagai algoritma loadbalancing yang disediakan oleh `nginx`. kita perlu menambahkan semua algoritma ke dalam konfigurasi nginx di load balancer

Isi file `/etc/nginx/sites-available/loadbalancer`

```bash
upstream myweb {
	# Round robin (default)
	server 10.7.3.1; # IP Lawine
	server 10.7.3.2; # IP Linie
	server 10.7.3.3; # IP Lugner

	# Weighted round robin
	#server 10.7.3.1 weight=5; # IP Lawine
	#server 10.7.3.2 weight=3; # IP Linie
	#server 10.7.3.3 weight=1; # IP Lugner

	# Least Connection
	#least_conn;
	#server 10.7.3.1; # IP Lawine
	#server 10.7.3.2; # IP Linie
	#server 10.7.3.3; # IP Lugner

	# IP Hash
	#ip_hash;
	#server 10.7.3.1; # IP Lawine
	#server 10.7.3.2; # IP Linie
	#server 10.7.3.3; # IP Lugner

	# Generic Hash
	#hash $request_uri consistent;
	#server 10.7.3.1; # IP Lawine
	#server 10.7.3.2; # IP Linie
	#server 10.7.3.3; # IP Lugner
}

server {
	listen 80 default_server;
	server_name _;

	location / {
		proxy_pass http://myweb;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $http_host;
	}
}
```
Selanjutnya, dengan menggunakan perintah ab, uji penyeimbang sebagai berikut

`ab -n 200 -c 10 "http://10.7.2.2/"`

Selanjutnya setelah dilakukan pengujian, catat penggunaan CPU setiap pekerja menggunakan htop dan hasil benchmark, lalu tambahkan ke grimoire

## Soal 9
**Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)**

**Hasil Benchmark**

**1. Round Robin Biasa**

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/3c6fb158-cc2e-4740-9249-5e2cb953a90e)

**2. Weight Round Robin**

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/f727379b-5e03-4f12-a904-ce61b8afa95b)

**3. leastconnection**

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/4d49b177-f222-450f-bad2-993d0e6cc44a)

**4. Ip Hast Hash**

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/048ca8d3-a46e-40dc-a735-c401b44668e5)

**5. Generic Hash**

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/8e86168e-4e4d-43aa-81d1-7ad0dac58db7)

Keterangan

**Round Robin**

Requests per second: 701.14

Time per request: 1.426ms

Transfer rate: 521.52 KB/sec

Failed requests: 67

**Weighted Round Robin**

Requests per second: 742.12

Time per request: 1.347ms

Transfer rate: 552.00 KB/sec

Failed requests: 67

**Least Connections**

Requests per second: 737.89

Time per request: 1.352ms

Transfer rate: 548.86 KB/sec

Failed requests: 65

**IP Hash**

Requests per second: 749.01

Time per request: 1.335ms

Transfer rate: 557.37 KB/sec

Failed requests: 0

**Generic Hash**

Requests per second: 842.21

Time per request: 1.187ms

Transfer rate: 626.72 KB/sec

Failed requests: 0

**Grafik**

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/64699cf7-cba4-4542-921e-7e9808d19dc1)

![image](https://github.com/herukurniawann/Jarkom-Modul-3-A16-2023/assets/93961310/ca995538-7f8d-4fc7-a252-355ccba16636)

**Analisis**

Algoritma Generic Hash menunjukkan performa terbaik dengan jumlah permintaan per detik tertinggi dan laju transfer tertinggi dan memiliki waktu per permintaan terendah, yang menunjukkan bahwa algoritma ini dapat menangani lebih banyak permintaan dalam waktu yang lebih singkat tanpa adanya permintaan yang gagal.

Algoritma IP Hash dan Generic Hash tidak menunjukkan permintaan yang gagal, yang menunjukkan keandalan yang lebih tinggi dibandingkan dengan algoritma lainnya.

Berikut ini adalah perbandingan lainnya dari algoritma-algoritma yang digunakan:

**Perbandingan Round Robin dan Weighted Round Robin**

Weighted Round Robin memiliki performa sedikit lebih baik dari Round Robin biasa, yang menunjukkan bahwa memberikan bobot yang berbeda kepada server yang berbeda dapat meningkatkan distribusi beban dan efisiensi.Least Connections Performa Least Connections sebanding dengan metode Round Robin, namun dengan jumlah permintaan gagal yang lebih sedikit, yang menunjukkan bahwa algoritma ini mungkin lebih baik dalam menangani beban trafik yang tidak merata.

**Cara Kerja Masing-masing Algoritma**

Round Robin mendistribusikan permintaan secara berurutan di antara server dalam sebuah kelompok. Setelah mencapai akhir kelompok, algoritma ini mulai lagi dari awal.
Weighted Round Robin memberikan bobot kepada setiap server berdasarkan kapasitasnya. Server dengan bobot lebih tinggi menerima lebih banyak koneksi daripada yang berbobot rendah. Least Connections mengarahkan lalu lintas ke server dengan jumlah koneksi aktif paling sedikit, yang bisa menguntungkan jika ada server dengan karakteristik kinerja yang berbeda. IP Hash menggunakan hash dari alamat IP klien untuk menentukan server mana yang menerima permintaan, memastikan klien secara konsisten mencapai server yang sama. Generic Hash bisa menjadi mekanisme hashing apa pun yang mendistribusikan permintaan masuk berdasarkan nilai hash yang dihitung, seperti hash dari sesi atau parameter, yang dapat mendistribusikan beban lebih merata. Berdasarkan data, untuk sistem yang memprioritaskan throughput dan keandalan, algoritma Generic Hash akan menjadi pilihan terbaik karena performa tinggi dan tidak adanya permintaan gagal. 

## Soal 10
**Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ (10)**

Hal pertama yang perlu menambahkan autentikasi ke penyeimbang sehingga hanya dapat diakses jika kita dapat memberikan kredensial yang benar. Kita perlu mengatur kredensial dengan `htpasswd`. Jika Anda sudah menginstal paket `Apache2-utils` di mesin Anda, Anda seharusnya sudah memiliki perintahnya.

`htpasswd -c .htpasswd netics`

Selanjutnya, masukkan kata sandi Anda untuk pengguna `netics`. Pada kasus ini adalah ajka16. Lalu pindahkan file password .htpasswd ke direktori `/etc/nginx/rahasiakita/`

```bash
mkdir -p /etc/nginx/rahasiakita
mv .htpasswd /etc/nginx/rahasiakita/.htpasswd
```

Selanjutnya, kita perlu menambahkan baris ini ke konfigurasi penyeimbang `nginx`

Isi file `/etc/nginx/sites-available/loadbalancer`

```bash
location / {
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
}
```
Ketika sudah berhasil. Jika tidak memberikan kredensial yang benar saat membuka situs web menggunakan `lynx`, kami tidak akan mendapatkan akses ke situs web tersebut. Berikut cara masuk ke situs web menggunakan kredensial.

`lynx -auth=netics:ajka16 http://10.7.2.2/`

## Soal 11
**Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)**
Sebelum melanjutkan dengan konfigurasi, penting untuk menyelesaikan pengaturan yang diperlukan. 
Skrip Konfigurasi:
```bash
upstream worker {
    server 10.7.3.1;
    server 10.7.3.2;
    server 10.7.3.3;
}

server {
    listen 80;
    server_name granz.channel.a16.com www.granz.channel.a16.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker;
    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```
Konfigurasi ini bertujuan agar setiap permintaan yang memuat "/its" akan diteruskan ke situs https://www.its.ac.id melalui proxy_pass. Hal ini memastikan bahwa ketika akses ke endpoint yang mengandung "/its" dijalankan, ia akan langsung diarahkan oleh proxy_pass ke situs ITS.

***Testing***
Untuk melakukan pengujian, gunakan perintah berikut pada klien Revolte:
```bash
lynx www.granz.channel.a16.com/its
```


## Soal 12
**Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. (12) hint: (fixed in dulu clinetnya)**
Sebelumnya diperlukan penyiapan dasar sebelum erubah konfigurasi nginx. Kami memperbarui konfigurasi NGinx untuk membatasi akses hanya pada beerap IP tertentu dan mengatur proxy passing. Berikut adalah skripnya:
```bash
upstream worker {
    server 10.7.3.1;
    server 10.7.3.2;
    server 10.7.3.3;
}

server {
    listen 80;
    server_name granz.channel.a16.com www.granz.channel.a16.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        allow 10.7.3.69;
        allow 10.7.3.70;
        allow 10.7.4.167;
        allow 10.7.4.168;
        deny all;
        proxy_pass http://worker;
    }

    location /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Skrip di atas menetapkan bahwa hanya beberapa alamat IP spesifik yang diizinkan untuk mengakses server. Semua IP lainnya akan diblokir. Pengaturan ini dilakukan di bagian location / dalam konfigurasi Nginx.

***Testing***
Untuk memastikan konfigurasi berfungsi, akses server menggunakan salah satu IP yang diizinkan: 10.7.3.69, 10.7.3.70, 10.7.4.167, atau 10.7.4.168. Akses dari IP lain akan ditolak.

***Penyesuian konfigurasi untuk alamat IP tambahan***
Mengingat distribusi IP yang acak, kami memutuskan untuk menambahkan alamat IP tambahan, yaitu 10.7.3.19, ke dalam konfigurasi Nginx yang telah ada. Berikut adalah pembaruan pada konfigurasi:
```bash
location / {
    allow 10.7.3.69;
    allow 10.7.3.70;
    allow 10.7.3.19;  # IP baru yang ditambahkan
    allow 10.7.4.167;
    allow 10.7.4.168;
    deny all;
    proxy_pass http://worker;
}
```
Perubahan ini memungkinkan akses dari IP tambahan tersebut sambil mempertahankan pembatasan untuk IP lainnya.

***Mengatur Alamat IP Tetap pada Klien***
Sebagai alternatif, kita bisa mengatur alamat IP tetap pada klien Revolte dengan menambahkan konfigurasi berikut pada server DHCP:
```bash
host Revolte {
    hardware ethernet fe:c0:13:f5:1c:02;
    fixed-address 10.7.3.69;
}
```
Dengan cara ini, klien Revolte akan selalu menerima alamat IP 10.7.3.69, memastikan konsistensi akses tanpa perlu menyesuaikan konfigurasi Nginx secara berulang.


## Soal 13
**Karena para petualang kehabisan uang, mereka kembali bekerja untuk mengatur riegel.canyon.yyy.com.
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)**
Konfigurasi MySQL:
```bash
echo '
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Opsi yang mempengaruhi server MySQL (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf
cd /etc/mysql/mariadb.conf.d/
sed -i 's/^bind-address\s*=.*/bind-address = 0.0.0.0/' 50-server.cnf

service mysql restart
mysql -u root -p
Enter password:

CREATE USER 'kelompoka16'@'%' IDENTIFIED BY 'passworda16';
CREATE USER 'kelompoka16'@'localhost' IDENTIFIED BY 'passworda16';
CREATE DATABASE dbkelompoka16;
GRANT ALL PRIVILEGES ON *.* TO 'kelompoka16'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompoka16'@'localhost';
FLUSH PRIVILEGES;

```
***Verifikasi***
Untuk memastikan konfigurasi berjalan dengan baik, lakukan pengecekan pada salah satu worker Laravel. Sebagai contoh, kita akan memeriksa pada worker Fern dengan perintah berikut:
```bash
mariadb --host=10.7.2.1 --port=3306 --user=kelompoka16 --password=passworda16 dbkelompoka16 -e "SHOW DATABASES;"
```

## Soal 14
**Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)**
Diperlukan penginstalan composer dan git:
```bash
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer
apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update
```
Konfigurasi nginx:
```
echo 'server {
    listen <X>;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}' > /etc/nginx/sites-available/laravel-worker
```

***Konfigurasi aplikasi laravel***
```bash
cd /var/www/laravel-praktikum-jarkom && cp .env.example .env
# Tambahkan konfigurasi DB dan aplikasi ke file .env
cd /var/www/laravel-praktikum-jarkom
php artisan key:generate
php artisan config:cache
php artisan migrate
php artisan db:seed
php artisan storage:link
php artisan jwt:secret
php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
```
***Testing***
```bash
lynx localhost:[PORT]
# Ganti [PORT] dengan 8001, 8002, atau 8003 sesuai setup nginx.
```
## Soal 15
**Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/register (15)**
Sebelum melakukan testing, kita perlu menyiapkan file .json yang akan digunakan sebagai body untuk request ke endpoint /api/auth/register.
```bash
echo '
{
  "username": "kelompoka16",
  "password": "passworda16"
}' > register.json
```
***Testing***
Dari sii client Revolte, perintah Apache Benchmark dilakukan untuk testing:
```bash
ab -n 100 -c 10 -p register.json -T application/json http://10.7.4.1:8001/api/auth/register
```

## Soal 16
**POST /auth/login (16)**
Sebelum melakukan testing, kita perlu menyiapkan file .json yang akan digunakan sebagai body untuk request ke endpoint /api/auth/register.
```bash
echo '
{
  "username": "kelompoka16",
  "password": "passworda16"
}' > register.json
```
***Testing***
Dari sii client Revolte, perintah Apache Benchmark dilakukan untuk testing:
```bash
ab -n 100 -c 10 -p login.json -T application/json http://10.7.4.1:8001/api/auth/login
```


## Soal 17
**GET /me (17)**
Pertama, kelompok ami menggunakan curl untuk mengirim permintaan POST ke endpoint login Laravel dengan isi dari file login.json, menyimpan respons yang berisi token otentikasi ke dalam file login_output.txt. Kemudian, ekstrak token tersebut menggunakan jq dan simpan ke dalam variabel token. Lalu digunakna Apache Benchmark (ab) untuk mengirim 100 permintaan secara bersamaan (10 permintaan concurrent) ke endpoint /api/me server Laravel, dengan header Authorization yang menyertakan token Bearer. Tujuannya adalah untuk menilai kemampuan server dalam menangani beban permintaan dengan otentikasi.

```bash
curl -X POST -H "Content-Type: application/json" -d @login.json http://10.7.4.1:8001/api/auth/login > login_output.txt
token=$(cat login_output.txt | jq -r '.token')
```

Testing:
```bash
ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.7.4.1:8001/api/me
```


## Soal 18
**Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. (18)**
Untuk melakukan testing server Laravel menggunakan Apache Benchmark, prosesnya dimulai dengan pengambilan token autentikasi. Skrip kami menggunakan curl untuk mengirim permintaan POST ke endpoint login Laravel (http://10.7.4.1:8001/api/auth/login) dengan data yang diambil dari file login.json. Respon dari permintaan ini, yang mengandung token autentikasi, disimpan ke dalam file login_output.txt.

Skrip:
```bash
curl -X POST -H "Content-Type: application/json" -d @login.json http://10.7.4.1:8001/api/auth/login > login_output.txt
```
Kemudian token diekstrak dan disimpan dalam vairabelnya. Kemudian, Apache Benchmark digunakan untuk mengirim 100 permintaan ke endpoint /api/me dari server Laravel. Permintaan ini dilakukan dengan 10 permintaan yang dijalankan secara bersamaan (concurrent), dengan masing-masing permintaan menyertakan token autentikasi yang diperoleh sebelumnya dalam header Authorization.

Testing:
ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.7.4.1:8001/api/me


## Soal 19
**Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan**
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
**sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.(19)**
Untuk meningkatkan performa worker Laravel (Fern, Flamme, dan Frieren), kami mengimplementasikan PHP-FPM dengan berbagai konfigurasi. Proses ini melibatkan penyesuaian pengaturan PHP-FPM untuk mengoptimalkan penanganan permintaan. Empat skrip berbeda disiapkan, masing-masing dengan konfigurasi PHP-FPM yang berbeda, bertujuan untuk menilai dampak perubahan konfigurasi terhadap performa server.

```bash
# Skrip Konfigurasi PHP-FPM untuk Meningkatkan Performa Worker

# Konfigurasi 1
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3' > /etc/php/8.0/fpm/pool.d/www.conf
service php8.0-fpm restart

# Tunggu beberapa saat sebelum menjalankan konfigurasi berikutnya

# Konfigurasi 2
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

pm = dynamic
pm.max_children = 25
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10' > /etc/php/8.0/fpm/pool.d/www.conf
service php8.0-fpm restart

# Tunggu beberapa saat sebelum menjalankan konfigurasi berikutnya

# Konfigurasi 3
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

pm = dynamic
pm.max_children = 50
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 15' > /etc/php/8.0/fpm/pool.d/www.conf
service php8.0-fpm restart

# Tunggu beberapa saat sebelum menjalankan konfigurasi berikutnya

# Konfigurasi 4
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20' > /etc/php/8.0/fpm/pool.d/www.conf
service php8.0-fpm restart

```
Setiap skrip ini akan diuji dengan Apache Benchmark, mengirim 100 request dengan 10 request/detik, untuk menganalisis perbedaan performa antara konfigurasi ini. Analisis ini akan membantu dalam menentukan konfigurasi mana yang paling efektif untuk penanganan beban permintaan pada server. Hasil dari setiap konfigurasi ini akan memberikan data yang penting untuk memahami bagaimana penyesuaian PHP-FPM dapat mempengaruhi kinerja server dalam kondisi beban berbeda.


## Soal 20
**Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)**

skrip konfigurasi:
```
# Konfigurasi PHP-FPM pada Worker
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

# Konfigurasi Least-Connection pada Load Balancer Nginx
echo 'upstream worker {
    least_conn;
    server 10.7.4.1:8001;
    server 10.7.4.2:8002;
    server 10.7.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.a16.com www.riegel.canyon.a16.com;

    location / {
        proxy_pass http://worker;
    }
}' > /etc/nginx/sites-available/laravel-worker

service nginx restart

```

Peningkatan performa worker Laravel tidak hanya bergantung pada konfigurasi PHP-FPM, tetapi juga pada strategi load balancing yang digunakan. Dalam kasus ini, algoritma Least-Connection diimplementasikan pada load balancer Eisen. Algoritma ini bertujuan untuk mendistribusikan beban kerja secara lebih efisien dengan memprioritaskan server yang memiliki jumlah koneksi aktif paling sedikit. Dengan demikian, setiap request baru akan dialihkan ke server yang paling sedikit beban kerjanya, mengurangi kemungkinan beberapa server mengalami beban tinggi sementara yang lainnya kurang dimanfaatkan.

Konfigurasi nginx untuk implementasi Least-Connection pada load balancer Eisen dilakukan dengan menambahkan least_conn pada blok upstream dalam file konfigurasi nginx. Server-server yang tersedia dalam kluster (10.7.4.1:8001, 10.7.4.2:8002, dan 10.7.4.3:8003) diatur di bawah directive least_conn, yang memungkinkan nginx untuk mengalokasikan request ke server dengan jumlah koneksi terkecil. Setelah konfigurasi ini diterapkan, nginx di-restart untuk memastikan perubahan diterapkan.

Konfigurasi ini masih tetap menggunakan pengaturan PHP-FPM yang sebelumnya telah ditentukan, termasuk pm.max_children = 75, pm.start_servers = 10, pm.min_spare_servers = 5, dan pm.max_spare_servers = 20. Pengaturan ini, bersama dengan strategi load balancing yang baru, diharapkan dapat meningkatkan performa keseluruhan dari kluster server.

Dengan penggunaan Least-Connection di load balancer, setiap server dalam kluster memiliki peluang yang lebih seimbang untuk menangani permintaan, mencegah situasi di mana beberapa server mungkin kelebihan beban sementara yang lainnya kurang dimanfaatkan.


## Kendala 
Menghadapi kendala kapasitas CPU yang terbatas menjadi tantangan utama bagi kelompok kami selama pengerjaan praktikum dan fase testing kode. Masalah ini terutama berakar pada keterbatasan hardware yang kami gunakan, dimana CPU seringkali tidak mampu menangani beban kerja yang dibutuhkan untuk tugas-tugas yang kompleks. Akibatnya, kami mengalami berbagai error yang berulang kali terjadi selama proses pengembangan dan pengujian, yang sangat mengganggu kelancaran pengerjaan.

Dampak dari keterbatasan CPU ini cukup signifikan. Salah satunya adalah kebutuhan untuk sering kali memindahkan pekerjaan dari satu laptop ke laptop lain yang memiliki spesifikasi lebih mampu. Proses ini tidak hanya menyita waktu, tetapi juga menimbulkan tantangan tambahan seperti perbedaan lingkungan pengembangan antara dua perangkat. Inkonsistensi ini bisa menghasilkan perbedaan dalam hasil testing atau bahkan kesalahan dalam kompatibilitas kode.

Kendala kapasitas CPU ini juga membatasi kemampuan kami untuk melakukan eksperimen dengan berbagai skenario aplikasi, terutama dalam situasi yang memerlukan penggunaan sumber daya komputasi yang tinggi. Hal ini mengurangi efektivitas pengujian kami dan membatasi pemahaman kami tentang performa aplikasi di bawah kondisi beban kerja yang beragam.
