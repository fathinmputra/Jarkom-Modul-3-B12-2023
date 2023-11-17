# Praktikum Jaringan Komputer
# Modul 2
### Kelompok B12
### Anggota :
|            Nama           |     NRP    |
| ------                    | ------     |
| Darvin Exaudi Simanjuntak | 5025211172 |
| Fathin Muhashibi Putra    | 5025211229 |

## NO. 1
Perjalanan selanjutnya akan menggunakan peta berikut:

<img width="512" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/446104af-df50-4b19-b676-ead6a45f2fc2">

dengan ketentuan sebagai berikut:

<img width="386" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/2e73856d-46a5-4246-b14d-9a72d4398393">

Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.

Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

### Penjelasan :

Berikut merupakan Topologi yang sudah kami Buat :

<img width="556" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/709b9b7b-fe14-4a5f-8e15-eaea9c7aa10f">

### Network Configuration :
**Aura (Router (DHCP Relay))**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.184.1.55
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.184.2.55
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.184.3.55
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.184.4.55
	netmask 255.255.255.0
```

**Himmel (DHCP Server)**
```
auto eth0
iface eth0 inet static
	address 192.184.1.2
	netmask 255.255.255.0
	gateway 192.184.1.55
```

**Heiter (DNS Server)**
```
auto eth0
iface eth0 inet static
	address 192.184.1.3
	netmask 255.255.255.0
	gateway 192.184.1.55
```

**Denken (Database Server)**
```
auto eth0
iface eth0 inet static
	address 192.184.2.2
	netmask 255.255.255.0
	gateway 192.184.2.55
```

**Eisen (Load Balancer)**
```
auto eth0
iface eth0 inet static
	address 192.184.2.3
	netmask 255.255.255.0
	gateway 192.184.2.55
```

**Frieren (Laravel Worker)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.4.3
#	netmask 255.255.255.0
#	gateway 192.184.4.55

auto eth0
iface eth0 inet dhcp
hwaddress ether e6:7f:04:bf:d4:1e
```

**Flamme (Laravel Worker)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.4.2
#	netmask 255.255.255.0
#	gateway 192.184.4.55

auto eth0
iface eth0 inet dhcp
hwaddress ether 5e:36:a7:43:d1:e9
```

**Fern (Laravel Worker)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.4.1
#	netmask 255.255.255.0
#	gateway 192.184.4.55

auto eth0
iface eth0 inet dhcp
hwaddress ether ea:32:95:1e:2d:1d
```

**Lawine (PHP Worker)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.3.3
#	netmask 255.255.255.0
#	gateway 192.184.3.55

auto eth0
iface eth0 inet dhcp
hwaddress ether 5a:42:07:b4:5e:34

```

**Linie (PHP Worker)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.3.2
#	netmask 255.255.255.0
#	gateway 192.184.3.55

auto eth0
iface eth0 inet dhcp
hwaddress ether fa:14:32:dd:9d:9f
```

**Lugner (PHP Worker)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.3.1
#	netmask 255.255.255.0
#	gateway 192.184.3.55

auto eth0
iface eth0 inet dhcp
hwaddress ether b6:e1:e0:47:a0:2c
```

**Revolte (Client)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.3.5
#	netmask 255.255.255.0
#	gateway 192.184.3.55

auto eth0
iface eth0 inet dhcp
```

**Richter (Client)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.3.4
#	netmask 255.255.255.0
#	gateway 192.184.3.55

auto eth0
iface eth0 inet dhcp
```

**Sein (Client)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.4.5
#	netmask 255.255.255.0
#	gateway 192.184.4.55

auto eth0
iface eth0 inet dhcp
```

**Stark (Client)**
```
#auto eth0
#iface eth0 inet static
#	address 192.184.4.4
#	netmask 255.255.255.0
#	gateway 192.184.4.55

auto eth0
iface eth0 inet dhcp
```

### Register domain :
Register domain berupa `riegel.canyon.b12.com` untuk worker Laravel, yang mengarah pada `Fern (worker php)` yang memiliki fixed IP address `192.184.4.1` dan Register domain berupa granz.channel.b12.com untuk worker PHP, yang mengarah pada `Lugner (worker laravel)` yang memiliki fixed IP address `192.184.3.1` :

**Heiter (DNS Server)**
Disimpan `script.sh` pada root, berisi :
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils
apt-get install isc-dhcp-server
apt-get install bind9
apt-get install nginx

