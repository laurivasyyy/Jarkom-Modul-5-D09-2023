# Jarkom-Modul-5-D09-2023

## Anggota Kelompok
| Nama | NRP |
|---------------------------|------------|
|Dafarel Fatih Wirayudha| 5025211120 |
|Laurivasya Gadhing Syahafidh | 5025211136 | 


## Daftar Isi
- [Pre-Requisites](#daftar-isi)
  - [Topologi GNS3](#topologi-gns3)
  - [Rute](#rute)
  - [Subnetting (VLSM)](#subnetting-vlsm)
  - [IP Tree](#ip-tree)
  - [Network Configuration](#network-configuration)
  - [Routing](#routing)
  - [Configuration](#configuration)
    - [DNS Server](#dns-server)
    - [DHCP Server](#dhcp-server)
    - [DHCP Relay](#dhcp-relay)
    - [Web Server](#web-server)
    - [Client](#client)

- [Pengerjaan](#work)

## Pre-requisites
## Topologi GNS3
![topologi](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/92b3612373a3ab12e474c1467180e77dd0b936f4/assets/Screenshot%202023-12-19%20at%2016.02.32.png)

Keterangan:	
* Richter adalah DNS Server
* Revolte adalah DHCP Server
* Sein dan Stark adalah Web Server
* Jumlah Host pada SchwerMountain adalah 64
* Jumlah Host pada LaubHills adalah 255
* Jumlah Host pada TurkRegion adalah 1022
* Jumlah Host pada GrobeForest adalah 512

## Rute
> Prefix IP kelompok kami adalah 10.26.x.x

![rute](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/92b3612373a3ab12e474c1467180e77dd0b936f4/assets/Screenshot%202023-12-19%20at%2016.03.19.png)

## Subnetting (VLSM)
![subnet](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/92b3612373a3ab12e474c1467180e77dd0b936f4/assets/Screenshot%202023-12-19%20at%2016.02.59.png)

## IP Tree
Untuk menghitung rute-rute yang diperlukan, gunakan perhitungan dengan metode VLSM. Buat juga pohonnya, dan lingkari subnet yang dilewati.

> Untuk mengakses IP Tree secara lebih jelas dapat mengunjungi [link](https://miro.com/app/board/uXjVNI5bWN0=/?share_link_id=350063121070 "tree") berikut.

![tree](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/92b3612373a3ab12e474c1467180e77dd0b936f4/assets/Screenshot%202023-12-19%20at%2016.04.36.png)

## Network Configuration
### 1. Revolte (DHCP Server)
``` 
# Static config for eth0
auto eth0
iface eth0 inet static
    address 10.26.14.130
    netmask 255.255.255.252
    gateway 10.26.14.129
    up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
### 2. Ritcher (DNS Server)
``` 
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.26.14.134
	netmask 255.255.255.252
	gateway 10.26.14.133
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
### 3. Stark (Web Server)
``` 
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.26.14.142
	netmask 255.255.255.252
	gateway 10.26.14.141
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
### 4. Sein (Web Server)
```
auto eth0
iface eth0 inet static
    address 10.26.8.2
    netmask 255.255.252.0
    gateway 10.26.8.1
    up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
### 5. Fern (Router)
```
auto eth0
iface eth0 inet static
    address 10.26.14.2
    netmask 255.255.255.128
    gateway 10.26.14.1

auto eth1
iface eth1 inet static
    address 10.26.14.133
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.26.14.129
    netmask 255.255.255.252

```
### 6. Aura (Router)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 10.26.14.149
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.26.14.145
    netmask 255.255.255.252
```
### 7. Himmel (Router)
```
auto eth0
iface eth0 inet static
address 10.26.14.138
netmask 255.255.255.252
gateway 10.26.14.137
up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
address 10.26.12.1
netmask 255.255.254.0

auto eth2
iface eth2 inet static
address 10.26.14.1
netmask 255.255.255.128
```
### 8. Heiter (Router)
```
auto eth0
iface eth0 inet static
address 10.26.14.150
netmask 255.255.255.252
gateway 10.26.14.149
up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
address 10.26.0.1
netmask 255.255.248.0

auto eth2
iface eth2 inet static
address 10.26.8.1
netmask 255.255.252.0
```
### 9. Client (SchwerMountain, LaubHills, TurkRegion, GrobeForest)
Karena pada permintaan soal IP pada subnet SchwerMountain, LaubHills, TurkRegion, dan GrobeForest menggunakan bantuan DHCP maka IP client tidak perlu dikonfigurasikan.
```
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
## Routing
Tambahkan perintah berikut di dalam network configuration untuk melakukan routing di setiap routernya

### 1. Aura
```
# A9, A10
up route add -net 10.26.0.0 netmask 255.255.248.0 gw 10.26.14.150
up route add -net 10.26.8.0 netmask 255.255.252.0 gw 10.26.14.150

# A1, A2, A3, A4, A5, A6
up route add -net 10.26.14.128 netmask 255.255.255.252 gw 10.26.14.146
up route add -net 10.26.14.132 netmask 255.255.255.252 gw 10.26.14.146
up route add -net 10.26.14.0 netmask 255.255.255.128 gw 10.26.14.146
up route add -net 10.26.12.0 netmask 255.255.254.0 gw 10.26.14.146
up route add -net 10.26.14.136 netmask 255.255.255.252 gw 10.26.14.146
up route add -net 10.26.14.140 netmask 255.255.255.252 gw 10.26.14.146
```

### 2. Frieren
```
# A1, A2, A3, A4
up route add -net 10.26.14.128 netmask 255.255.255.252 gw 10.26.14.138
up route add -net 10.26.14.132 netmask 255.255.255.252 gw 10.26.14.138
up route add -net 10.26.14.0 netmask 255.255.255.128 gw 10.26.14.138
up route add -net 10.26.12.0 netmask 255.255.254.0 gw 10.26.14.138
```

### 3. Himmel
```
# A1, A2
up route add -net 10.26.14.128 netmask 255.255.255.252 gw 10.26.14.2
up route add -net 10.26.14.132 netmask 255.255.255.252 gw 10.26.14.2
```

Setelah malakukan perintah diatas untuk melakukan routing pada setiap 
router, restart node Aura, Frieren, dan Himmel agar routing bisa 
berjalan dengan lancar. Setelah itu, lakukan tes ping antar IP node 
untuk melihat apakah routing sudah berhasil dan pastikan tidak ada error 
agar tidak menghambar perjalanan kita selanjutnya.

## Configuration
Agar kita bisa melakukan config dibawah, kita harus memastikan bahwa setiap node terhubung dengan internet luar dengan cara menambahkan perintah berikut pada network config Aura.
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.26.0.0/16
```
* ### DNS Server
    Lakukan config berikut pada node `Ritcher` yang berperan sebagai DNS Server
```
    # DNS Server
    apt update
    apt install netcat -y
    apt install bind9 -y

    echo '
    options {
        directory "/var/cache/bind";
        
        forwarders {
            192.168.122.1;
        };

        allow-query { any; };
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };' >/etc/bind/named.conf.options

    service bind9 start
```
* ### DHCP Server
    Lakukan config berikut pada node `Revolte` yang berperan sebagai DHCP Server
```
    # DHCP Server

    apt update
    apt install netcat -y
    apt install isc-dhcp-server -y

    echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

    echo '
    # option definitions common to all supported networks...
    option domain-name "example.org";
    option domain-name-servers ns1.example.org, ns2.example.org;

    default-lease-time 600;
    max-lease-time 7200;

    # have support for DDNS.
    ddns-update-style none;

    # A1
    subnet 10.26.14.128 netmask 255.255.255.252{
    }

    # A2
    subnet 10.26.14.132 netmask 255.255.255.252 {
    }

    # A3
    subnet 10.26.14.0 netmask 255.255.255.128 {
    range 10.26.14.2 10.26.14.126;
    option routers 10.26.14.1;
    option broadcast-address 10.26.14.127;
    option domain-name-servers 10.26.14.134;
    default-lease-time 720;
    max-lease-time 7200;
    }

    # A4
    subnet 10.26.12.0 netmask 255.255.254.0 {
    range 10.26.12.2 10.26.13.254;
    option routers 10.26.12.1;
    option broadcast-address 10.26.13.255;
    option domain-name-servers 10.26.14.134;
    default-lease-time 720;
    max-lease-time 7200;
    }

    # A5
    subnet 10.26.14.136 netmask 255.255.255.252 {
    }

    # A6
    subnet 10.26.14.140 netmask 255.255.255.252 {
    }

    # A7
    subnet 10.26.14.144 netmask 255.255.255.252 {
    }

    # A8
    subnet 10.26.14.148 netmask 255.255.255.252 {
    }

    # A9
    subnet 10.26.0.0 netmask 255.255.248.0 {
    range 10.26.0.2 10.26.7.254;
    option routers 10.26.0.1;
    option broadcast-address 10.26.7.255;
    option domain-name-servers 10.26.14.134;
    default-lease-time 720;
    max-lease-time 7200;
    }

    # A10
    subnet 10.26.8.0 netmask 255.255.252.0 {
    range 10.26.8.2 10.26.11.254;
    option routers 10.26.8.1;
    option broadcast-address 10.26.11.255;
    option domain-name-servers 10.26.14.134;
    default-lease-time 720;
    max-lease-time 7200;
    }' > /etc/dhcp/dhcpd.conf

    service isc-dhcp-server restart
```
* ### DHCP Relay
    Lakukan config berikut pada node `Himmel` dan `Heiter` karena node tersebut berdekatan dengan node client yang nanti akan menerima IP forwarder dari DHCP.
```
    #DHCP Relay

    apt update
    apt install netcat -y
    apt install isc-dhcp-relay -y

    echo '
    SERVERS="10.26.14.130"
    INTERFACES="eth0 eth1 eth2"
    OPTIONS=""
    ' > /etc/default/isc-dhcp-relay

    echo '
    net.ipv4.ip_forward=1
    ' > /etc/sysctl.conf

    service isc-dhcp-relay restart
```
    * ### Web Server
    Lakukan config berikut pada node `Sein` dan `Stark` yang berperan sebagai Web Server.
```
    apt update
    apt install netcat -y
    apt install apache2 -y

    echo '# If you just change the port or add more ports here, you will 
    likely also
    # have to change the VirtualHost statement in
    # /etc/apache2/sites-enabled/000-default.conf

    Listen 80
    Listen 443

    <IfModule ssl_module>
            Listen 443
    </IfModule>

    <IfModule mod_gnutls.c>
            Listen 443
    </IfModule>

    # vim: syntax=apache ts=4 sw=4 sts=4 sr noet' > /etc/apache2/ports.
    conf

    echo '# Sein | Stark
    Sein | Stark nih' > /var/www/html/index.html

    service apache2 start
```

* ### Client
    Lakukan config berikut pada semua node client yaitu `TurkRegion`, `SchwerMountain`, `LaubHills`, `GrobeForest`.
```
    apt update
    apt install lynx -y
    apt install netcat -y
```

## Pengerjaan
### Nomor 1
> Soal : Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.

karena pada perintah soal kita tidak ingin menggunakan `MASQUERADE` maka kita perlu menggantinya dengan menghapus `IPTABLES` yang sudah kita konfigurasikan sebelumnya dan memasukkan script berikut ke dalam file.sh
```
    # No 1 (Aura)
    IPETH0="$(ip -br a | grep eth0 | awk '{print $NF}' | cut -d'/' -f1)"
```
    Baris kode pertama digunakan untuk mendapatkan alamat IP dari eth0. Ia enggunakan perintah ip untuk menampilkan daftar semua antarmuka jaringan, lalu menggunakan perintah grep untuk memfilter daftar tersebut berdasarkan antarmuka eth0. Kemudian, perintah awk digunakan untuk mengambil alamat IP dari antarmuka eth0. Terakhir, perintah cut digunakan untuk menghapus informasi subnet dari alamat IP.
```
    iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$IPETH0" -s 10.26.0.0/20
```
* `-t nat` : menentukan tabel NAT yang akan digunakan.
* `-A POSTROUTING` : menambahkan aturan baru ke bagian POSTROUTING dari tabel NAT.
* `-o eth0` : menentukan antarmuka jaringan yang akan digunakan.
* `-j SNAT` : menentukan jenis aturan NAT yang akan digunakan. Dalam hal ini, digunakan SNAT (Source NAT), yang akan mengganti alamat IP sumber dari paket data.
* `--to-source "$IPETH0"` : alamat IP sumber yang akan digunakan. Dalam hal ini, alamat IP sumber yang digunakan adalah alamat IP dari antarmuka eth0.
* `-s 10.26.0.0/20` : alamat IP sumber yang akan diganti. Dalam hal ini, alamat IP sumber yang akan diganti adalah alamat IP dari perangkat yang terhubung ke antarmuka eth0.

### Testing
![1](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/main/assets/1.png)

### Nomor 2
> Soal : Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.

Disini kita akan melakukan filtering pada TCP dan UDP dengan hanya mengizinkan akses dari port 8080 saja pada TCP, disini saya akan melakukannya pada node `Revolte`
```
    iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
    iptables -A INPUT -p tcp -j DROP
    iptables -A INPUT -p udp -j DROP
```
` iptables -A INPUT -p tcp --dport 8080 -j ACCEPT` berfungsi untuk membuka akses protokol TCP melalui port 8080. ` iptables -A INPUT -p tcp|udp -j DROP` mengedrop semua akses komunikasi protokol TCP|UDP yang masuk, dengan ini traffic TCP yang masuk selain lewat port 8080 akan diblokir, sedangkan semua traffic UDP yang masuk akan diblokir semua.

### Testing
* Testing dengan perintah `nc -l -p 8080` :

    ![2a](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/463dea7edcf84786c6404b2bfae00ab4bb3388ae/assets/2a.png)

* Testing dengan perintah `nc -u -l -p 8080` (menggunakan UDP) :

    ![2b](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/463dea7edcf84786c6404b2bfae00ab4bb3388ae/assets/2b.png)

* Testing dengan perintah `nc -l -p 800` :

    ![2c](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/463dea7edcf84786c6404b2bfae00ab4bb3388ae/assets/2c.png)

### Nomor 3
> Soal : Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.

```
iptables -I INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```
* `iptables -I INPUT` : Menambahkan aturan baru ke bagian INPUT dari firewall.
* `-p icmp` : Menargetkan paket protokol ICMP (biasanya digunakan untuk ping).
* `-m connlimit` : Menggunakan modul connlimit untuk membatasi jumlah koneksi.
* `--connlimit-above 3` : Mengedrop paket jika jumlah koneksi dari satu IP melebihi 3.
* `--connlimit-mask 0` : Membatasi per IP, bukan per subnet.
* `-j DROP` : Mengedrop paket yang memenuhi kondisi di atas.
```
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```
* `-m state` : Memfilter berdasarkan status koneksi (state).
* `--state ESTABLISHED,RELATED` : Mengizinkan paket dalam koneksi yang sudah terbentuk (ESTABLISHED) atau terkait dengannya (RELATED).
* `-j ACCEPT`: Mengizinkan paket yang memenuhi kondisi di atas.

### Testing
![3](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/main/assets/3.png)

### Nomor 4
> Soal : Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.

```
iptables -A INPUT -p tcp --dport 22 -s 10.26.8.0/22 -j ACCEPT
```
* `iptables -A INPUT`: Menambahkan aturan baru ke bagian INPUT 
* `-p tcp`: Menargetkan paket protokol TCP.
* `--dport 22`: Menargetkan paket yang ditujukan ke port 22.
* `-s 10.26.8.0/22`: Mengizinkan akses hanya dari alamat IP pada subnet 10.26.8.0/22.
* `-j ACCEPT`: Mengizinkan paket yang memenuhi kondisi di atas.
```
iptables -A INPUT -p tcp --dport 22 -j DROP
```
* `-p tcp` : Menargetkan paket protokol TCP.
* `--dport 22`: Menargetkan paket yang ditujukan ke port 22.
* `-j DROP`: Menngedrop paket yang memenuhi kondisi di atas.

### Testing
![4](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/main/assets/4.png)

### Nomor 5
> Soal : Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.
```
iptables -A INPUT -m time --timestart 08:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```
* `iptables -A INPUT`: Menambahkan aturan baru ke bagian INPUT.
* `-m time` : Memfilter berdasarkan waktu.
* `--timestart 08:00`: Memulai pada pukul 08:00.
* `--timestop 16:00`: Mengakhiri pada pukul 16:00.
* `--weekdays Mon,Tue,Wed,Thu,Fri`: Aturan berlaku hanya pada Senin-Jumat.
* `-j ACCEPT`: Mengizinkan paket yang memenuhi semua kondisi di atas.
* `-j REJECT`: Menolak paket yang tidak memenuhi aturan sebelumnya.

### Testing
Jika saat melakukan testing kita berada di luar aturan parameter waktu yang diberikan, maka kita perlu set manual tanggal dan waktu terlebih dahulu sebelum melakukan testing dengan `date --set="2023-12-19 09:00:00"` menjalankan command tersebut.

* Sesuai dengan parameter waktu yang diberikan :

    ![5a](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/main/assets/5a.png)

* Tidak sesuai dengan parameter waktu yang diberikan :

    ![5b](https://github.com/laurivasyyy/Jarkom-Modul-5-D09-2023/blob/main/assets/5b.png)
