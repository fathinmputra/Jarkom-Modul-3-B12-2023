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
