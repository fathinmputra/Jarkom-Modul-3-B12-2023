# Praktikum Jaringan Komputer
# Modul 3
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

## NO. 11
### Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass).

### Penjelasan :

Melakukan bash `no11.sh` pada load balancer `eisen`. Fungsi bash adalah menambahkan location baru untuk its ke lb-granz :
```
        location ~ /its {
                proxy_pass  https://www.its.ac.id;
                proxy_set_header Host www.its.ac.id;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }
```

Sehingga selengkapnya seperti berikut :
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

        location ~ /its {
                proxy_pass  https://www.its.ac.id;
                proxy_set_header Host www.its.ac.id;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
}
```

Kemudian dilakukan lynx dengan menambahkan `/its` untuk pengecekan :
> lynx eisen.granz.channel.b12.com/its

Berikut hasilnya :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/6a61738e-b08c-46fa-a5db-31469d742c01)

## No.12
### 7.	Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.7, [Prefix IP].4.167, dan [Prefix IP].4.168. (12)

### Penjelasan :

### Eisen

Dilakukan bash `no12.sh` pada Eisen. Isi dari no12.sh :
```
cp lb-granz-no12 /etc/nginx/sites-enabled/lb-granz
service nginx restart
```

Dimana isi dari lb-granz-no12 adalah untuk menambahkan akses kepada beberapa ip pada location. 
```
location / {
                allow 192.184.3.69;
                allow 192.184.3.70;
                allow 192.184.4.167;
                allow 192.184.4.168;
                deny all;
                proxy_pass http://backend;
        }
```

### Richter

Pada client Richter, dilakukan beberapa instalasi dan perubahan konfigurasi interfacenya. Dapat dilakukan dengan `bash no12.sh` :
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install apache2-utils
apt install less -y
echo 'nameserver 192.184.1.3' > /etc/resolv.conf

cp interfaces12 /etc/network/interfaces
```

Isi dari konfigurasi interface yang diganti adalah :
```
#auto eth0
#iface eth0 inet static
#       address 192.184.3.4
#       netmask 255.255.255.0
#       gateway 192.184.3.55

auto eth0
iface eth0 inet dhcp
hwaddress ether 62:57:bb:6d:48:c8
```

### Himmel

Memberi fixed address kepada client Richter. Dapat dilakukan dengan `bash no12.sh`. Isi dari bash no12.sh adalah menambahkan fixed adrress ke Richter pada `dhcpd.conf` :
```
host Richter {
    hardware ethernet 62:57:bb:6d:48:c8;
    fixed-address 192.184.3.70;
}
```

Kemudian Restart node `Client Richter`.

### Revolta

Lakukan lynx pada client revolta
>lynx eisen.granz.channel.b12.com

Hasilnya :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/510f0e79-a494-4755-92d2-c00e445e6f13)

- Forbidden, karena IP nya sudah di luar rentang yang ditentukan.

## No. 13
### 1.	Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.

### Penjelasan :

### Denken

Bash `script.sh`. Isi dari script.sh adalah installing mariadb-server dan kemudian membuat user, password, database, dan privilage pada databsenya. Isi dari `script.sh` :

```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y
service mysql start
echo 'nameserver 192.184.1.3' > /etc/resolv.conf

mysql -e "CREATE USER 'kelompokb12'@'%' IDENTIFIED BY 'passwordb12';"
mysql -e "CREATE USER 'kelompokb12'@'localhost' IDENTIFIED BY 'passwordb12';"
mysql -e "CREATE DATABASE dbkelompokb12;"
mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'kelompokb12'@'%';"
mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'kelompokb12'@'localhost';"
mysql -e "FLUSH PRIVILEGES;"

cp my.cnf /etc/mysql/my.cnf
service mysql restart
```

Selanjutnya pada bagian terakhir script.sh, ada my.cnf, dimana akan ditambahkan sedikit konfigurasi. Isi tambahannya :
> my.cnf
```
[mysqld]
skip-networking=0
skip-bind-address
```

