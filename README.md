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

### Register domain berupa `riegel.canyon.b12.com` untuk worker Laravel, yang mengarah pada `Fern (worker php)` yang memiliki fixed IP address `192.184.4.1` dan Register domain berupa granz.channel.b12.com untuk worker PHP, yang mengarah pada `Lugner (worker laravel)` yang memiliki fixed IP address `192.184.3.1` :

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