cp named.conf.local /etc/bind/named.conf.local

mkdir /etc/bind/riegel/
cp riegel.canyon.b12.com /etc/bind/riegel/riegel.canyon.b12.com
service bind9 restart

mkdir /etc/bind/granz/
cp granz.channel.b12.com /etc/bind/granz/granz.channel.b12.com
cp 1.184.192.in-addr.arpa.granz /etc/bind/granz/1.184.192.in-addr.arpa
service bind9 restart
```

Kemudian, `bash script.sh` sehingga :

Isi dari `/etc/bind/named.conf.local` :
```
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "riegel.canyon.b12.com" {
        type master;
        file "/etc/bind/riegel/riegel.canyon.b12.com";
};

zone "granz.channel.b12.com" {
        type master;
        file "/etc/bind/granz/granz.channel.b12.com";
};

zone "1.184.192.in-addr.arpa" {
    type master;
    file "/etc/bind/granz/1.184.192.in-addr.arpa";
};
```

Isi dari `/etc/bind/riegel/riegel.canyon.b12.com` :
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.b12.com. root.riegel.canyon.b12.com. (
                        2023111301    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.b12.com.
@               IN      A       192.184.4.1 ; IP Fern
www             IN      CNAME   riegel.canyon.b12.com.
heimmel         IN      A       192.184.1.2 ; IP Heimmel
heiter          IN      A       192.184.1.3 ; IP Heiter
denken          IN      A       192.184.2.2 ;   IPDenken
eisen           IN      A       192.184.2.3 ; IP Eisen
```