### Fern, Flamme, Frieren

Bash script.sh yang isinya installing mariadb client & show database Derken. Isi dari `script.sh` :
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install mariadb-client -y
echo 'nameserver 192.184.1.3' > /etc/resolv.conf
mariadb --host=192.184.2.2 --port=3306 --user=kelompokb12 --password=passwordb12 -e "SHOW DATABASES;"
```

Setelah selesai mejalankan, jika berhasil maka akan ditampilkan database dari `node Denken` :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/02c0bfdf-23ae-4063-a0d9-11c63dea2665)

## No. 14
### 2.	Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)

### Penjelasan :

### Frieren, Flamme, dan Fern 

Melakukan `bash no14.sh` pada semua worker laravel. Isi dari no14.sh :
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

apt-get install git -y

cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git
cd /var/www/laravel-praktikum-jarkom && composer update

echo 'nameserver 192.184.1.3' > /etc/resolv.conf

cd /var/www/laravel-praktikum-jarkom && cp .env.example .env

cp env1 /var/www/laravel-praktikum-jarkom/.env

cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear

chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage

cp default /etc/nginx/sites-available/default

service nginx start
service php8.0-fpm start

service nginx restart
service php8.0-fpm restart
```
- Pertama, akan dilakukan instalasi composer dan git.
- Kemudian, akan diclone repository laravel-praktikum-jarkom ke path /var/www
- Copy .env.example dan paste penjadi .env, setelahnya akan dipindahkan beberapa konfigurasi dari env1 ke .env . Berikut perubahannya :
> env1
```
DB_CONNECTION=mysql
DB_HOST=192.184.2.2
DB_PORT=3306
DB_DATABASE=dbkelompokb12
DB_USERNAME=kelompokb12
DB_PASSWORD=passwordb12
```
- Selanjutnya, ada beberapa perintah yang dijalankan pada repo yang telah diclone tadi. Beberapa contohnya seperti `php artisan migrate` untuk migrasi tabel dari Laravel ke Database dan `php artisan db:seed` melakukan seed data ke database.
- Selanjutnya adalah melakukan perubahan pada `/etc/nginx/sites-available/default`. Berikut adalah isi dari default yang telah dirubah :
> default
```
server {

    listen 8003;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/default_error.log;
    access_log /var/log/nginx/default_access.log;
}
```
> Setiap worker diberi port yang berbeda. Fern: 8001, Flamme: 8002, Frieren: 8003. Jadi, potongan di atas untuk Frieren karena listen 8003.
- Terakhir restart nginx dan php.

Setelah selesai, untuk testing dapat dilakukan lynx pada worker :
> lynx localhost:(Port worker)

Contoh testing pada `worker Frieren` :
> lynx localhost:8003

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/e6887390-2fae-4c97-b53a-b64be1c7e855)

## No. 15
### 3.	Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. a.	POST /auth/register (15)

### Penjelasan :

### Sein

Pada `Client Sein`. Dibuat `register.json` untuk nantinya dipakai untuk melakukan POST /auth/register. Isi dari register.json :
```
{
    "username": "kelompokb12",
    "password": "passwordb12"
}
```

Kemudian, karena harus ditesting sebanyak 100 requests dengan 10 request/second, maka akan dilakukan pengetesan dengan menggunakan apache benchmark. Perintahnya :
> ab -n 100 -c 10 -p register.json -T application/json http://192.184.4.1:8001/api/auth/register

Hasil Testing :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/e1bb25c5-fcde-4d0e-91be-006a3fa88cfb)


## No. 16
### b.	POST /auth/login (16)

### Penjelasan

### Sein

Sama seperti register tadi, dibuat file json untuk nantinya dipakai untuk melakukan POST /auth/login. Kami diberi nama `login.json` yang isinya :
```
{
    "username": "kelompokb12",
    "password": "passwordb12"
}
```

