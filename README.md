# Jarkom-Modul-5-E08-2022
Laporan Resmi Praktikum V Jaringan Komputer oleh Kelompok E08

### Kelompok E08, PREFIX IP: 192.196

| **No** | **Nama** | **NRP** |
| - | - | - |
| 1. | Halyusa Ard Wahyudi | 5025201088 |
| 2. | Muhammad Ismail | 5025201223 |
| 3. | Immanuel Maruli Tua Pardede | 5025201166 |

## Soal Pengantar
Setelah kalian mempelajari semua modul yang telah diberikan, Loid ingin meminta bantuan untuk terakhir kalinya kepada kalian. Dan kalian dengan senang hati mau membantu Loid.

(A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Loid dibawah ini:

<img src="https://github.com/immanuelmtpardede/Jarkom-Modul-5-E08-2022/blob/main/img/0.png" width="50%">

Keterangan:
<ul>
<li>Eden adalah DNS Server</li>
<li>WISE adalah DHCP Server</li>
<li>Garden dan SSS adalah Web Server</li>
<li>Jumlah Host pada Forger adalah 62 host</li>
<li>Jumlah Host pada Desmond adalah 700 host</li>
<li>Jumlah Host pada Blackbell adalah 255 host</li>
<li>Jumlah Host pada Briar adalah 200 host</li>
</ul>

(B) Untuk menjaga perdamaian dunia, Loid ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM setelah melakukan subnetting.

(C) Anya, putri pertama Loid, juga berpesan kepada anda agar melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

(D) Tugas berikutnya adalah memberikan ip pada subnet Forger, Desmond, Blackbell, dan Briar secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

## Jawaban Pengantar
### Topologi dan Perhitungan
Berikut merupakan topologi pada GNS3 dan pembagian subnetnya.

<img src="https://github.com/immanuelmtpardede/Jarkom-Modul-5-E08-2022/blob/main/img/A.png" width="50%">

Beikut merupakan perhitungan konfigurasinya.

<img src="https://github.com/immanuelmtpardede/Jarkom-Modul-5-E08-2022/blob/main/img/B.png" width="50%">

### Konfigurasi Network Setiap Node
1. Strix
```
auto lo
iface lo inet loopback

auto eth1
iface eth1 inet dhcp

auto eth0
iface eth0 inet static
address 192.196.7.145
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.196.7.149
netmask 255.255.255.252
```

2. Ostania
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
      address 192.196.7.150
      netmask 255.255.255.252
      gateway 192.196.7.149

auto eth1
iface eth1 inet static
      address 192.196.4.1
      netmask 255.255.254.0

auto eth2
iface eth2 inet static
      address 192.196.6.1
      netmask 255.255.255.0

auto eth3
iface eth3 inet static
      address 192.196.7.141
      netmask 255.255.255.248
```

3. Westalis
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
      address 192.196.7.129
      netmask 255.255.255.248

auto eth1
iface eth1 inet static
      address 192.196.7.1
      netmask 255.255.255.128

auto eth2
iface eth2 inet static
      address 192.196.0.1
      netmask 255.255.252.0

auto eth3
iface eth3 inet static
      address 192.196.7.146
      netmask 255.255.255.252
      gateway 192.196.7.145
```

4. Blackbell
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

5. Briar
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

6. Desmond
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

7. Forger
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

8. Garden
```
auto eth0
iface eth0 inet static
      address 192.196.7.143
      netmask 255.255.255.248
      gateway 192.196.7.141
```

9. Eden
```
auto eth0
iface eth0 inet static
      address 192.196.7.131
      netmask 255.255.255.248
      gateway 192.196.7.129
```

10. SSS
```
auto eth0
iface eth0 inet static
      address 192.196.7.142
      netmask 255.255.255.248
      gateway 192.196.7.141
```

11. WISE
```
auto eth0
iface eth0 inet static
      address 192.196.7.130
      netmask 255.255.255.248
      gateway 192.196.7.129
```

### Routing
1. Strix
```
route add -net  192.196.7.128 netmask 255.255.255.248 gw 192.196.7.146
route add -net  192.196.7.0 netmask 255.255.255.128 gw 192.196.7.146
route add -net  192.196.0.0 netmask 255.255.252.0 gw 192.196.7.146
route add -net  192.196.7.144 netmask 255.255.255.252 gw 192.196.7.146

route add -net  192.196.7.148 netmask 255.255.255.252 gw 192.196.7.150
route add -net  192.196.6.0 netmask 255.255.255.0 gw 192.196.7.150
route add -net  192.196.4.0 netmask 255.255.254.0 gw 192.196.7.150
route add -net  192.196.7.136 netmask 255.255.255.248 gw 192.196.7.150
```

2. Ostania
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.196.7.149
```

3. Westalis
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.196.7.145
```

### Konfigurasi DNS Server, Web server, DHCP Server, dan DHCP relay
1. Eden sebagai DNS Server

Buat file bernama named.conf.options dan isi kode berikut.
```
options {
        directory "/var/cache/bind";
        forwarders {
                192.168.122.1;
        };
        allow-query { any; };
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
```

Kemudian, jalankan perintah berikut.
```
apt update
apt install bind9 -y
cp named.conf.options /etc/bind/named.conf.options
service bind9 restart
```

2. WISE sebagai DHCP Server

Buat file bernama dhcpd.conf dan isi kode berikut.
```
ddns-update-style none;
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
default-lease-time 600;
max-lease-time 7200;
log-facility local7;
subnet 192.196.0.0 netmask 255.255.252.0 {
range 192.196.0.2 192.196.3.254;
option routers 192.196.0.1;
option broadcast-address 192.196.3.255;
option domain-name-servers 192.196.7.131;
default-lease-time 360;
max-lease-time 7200;
}
subnet 192.196.7.0 netmask 255.255.255.128 {
range 192.196.7.2 192.196.7.126;
option routers 192.196.7.1;
option broadcast-address 192.196.7.127;
option domain-name-servers 192.196.7.131;
default-lease-time 720;
max-lease-time 7200;
}
subnet 192.196.4.0 netmask 255.255.254.0 {
range 192.196.4.2 192.196.5.254;
option routers 192.196.4.1;
option broadcast-address 192.196.5.255;
option domain-name-servers 192.196.7.131;
default-lease-time 720;
max-lease-time 7200;
}
subnet 192.196.6.0 netmask 255.255.255.0 {
range 192.196.6.2 192.196.6.254;
option routers 192.196.6.1;
option broadcast-address 192.196.6.255;
option domain-name-servers 192.196.7.131;
default-lease-time 720;
max-lease-time 7200;
}
subnet 192.196.7.128 netmask 255.255.255.248 {}
subnet 192.196.7.144 netmask 255.255.255.252 {}
subnet 192.196.7.148 netmask 255.255.255.252 {}
subnet 192.196.7.136 netmask 255.255.255.248 {}
```

Selanjutnya, buat juga file bernama isc-dhcp-server dan isi kode berikut.
```
INTERFACES="eth0"
```

Kemudian, jalankan perintah berikut.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt update
apt install isc-dhcp-server -y
cp isc-dhcp-server /etc/default/isc-dhcp-server
cp dhcpd.conf /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
```

3. Ostania sebagai DHCP Relay

Jalankan perintah berikut.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt update
apt install isc-dhcp-relay -y
echo '
SERVERS="192.196.7.130"
INTERFACES="eth2 eth3 eth0 eth1"
OPTIONS=""
' > /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
```

4. Westalis sebagai DHCP Relay

Jalankan perintah berikut.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt update
apt install isc-dhcp-relay -y
echo '
SERVERS="192.196.7.130"
INTERFACES="eth2 eth3 eth1 eth0"
OPTIONS=""
' > /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
```

5. Garden sebagai Web Server

Jalankan perintah berikut.
```
apt update
apt install apache2 -y
service apache2 start
echo "$HOSTNAME" > /var/www/html/index.html
```

6. SSS sebagai Web Server

Jalankan perintah berikut.
```
apt update
apt install apache2 -y
service apache2 start
echo "$HOSTNAME" > /var/www/html/index.html
```

## Soal dan Jawaban Modul
1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE.

Strix
```
iptables -t nat -A POSTROUTING -s 192.196.0.0/21 -o eth1 -j SNAT --to-source 192.168.122.241
cat /etc/resolv.conf
ping google.com
```

2. Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan.

Strix
```
iptables -A FORWARD -d 192.196.7.130 -i eth0 -p tcp -j DROP
iptables -A FORWARD -d 192.196.7.130 -i eth0 -p udp -j DROP
```

3. Loid meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 2 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

WISE
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
```

Eden
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
```

4. Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00.

Garden
```
iptables -A INPUT -s 192.196.7.136/29 -m time --weekdays Sat,Sun -j REJECT
iptables -A INPUT -s 192.196.7.136/29 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri  -j REJECT
iptables -A INPUT -s 192.196.7.136/29 -m time --timestart 16:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
```

SSS
```
iptables -A INPUT -s 192.196.7.136/29 -m time --weekdays Sat,Sun -j REJECT
iptables -A INPUT -s 192.196.7.136/29 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri  -j REJECT
iptables -A INPUT -s 192.196.7.136/29 -m time --timestart 16:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
```

5. Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan.

```

```

6. Karena Loid ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.

```

```

Loid berterima kasih pada kalian karena telah membantunya. Loid juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.
