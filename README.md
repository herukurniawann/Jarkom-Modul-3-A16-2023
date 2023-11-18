# Jarkom-Modul-2-A16-2023
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

**Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.**

Pastikan domain riegel.canyon.a16.com diarahkan ke worker Laravel bernama Fern dengan alamat IP tetap `192.184.4.1`. Selain itu, atur domain `granz.channel.b12.com` agar mengarah pada worker PHP yang disebut Lugner dengan alamat IP tetap `192.184.3.1.` Semua klien harus mengadopsi konfigurasi dari DHCP Server agar terhubung dengan lancar.

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

## (1) Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

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

**Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)**
**Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)**
**Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)**
**Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)**


Node Himmel 

dhcp.conf
max-lease-time 7200;

ddns-update-style none;

subnet 10.7.1.0 netmask 255.255.255.0 {
}

subnet 10.7.2.0 netmask 255.255.255.0 {
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



script.sh
echo nameserver 192.168.122.1 > /etc/resolv.conf


apt-get update
wait
apt-get install isc-dhcp-server -y
wait

echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

cp ~/dhcpd.conf /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart


Berjalannya waktu, petualang diminta untuk melakukan deployment.
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

	Lawine 
	script.sh
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install mariadb-client -y

apt-get update
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer
apt-get install git -y

apt-get install wget -y

service php8.0-fpm start

#wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O modul-3

cp -r  ~/modul-3 /var/www/modul-3

rm -r /etc/nginx
cp -r ~/nginx /etc
ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
service nginx restart

service nginx start


Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

Node Eisen
script.sh

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update && apt-get install nginx -y

rm -r /etc/nginx
cp -r ~/nginx /etc/nginx

service nginx start

# Soal 10
apt-get install apache2-utils -y
mkdir /etc/nginx/rahasisakita
htpasswd -b -c /etc/nginx/rahasisakita/htpasswd netics ajka16

ln -s /etc/nginx/sites-available/riegel.canyon.a16.com /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/granz.channel.a16.com /etc/nginx/sites-enabled/

service nginx restart








nginx
nginx >> sites-available

granz.channel.a16.com
upstream web {
   # no load balancing method is specified for Round Robin
   server 10.7.3.1;
   server 10.7.3.2;
   server 10.7.3.3;
}


server {
        listen 80;
        server_name grenz.channel.a16.com;

        location ^~ /its {
                proxy_pass https://www.its.ac.id;
        }

        location / {
                auth_basic "Restricted Access";
                auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;

                proxy_pass http://web;
        }
        # Batasan akses berdasarkan alamat IP
        allow 10.7.3.69;
        allow 10.7.3.70;
        allow 10.7.4.167;
        allow 10.7.4.168;
        deny all;
}


riegel.canyon.a16.com
server {
        listen 80;
        server_name riegel.canyon.a16.com;

        location /api/auth/register {
                proxy_pass http://10.7.4.1;
        }

        location /api/auth/login {
                proxy_pass http://10.7.4.2;
        }

        location /api/me {
                proxy_pass http://10.7.4.3;
        }

        location / {
                proxy_pass http://10.7.4.1;
        }
}


3. Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
Nama Algoritma Load Balancer
Report hasil testing pada Apache Benchmark
Grafik request per second untuk masing masing algoritma. 
Analisis (8)

4. Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)


5. Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ (10)





