Kemudian, karena harus ditesting sebanyak 100 requests dengan 10 request/second, maka akan dilakukan pengetesan dengan menggunakan apache benchmark. Perintahnya :
> ab -n 100 -c 10 -p login.json -T application/json http://192.184.4.1:8001/api/auth/login

- Testing dilakukan dengan IP mengarah ke worker `Fern` yaitu `192.184.4.1` dan dengan port `8001`.

Hasil Testing :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/5ae7cfd5-e321-438b-b0a3-9bb0baced8b9)
![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/dfd059f2-650c-4609-b8e5-87d7afa11e79)

## No. 17
### c.	GET /me (17)

### Penjelasan :

### Sein

Pertama, untuk melakukan proses otentikasi atau login ke suatu sistem dengan menggunakan data yang ada dalam file JSON, perlu mengambil token loginnya terlebih dahulu. Hasil tokennya akan ditulis ke `login_output.txt`. Perintahnya :
> curl -X POST -H "Content-Type: application/json" -d @login.json http://192.184.4.1:8001/api/auth/login > login_output.txt

Setelah itu, gunakan perintah untuk membaca file login_output.txt, mengekstrak nilai dari kunci "token" dalam respons JSON menggunakan jq, dan menyimpan nilai tersebut dalam variabel shell token. Perintahnya :
> token=$(cat login_output.txt | jq -r '.token')

Dengan memasukkan token otentikasi dalam header "Authorization". Dapat dilakukan akses /me. Karena ingin dilakukan testing sebanyak 100 requests dengan 10 request/second, maka akan dilakukan pengetesan dengan menggunakan apache benchmark. Perintahnya :
> ab -n 100 -c 10 -H "Authorization: Bearer $token" http://192.184.4.1:8001/api/me

Hasil testing :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/c6a0c53b-98ce-4635-a948-fe0cef5b61b8)
![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/8c882cab-e8b2-40d5-809d-df116dd103ff)

## No. 18
### Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. (18)

### Penjelasan :

### Eisen

Agar ketiganya bekerja secara adil, disini akan diimplementasikan load balancing pada load balancer `Eisen`. 
Untuk menjalankannya, cukup melakukan `bash no18.sh`. Isinya :
```
cp lb-riegel /etc/nginx/sites-available/lb-riegel

ln -s /etc/nginx/sites-available/lb-riegel /etc/nginx/sites-enabled/lb-riegel

unlink /etc/nginx/sites-enabled/lb-granz

service nginx restart
```
- Copy isi dari lb-riegel ke path /etc/nginx/sites-available/. isi dari lb-riegel sendiri adalah implementasi load balancing ketiga worker laravel dan diarahkan ke website `riegel.canyon.b12.com`. Isinya :
```
upstream laravel {
        server 192.184.4.1:8001;
        server 192.184.4.2:8002;
        server 192.184.4.3:8003;
}

server {
        listen 80;
        server_name riegel.canyon.b12.com;

        location / {
                proxy_pass http://laravel;
        }
}
```
- Setelah itu, akan dilakukan link /etc/nginx/sites-available/lb-riegel ke /etc/nginx/sites-enabled/lb-riegel.
- Terakhir, unlink lb-granz karena ditakutkan bertabrakan. Kemudian restart nginx.

Setelah konfigurasi, saatnya melakukan testing ke domain eisen.riegel.canyon.b12.com untuk cek apakah load balancer ke worker laravel sudah berhasil atau belum. `Note: Mengapa domainnya eisen.riegel.canyon.b12.com? karena pada DNS Master, IP utama riegel.canyon.b12.com menuju ke IP worker Fern. Jadi, jika ingin aksesnya mengarah ke IP eisen, antara mengubah IP utama di DNS Master menjadi menuju ke IP Eisen atau menggunakan domain eisen.riegel.canyon.b12.com`. Perintah testing login :
> ab -n 100 -c 10 -p login.json -T application/json http://eisen.riegel.canyon.b12.com/api/auth/login

