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

## Soal 8

## Soal 9

## Soal 10

## Soal 11

## Soal 12

## Soal 13

## Soal 14

## Soal 15

## Soal 16

## Soal 17

## Soal 18

## Soal 19

## Soal 20
