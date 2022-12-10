# Jarkom-Modul-5-E08-2022
Laporan Resmi Praktikum V Jaringan Komputer oleh Kelompok E08

### Kelompok E08, PREFIX IP: 192.196

| **No** | **Nama** | **NRP** |
| - | - | - |
| 1. | Halyusa Ard Wahyudi | 5025201088 |
| 2. | Muhammad Ismail | 5025201223 |
| 3. | Immanuel Maruli Tua Pardede | 5025201166 |

## Soal
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

<ol>
<li>Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE.</li>
<li>Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan.</li>
<li>Loid meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 2 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.</li>
<li>Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00.</li>
<li>Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan.</li>
<li>Karena Loid ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.</li>
</ol>

Loid berterima kasih pada kalian karena telah membantunya. Loid juga mengingatkan agar semua aturan iptables harus disimpan pada sistem atau paling tidak kalian menyediakan script sebagai backup.

## Jawaban
Berikut merupakan topologi pada GNS3 dan pembagian subnetnya.

<img src="https://github.com/immanuelmtpardede/Jarkom-Modul-5-E08-2022/blob/main/img/A.png" width="50%">

Beikut merupakan perhitungan konfigurasinya.

<img src="https://github.com/immanuelmtpardede/Jarkom-Modul-5-E08-2022/blob/main/img/B.png" width="50%">

## Konfigurasi Network Setiap Node
Ostania
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

Westalis
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

Strix
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

Blackbell
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

Briar
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

Desmond
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

Forger
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

Garden
```
auto eth0
iface eth0 inet static
      address 192.196.7.143
      netmask 255.255.255.248
      gateway 192.196.7.141
```

Eden
```
auto eth0
iface eth0 inet static
      address 192.196.7.131
      netmask 255.255.255.248
      gateway 192.196.7.129
```

SSS
```
auto eth0
iface eth0 inet static
      address 192.196.7.142
      netmask 255.255.255.248
      gateway 192.196.7.141
```

WISE
```
auto eth0
iface eth0 inet static
      address 192.196.7.130
      netmask 255.255.255.248
      gateway 192.196.7.129
```