Hasil Testing :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/8ebaf462-279e-4562-a637-268bdc0b2735)

Hasil cek log masing-masing worker untuk memastikan ketiganya bekerja secara adil :

Log Fern :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/5b7c71c8-f258-498d-a2a2-7935d6efdb2b)

Log Flamme :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/efd4b311-3c75-47f6-acb7-9f420fd6ec77)

Log Frieren :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/f2ad7f2f-b988-43af-8160-15ac61f9d22f)

## No. 19
### 5. Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan - pm.max_children - pm.start_servers - pm.min_spare_servers - pm.max_spare_servers sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.(19)

### Penjelasan :

### Fern, Flamme, Frieren
Pada masing masing worker, telah disediakan script `bash percobaan-0.sh` `bash percobaan-1.sh` `bash percobaan-2.sh` `bash percobaan-3.sh`. Dimana `percobaan-0` merupakan nilai default dari pm.max_children - pm.start_servers - pm.min_spare_servers - pm.max_spare_servers, `percobaan-1` adalah nilai pm.max_children - pm.start_servers - pm.min_spare_servers - pm.max_spare_servers yang sudah dinaikkan, dst. Jika ingin melakukan testing, maka perintah bash harus dijalankan di semua worker. Contoh ingin melakukan percobaan-2, maka pada semua worker perlu `bash percobaan-2.sh`.

Isi dari masing masing percobaan :
> percobaan-0 (Default)
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
```

> percobaan-1
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 10
pm.start_servers = 4
pm.min_spare_servers = 2
pm.max_spare_servers = 6
```

> percobaan-2
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 15
pm.start_servers = 6
pm.min_spare_servers = 3
pm.max_spare_servers = 9
```

> percobaan-3
```
[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 20
pm.start_servers = 8
pm.min_spare_servers = 4
pm.max_spare_servers = 12
```

Setelah itu, masing-masing percobaan dilakukan test sebanyak 100 request dengan 10 request/second. Disini akan ditest dengan perintah login. Perintahnya :
> ab -n 100 -c 10 -p login.json -T application/json http://eisen.riegel.canyon.b12.com/api/auth/login

Hasil Testing :

> percobaan-0

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/ed410d33-aebf-436a-aec3-bd786dc9c12e)

> percobaan 1

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/abf0a780-bfd3-4c91-9c1e-f107665d1f32)


> percobaan 2

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/8b707f83-a614-4c9f-8cfc-8beaf8e785b2)


> percobaan 3

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/7d29fcb9-88c2-4655-8280-ef667b4b5a2f)

## No. 20
### 6. Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)

### Penjelasan :

### Eisen
Untuk mengimplementasikan algoritma Least-Conn pada load balancer. Maka pada file lb-riegel akan ditambahkan :
> least_conn;

Implementasi lengkapnya dapat dilakukan `bash no20.sh` yang isinya :
```
cp leastcon-lb-riegel /etc/nginx/sites-available/lb-riegel

service nginx restart
```
- Isi leastcon-lb-riegel :
```
upstream laravel {
        least_conn;
        server 192.184.4.1:8001;
        server 192.184.4.2:8002;
        server 192.184.4.3:8003;
}

server {
        listen 80;
        server_name riegel.canyon.b12.com;

        location / {
                proxy_pass http://laravel;
        }
}
```
- leastcon-lb-riegel merupakan implementasi algoritma Least-Conn. leastcon-lb-riegel akan dicopy untuk menggantikan algoritma sebelumnya ke /etc/nginx/sites-available/lb-riegel.
- Setelah algoritma diganti, restart nginx.

Hasil testing login dengan implementasi Least-Conn :

![image](https://github.com/fathinmputra/Jarkom-Modul-3-B12-2023/assets/133391111/542a2818-4bb6-4097-9d1b-a15db381ccc4)
