# Jarkom-Modul-3-B20-2023

## Informasi Kelompok

| Nama | NRP |
| ---- | --- |
| Richie Seputro | 5025211213 |
| Dimas Aria Pujangga | 5025211212 |

## Topologi
![topologi](assets/topologi.png)

## Config
- **Aura (DHCP Relay)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.188.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.188.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.188.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.188.4.0
	netmask 255.255.255.0

```
- **Himmel (DHCP Server)**
```
auto eth0
iface eth0 inet static
	address 192.188.1.1
	netmask 255.255.255.0
	gateway 192.188.1.0

```
- **Heiter (DNS Server)**
```
auto eth0
iface eth0 inet static
	address 192.188.1.2
	netmask 255.255.255.0
	gateway 192.188.1.0
```
- **Denken (Database Server)**
```
auto eth0
iface eth0 inet static
	address 192.188.2.1
	netmask 255.255.255.0
	gateway 192.188.2.0
```
- **Eisen (Load Balancer)**
```
auto eth0
iface eth0 inet static
	address 192.188.2.2
	netmask 255.255.255.0
	gateway 192.188.2.0
```
- **Frieren (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.18.4.3
	netmask 255.255.255.0
	gateway 192.188.4.0
```
- **Flamme (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.4.2
	netmask 255.255.255.0
	gateway 192.188.4.0
```
- **Fern (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.4.1
	netmask 255.255.255.0
	gateway 192.188.4.0
```
- **Lawine (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.3.3
	netmask 255.255.255.0
	gateway 192.188.3.0
```
- **Linie (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.3.2
	netmask 255.255.255.0
	gateway 192.188.3.0
```
- **Lugner (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.3.1
	netmask 255.255.255.0
	gateway 192.188.3.0
```
- **Revolte, Richter, Sein, dan Stark (Client)**
```
auto eth0
iface eth0 inet dhcp
```

## Soal 1 
>Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

- **Lugner (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.3.1
	netmask 255.255.255.0
	gateway 192.188.3.0
```
- **Fern (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.188.4.1
	netmask 255.255.255.0
	gateway 192.188.4.0
```

Selanjutnya pada DNS Server (Heiter), kita perlu menjalankan command dibawah ini

### Script
```
echo 'zone "riegel.canyon.b20.com" {
    type master;
    file "/etc/bind/sites/riegel.canyon.b20.com";
};

zone "granz.channel.b20.com" {
    type master;
    file "/etc/bind/sites/granz.channel.b20.com";
};

zone "1.188.192.in-addr.arpa" {
    type master;
    file "/etc/bind/sites/1.188.192.in-addr.arpa";
};' > /etc/bind/named.conf.local
```

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.b20.com. root.riegel.canyon.b20.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.b20.com.
@       IN      A       192.188.4.1     ; IP Fern


; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.b20.com. root.granz.channel.b20.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.b20.com.
@       IN      A       192.188.3.1     ; IP Lugner

      options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options
```

### Testing

![1b](assets/1b.png)

## Soal 2

>Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

DHCP Server
### Script 
```
subnet 192.188.1.0 netmask 255.255.255.0 {
}

subnet 192.188.2.0 netmask 255.255.255.0 {
}

subnet 192.188.3.0 netmask 255.255.255.0 {
    range 192.188.3.16 192.188.3.32;
    range 192.188.3.64 192.188.3.80;
    option routers 192.188.3.0;
} > /etc/dhcp/dhcpd.conf
```

## Soal 3 
>Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

Selanjutnya kita perlu menambahkan beberapa konfigurasi baru untuk switch4 dengan menjalankan command dibawah ini

### Script 
```
subnet 192.188.1.0 netmask 255.255.255.0 {
}

subnet 192.188.2.0 netmask 255.255.255.0 {
}

subnet 192.188.3.0 netmask 255.255.255.0 {
    range 192.188.3.16 10.17.3.32;
    range 192.188.3.64 10.17.3.80;
    option routers 192.188.3.244;
}

subnet 192.188.4.0 netmask 255.255.255.0 {
    range 192.188.4.12 192.188.4.20;
    range 192.188.4.160 192.188.4.168;
    option routers 192.188.4.244;
}  > /etc/dhcp/dhcpd.conf
```

## Soal 4 
>Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

kita akan menambahkan beberapa konfigurasi seperti ``option broadcast-address`` dan ``option domain-name-server`` agar dapat DNS yang telah disiapkan sebelumnya dapat digunakan

``` 
subnet 192.188.1.0 netmask 255.255.255.0 {
}

subnet 192.188.2.0 netmask 255.255.255.0 {
}

subnet 192.188.3.0 netmask 255.255.255.0 {
    range 192.188.3.16 10.17.3.32;
    range 192.188.3.64 10.17.3.80;
    option routers 192.188.3.244;
    option broadcast-address 192.188.3.255;
    option domain-name-servers 192.188.1.2;
}

subnet 192.188.4.0 netmask 255.255.255.0 {
    range 192.188.4.12 192.188.4.20;
    range 192.188.4.160 192.188.4.168;
    option routers 192.188.4.244;
    option broadcast-address 192.188.4.255;
    option domain-name-servers 192.188.1.2;
}  > /etc/dhcp/dhcpd.conf
```

## Soal 5
>Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Kita perlu menggunakan bantuan fungsi ``default-lease-time`` dan ``max-lease-team`` dimana satuannya adalah detik.

Karena pada ``switch3`` dapat meminjamkan IP selama ``3 Menit`` dan ``Switch4`` dapat meminjamkan IP selama ``12 Menit``. Sehingga pada ``Switch3`` membutuhkan waktu ``180 s`` dan ``Switch4`` membutuhkan waktu ``720 s`` dan untuk ``max-lease-time`` nya adalah ``96 menit`` dimana akan menjadi ``5760 s``
 Selanjutnya kita perlu menambahkan beberapa konfigurasi baru untuk mengatur leasing time pada switch3 dan switch4 sesuai dengan aturan soal. Kita dapat menjalankan command berikut pada DHCP Server

### Script
```
subnet 192.188.1.0 netmask 255.255.255.0 {
}

subnet 192.188.2.0 netmask 255.255.255.0 {
}

subnet 192.188.3.0 netmask 255.255.255.0 {
    range 192.188.3.16 10.17.3.32;
    range 192.188.3.64 10.17.3.80;
    option routers 192.188.3.244;
    option broadcast-address 192.188.3.255;
    option domain-name-servers 192.188.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.188.4.0 netmask 255.255.255.0 {
    range 192.188.4.12 192.188.4.20;
    range 192.188.4.160 192.188.4.168;
    option routers 192.188.4.244;
    option broadcast-address 192.188.4.255;
    option domain-name-servers 192.188.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
```

## Soal 6
> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu pada **seluruh PHP Worker**. Jika sudah, silahkan untuk melakukan konfigurasi tambahan sebagai berikut untuk melakukan download dan unzip menggunakan command ``wget``
```sh
wget -O '/var/www/granz.channel.b20.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'
unzip -o /var/www/granz.channel.b20.com -d /var/www/
rm /var/www/granz.channel.b20.com
mv /var/www/modul-3 /var/www/granz.channel.b20.com
```

### Script
Setelah melakukan download dan unzip. Sekarang kita bisa melakukan konfigurasi pada ``nginx`` sebagai berikut:
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.b20.com
ln -s /etc/nginx/sites-available/granz.channel.b20.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

    server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.b20.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.b20.com

service nginx restart
```

### Result 
Jalanin Perintah ``lynx localhost`` pada masing-masing worker dan hasilnya akan sebagai berikut:

![6a](assets/6a.png)

## Soal 7
> Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah melakukan konfigurasi diatas, sekarang lakukan konfigurasi ``Load Balancing`` pada node ``Eisen`` sebagai berikut 

### Script
Sebelum melakukan setup soal 7. Buka kembali Node ``DNS Server`` dan arahkan ``domain`` tersebut pada ``IP Load Balancer Eisen``

``` 
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.b20.com. root.riegel.canyon.b20.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.b20.com.
@       IN      A       192.188.2.2     ; IP LB Eiken
www     IN      CNAME   riegel.canyon.b20.com.' > /etc/bind/sites/riegel.canyon.b20.com
```

### Result

![7a](assets/7a.png)

8. Richie

9. Richie

10. Richie

11. Richie

12. Richie

13. Richie

14. Richie

15. gtw

16. gtw

17. gtw

18. gtw

19. gtw

20. gtw