Isi dari `/etc/bind/granz/granz.channel.b12.com` :
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.b12.com. root.granz.channel.b12.com. (
                        2023111301    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      granz.channel.b12.com.
@               IN      A       192.184.3.1 ; IP Lugner
www             IN      CNAME   granz.channel.b12.com.
heimmel         IN      A       192.184.1.2 ; IP Heimmel
heiter          IN      A       192.184.1.3 ; IP Heiter
denken          IN      A       192.184.2.2 ;   IPDenken
eisen           IN      A       192.184.2.3 ; IP Eisen
```

Isi dari `/etc/bind/granz/1.184.192.in-addr.arp` :
```
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.b12.com. root.granz.channel.b12.com. (
                        2023111301    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
1.184.192.in-addr.arpa.         IN      NS      granz.channel.b12.com.
3                               IN      PTR     granz.channel.b12.com.
```
### Screenshot hasil:
ping granz.channel.b12.com pada client `Revolte` :

<img width="288" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/75a4180f-373e-4c9f-93ee-c087141e9e40">


ping riegel.canyon.b12.com pada client `sein` :

<img width="284" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/128b155d-a6f2-4299-b95f-032fa01f5028">

## NO. 2, 3, 4, dan 5
Kemudian, karena masih banyak spell yang harus dikumpulkan, bantulah para petualang untuk memenuhi kriteria berikut.:
### 1. Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
#### Penjelasan : 
Kita perlu mengatur DHCP Server (Heiter) dan DHCP Relay (Aura) terlebih dahulu.

**Himmel (DHCP Server)** :
Disimpan `script.sh` pada root, berisi :
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y

cp isc-dhcp-server /etc/default/isc-dhcp-server
cp dhcpd.conf /etc/dhcp/dhcpd.conf

service isc-dhcp-server start
service isc-dhcp-server restart
```
Kemudian, `bash script.sh` pada sehingga :

Isi dari `/etc/default/isc-dhcp-server` :
```
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
**Keterangan :**
INTERFACESv4 diisi dengan `eth0` karena `Himmel` melalui `eth0`.


Isi dari `/etc/dhcp/dhcpd.conf` :
```
subnet 192.184.1.0 netmask 255.255.255.0 {
}

subnet 192.184.2.0 netmask 255.255.255.0 {
}

subnet 192.184.3.0 netmask 255.255.255.0 {
    range 192.184.3.16 192.184.3.32;
    range 192.184.3.64 192.184.3.80;
    option routers 192.184.3.55;
    option broadcast-address 192.184.3.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.184.4.0 netmask 255.255.255.0 {
    range 192.184.4.12 192.184.4.20;
    range 192.184.4.160 192.184.4.168;
    option routers 192.184.4.55;
    option broadcast-address 192.184.4.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}

host Lugner {
    hardware ethernet b6:e1:e0:47:a0:2c;
    fixed-address 192.184.3.1;
}

host Linie {
    hardware ethernet fa:14:32:dd:9d:9f;
    fixed-address 192.184.3.2;
}

host Lawine {
    hardware ethernet 5a:42:07:b4:5e:34;
    fixed-address 192.184.3.3;
}


host Fern {
    hardware ethernet ea:32:95:1e:2d:1d;
    fixed-address 192.184.4.1;
}

host Flamme {
    hardware ethernet 5e:36:a7:43:d1:e9;
    fixed-address 192.184.4.2;
}


host Frieren {
    hardware ethernet e6:7f:04:bf:d4:1e;
    fixed-address 192.184.4.3;
}
```
**Keterangan :**
Bagian berikut digunakan untuk mengatur DHCP Server :
```
subnet 192.184.1.0 netmask 255.255.255.0 {
}

subnet 192.184.2.0 netmask 255.255.255.0 {
}

subnet 192.184.3.0 netmask 255.255.255.0 {
    range 192.184.3.16 192.184.3.32;
    range 192.184.3.64 192.184.3.80;
    option routers 192.184.3.55;
    option broadcast-address 192.184.3.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.184.4.0 netmask 255.255.255.0 {
    range 192.184.4.12 192.184.4.20;
    range 192.184.4.160 192.184.4.168;
    option routers 192.184.4.55;
    option broadcast-address 192.184.4.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}
```
- `subnet 'NID'` merupakan Network ID pada subnet interface.
- `netmask 'Netmask` merupakan Netmask pada subnet.
- `range 'IP_Awal' 'IP_Akhir'` merupakan rentang IP Address yang akan didistribusikan dan digunakan secara dinamis.
- `option routers 'Gateway'` merupakan IP gateway dari router menuju client sesuai konfigurasi subnet.
- `option broadcast-address 'IP_Broadcast'` merupakan IP broadcast pada subnet.
- `option domain-name-servers 'DNS_yang_diinginkan'` merupakan DNS yang ingin kita berikan pada client.
- `Lease time` merupakan waktu yang dialokasikan ketika sebuah IP dipinjamkan kepada komputer client.
- `default-lease-time 'Waktu'` merupakan lama waktu DHCP server meminjamkan IP Address kepada client, dalam satuan detik.
- `max-lease-time 'Waktu'` merupakan waktu maksimal yang di alokasikan untuk peminjaman IP oleh DHCP server ke client dalam satuan detik.


Bagian berikut digunakan untuk mengatur `fixed address` dari tiap worker PHP dan worker Laravel :
```
host Lugner {
    hardware ethernet b6:e1:e0:47:a0:2c;
    fixed-address 192.184.3.1;
}

host Linie {
    hardware ethernet fa:14:32:dd:9d:9f;
    fixed-address 192.184.3.2;
}

host Lawine {
    hardware ethernet 5a:42:07:b4:5e:34;
    fixed-address 192.184.3.3;
}

host Fern {
    hardware ethernet ea:32:95:1e:2d:1d;
    fixed-address 192.184.4.1;
}

host Flamme {
    hardware ethernet 5e:36:a7:43:d1:e9;
    fixed-address 192.184.4.2;
}

host Frieren {
    hardware ethernet e6:7f:04:bf:d4:1e;
    fixed-address 192.184.4.3;
}
```


**Aura (DHCP Relay)** :
Disimpan `script.sh` pada root, berisi :
```
apt-get update
apt-get install isc-dhcp-relay -y

cp isc-dhcp-relay /etc/default/isc-dhcp-relay
cp sysct1.conf /etc/sysctl.conf

service isc-dhcp-relay start
service isc-dhcp-relay restart
```

Kemudian, `bash script.sh` pada sehingga :

Isi dari `/etc/default/isc-dhcp-relay` :
```
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.184.1.2"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
**Keterangan :**
SERVERS diisi dengan IP `192.184.1.2` yang merupakan IP Address dari DHCP Server. Sedangkan, INTERFACES diisi dengan `eth1 eth2 eth3 eth4` yang mana merupakan cabang yang terhubung dengan DHCP Relay (Aura).

Isi dari `/etc/sysctl.conf` :
```
net.ipv4.ip_forward=1
```
**Keterangan :**
Konfigurasi tersebut digunakan untuk mengaktifkan IP Forwarding


### 2. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)
#### Penjelasan : 
Pada Himmel (DHCP Server) `/etc/dhcp/dhcpd.conf`  diatur pada bagian ini :
```
subnet 192.184.3.0 netmask 255.255.255.0 {
    range 192.184.3.16 192.184.3.32;
    range 192.184.3.64 192.184.3.80;
    option routers 192.184.3.55;
    option broadcast-address 192.184.3.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}
```

**Keterangan :**
```
    range 192.184.3.16 192.184.3.32;
    range 192.184.3.64 192.184.3.80;
```
Range pertama, yaitu `range 192.184.3.16 192.184.3.32`. Range kedua, yaitu `range 192.184.3.64 192.184.3.80`


### 3. Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)
#### Penjelasan : 
Pada Himmel (DHCP Server) `/etc/dhcp/dhcpd.conf`  diatur pada bagian ini :
```
subnet 192.184.4.0 netmask 255.255.255.0 {
    range 192.184.4.12 192.184.4.20;
    range 192.184.4.160 192.184.4.168;
    option routers 192.184.4.55;
    option broadcast-address 192.184.4.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}
```

**Keterangan :**
```
    range 192.184.4.12 192.184.4.20;
    range 192.184.4.160 192.184.4.168;
```
Range pertama, yaitu `range 192.184.4.12 192.184.4.20`. Range kedua, yaitu `range 192.184.4.160 192.184.4.168`


### 4. Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)
#### Penjelasan : 
Pada Himmel (DHCP Server) `/etc/dhcp/dhcpd.conf`  diatur pada bagian ini :
```
subnet 192.184.3.0 netmask 255.255.255.0 {
    range 192.184.3.16 192.184.3.32;
    range 192.184.3.64 192.184.3.80;
    option routers 192.184.3.55;
    option broadcast-address 192.184.3.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.184.4.0 netmask 255.255.255.0 {
    range 192.184.4.12 192.184.4.20;
    range 192.184.4.160 192.184.4.168;
    option routers 192.184.4.55;
    option broadcast-address 192.184.4.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}
```

**Keterangan :**
Pada bagian `option domain-name-servers` diisi dengan IP `192.184.1.3` yang merupakan IP Address dari `Heiter (DNS Server)`.

### 5. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)
#### Penjelasan : 
Pada Himmel (DHCP Server) `/etc/dhcp/dhcpd.conf`  diatur pada bagian ini :
```
subnet 192.184.3.0 netmask 255.255.255.0 {
    range 192.184.3.16 192.184.3.32;
    range 192.184.3.64 192.184.3.80;
    option routers 192.184.3.55;
    option broadcast-address 192.184.3.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}
```
**Keterangan :**
Pada bagian `default-lease-time` diisi dengan `180` (detik) atau  sama dengan 3 menit, yang merupakan lama waktu DHCP Sever meminjam alamat IP kepada client yang memlaui Switch3. Sedangakan, pada bagian `max-lease-time` diisi dengan `5760` (detik) atau sam dengan 96 menit yang merupakan waktu maksimal dialokasikan untuk peminjaman alamat IP.

```
subnet 192.184.4.0 netmask 255.255.255.0 {
    range 192.184.4.12 192.184.4.20;
    range 192.184.4.160 192.184.4.168;
    option routers 192.184.4.55;
    option broadcast-address 192.184.4.255;
    option domain-name-servers 192.184.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}
```

**Keterangan :**
Pada bagian `default-lease-time` diisi dengan `720` (detik) atau  sama dengan 12 menit, yang merupakan lama waktu DHCP Sever meminjam alamat IP kepada client yang memlaui Switch4. Sedangakan, pada bagian `max-lease-time` diisi dengan `5760` (detik) atau sam dengan 96 menit yang merupakan waktu maksimal dialokasikan untuk peminjaman alamat IP.


## NO. 6
Berjalannya waktu, petualang diminta untuk melakukan deployment.
### 1. Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)
#### Penjelasan : 

**Lugner :**
Buat `script.sh` pada root berisi :
```
cp resolv.conf1 /etc/resolv.conf

apt-get update 
apt-get install nginx -y
apt-get install php -y
apt-get install php php-fpm
apt-get install htop -y

cp index.php /var/www/html/index.php
cp info.php /var/www/html/info.php

mkdir /var/www/html/css
mkdir /var/www/html/js

cp style.css /var/www/html/css/style.css
cp script.js /var/www/html/js/script.js

cp default /etc/nginx/sites-available/default

cp resolv.conf2 /etc/resolv.conf

service nginx start
service php7.3-fpm start

service nginx restart
service php7.3-fpm restart
```
Kemudian `bash script.sh` tersebut.

Sehingga, 
- Isi dari `resolv.conf1`, yaitu `nameserver 192.168.122.1` yang akan di copy ke `/etc/resolv.conf`.
- Isi dari `resolv.conf2`, yaitu `nameserver 192.184.1.3` yang akan di copy ke `/etc/resolv.conf`.
  
- Isi dari `/var/www/html/index.php`, `/var/www/html/info.php`, `/var/www/html/css/style.css`, dan `/var/www/html/js/script.js` didapatkan dari `https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing` yang ada pada soal. 

- Isi dari `/etc/nginx/sites-available/default` :
```
server {
        listen 80;
	root /var/www/html;
        index index.html index.htm index.php index.nginx-debian.html;

        server_name granz.channel.b12.com;

        location / {
                try_files $uri $uri/ =404;
        }

	location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        }

	location /app1/ {
        # Lugner
        proxy_bind 192.184.3.1;
        proxy_pass http://192.184.3.1/index.php;
	}

	location /app2/ {
        # Linie
        proxy_bind 192.184.3.1;
        proxy_pass http://192.184.3.2/index.php;
	}

	location /app3/ {
        # Lawine
        proxy_bind 192.184.3.1;
        proxy_pass http://192.184.3.3/index.php;
	}
}
```

**Linie dan Lewine:**
Buat `script.sh` pada root berisi :
```
cp resolv.conf1 /etc/resolv.conf

apt-get update 
apt-get install nginx -y
apt-get install php -y
apt-get install php php-fpm
apt-get install htop -y

cp index.php /var/www/html/index.php
cp info.php /var/www/html/info.php

mkdir /var/www/html/css
mkdir /var/www/html/js

cp style.css /var/www/html/css/style.css
cp script.js /var/www/html/js/script.js

cp default /etc/nginx/sites-available/default

cp resolv.conf2 /etc/resolv.conf

service nginx start
service php7.3-fpm start

service nginx restart
service php7.3-fpm restart
```
Kemudian `bash script.sh` tersebut.

Sehingga, 
- Isi dari `resolv.conf1`, yaitu `nameserver 192.168.122.1` yang akan di copy ke `/etc/resolv.conf`.
- Isi dari `resolv.conf2`, yaitu `nameserver 192.184.1.3` yang akan di copy ke `/etc/resolv.conf`.
  
- Isi dari `/var/www/html/index.php`, `/var/www/html/info.php`, `/var/www/html/css/style.css`, dan `/var/www/html/js/script.js` didapatkan dari `https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing` yang ada pada soal. 

- Isi dari `/etc/nginx/sites-available/default` :
```
server {
        listen 80;
	root /var/www/html;
        index index.html index.htm index.php index.nginx-debian.html;

        server_name granz.channel.b12.com;

        location / {
                try_files $uri $uri/ =404;
        }

	location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        }
}
```

**Revolte (Client) :**
Buat `script.sh` yang berisi :
```
cp resolv.conf2 /etc/resolv.conf
apt update
apt-get install lynx -y
apt-get update
apt-get install apache2-utils
apt install less -y
cp resolv.conf1 /etc/resolv.conf
```
Setelah itu, `bash script.sh`.
Kemudian, lakukan testing pada Client Revolte.

**Screenshot Hasil :**

**Revolte :**
Testing pada `Client Revolte` :
```
lynx granz.channel.b12.com/app1
```

<img width="406" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/9e7b226d-f619-440d-8365-18d8991e847e">

Keterangan : Mengakses virtual host website Lugner.


```
lynx granz.channel.b12.com/app2
```

<img width="408" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/f3ccbfa5-874f-4b8f-860e-6b3a482ca27a">

Keterangan : Mengakses virtual host website Linie.


```
lynx granz.channel.b12.com/app3
```
 
<img width="410" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/44393069-1873-432a-b267-f7a6c49e91cf">

Keterangan : Mengakses virtual host website Lawine.




## NO. 7
Berjalannya waktu, petualang diminta untuk melakukan deployment.
### Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
- a. Lawine, 4GB, 2vCPU, dan 80 GB SSD.
- b. Linie, 2GB, 2vCPU, dan 50 GB SSD.
- c. Lugner 1GB, 1vCPU, dan 25 GB SSD.

aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

#### Penjelasan : 
**Worker PHP (Lugner, Linie, dan Lawine) :**
Pertama-tama buat `script.sh` pada seluruh Worker PHP (Lugner, Linie, dan Lawine) yang berisi :
```
cp granz /etc/nginx/sites-available/granz

mkdir /var/www/granz

cp index.php /var/www/granz/index.php
cp info.php /var/www/granz/info.php

mkdir /var/www/granz/css
mkdir /var/www/granz/js

cp style.css /var/www/granz/css/style.css
cp script.js /var/www/granz/js/script.js

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/granz /etc/nginx/sites-enabled

service nginx restart
service php7.3-fpm restart
```

Kemudian `bash script.sh` tersebut.

Sehingga, 
- Isi dari `/etc/nginx/sites-available/granz`:
```
server {

listen 80;

root /var/www/granz;

index index.php index.html index.htm;
server_name _;

location / {
        try_files $uri $uri/ /index.php?$query_string;
}

# pass PHP scripts to FastCGI server
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
}

location ~ /\.ht {
        deny all;
}

error_log /var/log/nginx/granz_error.log;
access_log /var/log/nginx/granz_access.log;
}
```

- Isi dari `/var/www/granz/index.php`, `/var/www/granz/info.php`, `/var/www/granz/css/style.css`, dan `/var/www/granz/js/script.js` didapatkan dari `https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing` yang ada pada soal.

**Eisen (Load Balancer) :**
Pertama-tama buat `script.sh` pada root yang berisi ;
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
apt-get install bind9 -y
apt-get install nginx -y
apt-get install apache2-utils -y
apt-get install htop -y

cp weightedrr-lb-granz /etc/nginx/sites-available/lb-granz

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/lb-granz /etc/nginx/sites-enabled/

service nginx restart
```

Kemudian `bash script.sh` tersebut.

Sehingga, 
- Isi dari `/etc/nginx/sites-available/lb-granz`:
```
#menggunakan Weighted Round Robin
upstream backend  {
server 192.184.3.1 weight=4; #IP Lugner
server 192.184.3.2 weight=2; #IP Linie
server 192.184.3.3 weight=1; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```
Keterangan : Load balancer tersebut menggunakan Algoritam Weighted Round Robin.

- Kemudian  `/etc/nginx/sites-available/lb-granz` di-link (dihubungkan) pada `/etc/nginx/sites-enabled/` me-replace `/etc/nginx/sites-enabled/default`

**Revolte :**
```
apt-get update
apt-get install apache2-utils
```
Setelah itu, lakukan testing pada `Client Revolte` .
  
**Screenshot Hasil :**

**Revolte :**

```
ab -n 1000 -c 100 -g out.data http://eisen.granz.channel.b12.com/
```
Berikut merupakan hasil testing dengan 1000 request dan 100 request/second :

<img width="289" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/07c3daf3-b2af-42f7-a7e3-4aba738a76ac">

Berikut merupakan hasil testing request Website Worker PHP menggunakan Load Balncer Algoritma Weighted Round Robin :
```
lynx eisen.granz.channel.b12.com
```

Mengakses Website Luggner : 

<img width="406" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/9e7b226d-f619-440d-8365-18d8991e847e">

Mengakses Website Linie : 

<img width="408" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/f3ccbfa5-874f-4b8f-860e-6b3a482ca27a">

Mengakses Website Lawine : 

<img width="410" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/44393069-1873-432a-b267-f7a6c49e91cf">

Keterangan : Website Lawine akan menerima request terbanyak dari client dibandingkan Website Linie dan Website Lugner, karena Lawine memiliki weight paling besar (weight = 4). Server yang memiliki weight paling besar akan dijadikan prioritas ketika menerima request dari client


## NO. 8
### 3. Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- a. Nama Algoritma Load Balancer
- b. Report hasil testing pada Apache Benchmark
- c. Grafik request per second untuk masing masing algoritma. 
- d. Analisis (8)
  
#### Penjelasan : 
Kemi menggunakan 5 Algoritma Load Balancer yang disimpan pada Eisen (Load Balancer) sebagai berikut :
**1. Round Robin**
```
#Default menggunakan Round Robin
upstream backend  {
server 192.184.3.1; #IP Lugner
server 192.184.3.2; #IP Linie
server 192.184.3.3; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

**2. Weighted Round Robin**
```
#menggunakan Weighted Round Robin
upstream backend  {
server 192.184.3.1 weight=4; #IP Lugner
server 192.184.3.2 weight=2; #IP Linie
server 192.184.3.3 weight=1; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

**3. Least Connection**
```
#menggunakan Least Connection
upstream backend  {
least_conn;
server 192.184.3.1; #IP Lugner
server 192.184.3.2; #IP Linie
server 192.184.3.3; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

**4. IP Hash**
```
#menggunakan IP Hash
upstream backend  {
ip_hash;
server 192.184.3.1; #IP Lugner
server 192.184.3.2; #IP Linie
server 192.184.3.3; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

**5. Generic Hash**
```
#menggunakan Generic Hash
upstream backend  {
hash $request_uri consistent;
server 192.184.3.1; #IP Lugner
server 192.184.3.2; #IP Linie
server 192.184.3.3; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

Berikut Grimoire nya :
### I. Hasil Testing
**a. Algoritma Round Robin (734,77 req/sec)**

<img width="329" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/e76d8794-5f39-431a-ab0f-6802c4feff4a">

<img width="1048" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/82043e8f-09d0-40ac-a409-78968015b4e8">

**b. Algoritma Weighted Round Robin (773,37 req/sec)**

<img width="389" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/9fdf42f8-16a9-460e-8033-b6a02d231e6c">

<img width="853" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/5ac64251-e4fe-43df-9fa1-b5a5d8e012c5">

**c. Algoritma Least Connection (764,74 req/sec)**

<img width="373" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/ff5e1ff7-25ff-41a4-a41f-e6a58d96cf3e">

<img width="814" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/b95b6c1f-ebf1-42db-bf38-1ec09ae6d926">

**d. IP Hash (808,64 req/sec)**

<img width="434" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/670704e8-11ab-4f83-a7fc-761c5f857a4e">

<img width="816" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/407be76c-0898-4cd6-8241-f99c137b6496">

**e. Generic Hash (806,96 req/sec)**

<img width="444" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/4cf3fe8b-efb3-42d2-ae83-80805abeaaec">


<img width="863" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/d00bb649-5a8a-409a-9512-1a55cf9a2a88">

### II. Grafik (Request per second untuk tiap Algo)
- x= nama algo
- y= waktu

<img width="264" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/08d9ab8d-bd90-4c88-93c6-ae780c025eef">

<img width="420" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/494bd481-7a5f-42ee-962d-8ef2623d2eb3">

### Analisis:
a. Algoritma Mana yang Paling Baik?
- Berdasarkan hasil testing, algoritma terbaik adalah IP Hash dengan 808.64 Request/second.

b. Alasan Mengapa IP Hash Paling Baik:
   - Cara Kerja IP Hash:
IP Hash menggunakan alamat IP klien untuk mengarahkan permintaan ke server tertentu. Dengan kata lain, setiap IP klien akan selalu diarahkan ke server yang sama selama IP klien tidak berubah.
   - Keunggulan IP Hash
IP Hash dapat bermanfaat dalam konteks di mana konsistensi sesi antara klien dan server backend diperlukan. Dengan mengaitkan klien ke server tertentu menggunakan alamat IP, keadaan sesi dapat dijaga, menghindari potensi masalah yang terkait dengan perpindahan sesi antar server.
   - Optimasi Load Distribution:
Dalam konteks tes ini, IP Hash dapat memberikan distribusi beban yang cukup merata, yang tercermin dalam jumlah request per second yang tinggi. Algoritma ini bekerja efisien karena mampu mempertahankan koneksi klien pada server yang sama selama alamat IP klien tidak berubah.

c. Cara Kerja IP Hash agar Maksimal:
   - Stabilitas Alamat IP Klien:
Untuk menjaga konsistensi sesi, diperlukan stabilitas alamat IP klien. Jika alamat IP klien sering berubah, keuntungan dari penggunaan IP Hash dapat berkurang.


## NO. 9
### 4. Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)
  
#### Penjelasan : 

**1 Worker (1193,73 req/seq) :**
<img width="432" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/19f395eb-84bb-426a-8a63-9778f8873839">

<img width="343" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/16eac018-514c-4973-9447-76fa1f8f7811">

<img width="816" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/ad860b53-6995-43f7-8be8-a40b5c6ed633">

**2 Worker (1165,99 req/seq): **

<img width="567" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/98fbdf39-b5af-4721-97fb-7f81fc4f0c73">

<img width="425" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/0af41869-f10c-46b8-9ae5-fdb7b134bd8d">

<img width="796" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/8c720a12-2917-44ce-9626-f661604669e6">

**3 Worker (1127,00 req/sec): **

<img width="770" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/8caad4bf-d925-45c1-84b4-2b88643bfb73">

<img width="397" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/2715ff8c-b815-4efe-b180-ff500c8b34fc">

<img width="863" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/150467cb-2f55-43fb-8b44-416591920d61">

**Tabel :** 

<img width="263" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/8d9fdf66-ff25-4b13-992d-01affe59f956">

**Grafik :** 

<img width="419" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/f602a4ea-21fe-4f7b-b4c1-2a45fbb39ec8">

**Analisis :**
Berdasarkan hasil pengujian dengan menggunakan algoritma Round Robin pada jumlah worker yang berbeda, berikut adalah analisisnya:

1 Worker
-	Request/second: 1193.73
- Dengan menggunakan hanya satu worker, aplikasi mencapai kinerja tertinggi. Hal ini menunjukkan bahwai dengan satu worker mampu menangani beban kerja dengan sangat efisien.

2 Worker
   - Request/second: 1165.99
- Meskipun peningkatan jumlah worker menjadi dua, kinerja sedikit menurun menjadi 1165.99 request per detik. Hal ini mungkin disebabkan oleh overhead yang terkait dengan manajemen multiple worker, serta pembagian beban yang kurang optimal dalam kasus ini.

3 Worker
   - Request/second: 1127.00
- Dengan tiga worker, kinerja turun lebih lanjut menjadi 1127.00 request per detik. Peningkatan jumlah worker tidak menghasilkan peningkatan kinerja yang signifikan, bahkan dapat menyebabkan penurunan kinerja, mungkin karena pembagian beban yang tidak optimal.

**Kesimpulan :**

- Pada kasus ini, penggunaan satu worker menunjukkan kinerja paling baik, mencapai tingkat request per detik tertinggi.
- Peningkatan jumlah worker tidak selalu menghasilkan peningkatan kinerja, bahkan dapat menyebabkan penurunan, tergantung pada karakteristik aplikasi dan beban kerja.
- Algoritma Round Robin mungkin mengalami overhead dalam manajemen multiple worker, yang dapat mempengaruhi kinerja keseluruhan.
- Pemilihan jumlah worker perlu disesuaikan dengan karakteristik aplikasi dan sumber daya server yang tersedia.


## NO. 10
### 5. Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ (10)
  
#### Penjelasan : 

**Eisen :**
Pertama-tama, buat file `no10.sh` pada root :
```
cp autentik-lb-granz /etc/nginx/sites-available/lb-granz

mkdir /etc/nginx/rahasisakita/

cp /etc/nginx/.htpasswd /etc/nginx/rahasisakita/

service nginx restart
```

Kemudian, `bash no10.sh`.

Saat diminta, isi `New Password:` dan `Re-type new password:` dengan `ajkb12`

Sehingga,
Isi dari `/etc/nginx/sites-available/lb-granz` :
```
upstream backend  {
server 192.184.3.1; #IP Lugner
server 192.184.3.2; #IP Linie
server 192.184.3.3; #IP Lawine
}

server {
listen 80;
server_name granz.channel.b12.com;

        location / {
                proxy_pass http://backend;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;

		auth_basic "Administrator's Area";
                auth_basic_user_file /etc/nginx/.htpasswd;
        }

        location ~ /\.ht {
            deny all;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

Isi dari `/etc/nginx/rahasisakita/`
```
netics:$apr1$x51ArGS7$Nnw1kePfq6yTmwxYTG4e.1
```

Setelah itu lakukan testing pada `Revolte (Client)`.

**Screenshot hasil :**

**Revolte (Client) :**

```
lynx eisen.granz.channel.b12.com
```

<img width="451" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/2b92a1a5-b7fd-49cb-b976-6aa2dbe47ed5">

Masukkan username `netics` :

<img width="402" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/4f3528c3-b0b7-4c3d-b01e-d85d1cd6c5db">

Masukkan password `ajkb12` :

<img width="415" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/731e80e3-323e-4dcd-805c-adf6f876a933">

Setelah memasukan username dan password yang benar, makan akan dapat mengakses website PHP :

<img width="377" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/103252800/a4303b54-d89c-428f-98f0-5019c0e8dca4">

