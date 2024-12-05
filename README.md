# Jarkom-Modul-5-IT30-2024

| Nama | NRP |
|---------------------------|------------|
|Riskiyatul Nur Oktarani | 5027231013 |
|Malvin Putra Rismahardian | 5027231048 |

<hr>

### Prefix IP
Disini Prefix IP yang kami gunakan adalah 192.248

### Topologi IT30
####  Sebuah topologi sederhana menggambarkan jaringan New Eridu:
![image](https://github.com/user-attachments/assets/50c95e0f-3050-48cd-b2d4-a5344823d303)
#### Keterangan:
- HDD: Berfungsi sebagai DNS Server.
- Fairy: Berfungsi sebagai DHCP Server.
- Web Servers: HIA, HollowZero.
#### Client:
- Burnice: Memiliki 5 host.
- Lycaon: Memiliki 20 host.
- Policeboo: Memiliki 30 host.
- Caesar: Memiliki 50 host
- Ellen: Memiliki 100 host.
- Jane: Memiliki 200 host.

###  Setelah membagi alamat IP menggunakan VLSM, gambarkan pohon subnet yang menunjukkan hierarki pembagian IP di jaringan New Eridu. Lingkari subnet-subnet yang akan dilewati dalam jaringan.
#### Tabel Routing : 
![image](https://github.com/user-attachments/assets/a76505de-a658-4690-b65b-abf6949ad417)
#### Tabel Pembagian IP VLSM :
![image](https://github.com/user-attachments/assets/1682cf75-6c13-41d1-b816-b4861a8ee2bb)
#### Tree VLSM:
![topologi drawio](https://github.com/user-attachments/assets/34a4ab3c-6113-4e04-8086-a8eb891ee8ad)
#### topologi jaringan pada GNS3 dengan lingkaran pembagian IP VLSM : 
![image](https://github.com/user-attachments/assets/2160066a-441c-4fe4-bb2b-887d3ca004bb)

###  Setelah pembagian IP selesai, buatlah konfigurasi rute untuk menghubungkan semua subnet dengan benar di jaringan New Eridu. Pastikan perangkat dapat saling terhubung.

### - NewEridu
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
post-up route add -net 192.248.1.16 netmask 255.255.255.252 gw 192.248.1.21
post-up route add -net 192.248.1.64 netmask 255.255.255.192 gw 192.248.1.21
post-up route add -net 192.248.1.0 netmask 255.255.255.248 gw 192.248.1.21
post-up route add -net 192.248.1.8 netmask 255.255.255.248 gw 192.248.1.21
post-up route add -net 192.248.1.128 netmask 255.255.255.128 gw 192.248.1.33
post-up route add -net 192.248.1.24 netmask 255.255.255.248 gw 192.248.1.33
post-up route add -net 192.248.0.0 netmask 255.255.255.0 gw 192.248.1.33
```

</details>

### - SixStreet
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>
  
```
post-up route add -net 192.248.1.16 netmask 255.255.255.252 gw 192.248.1.1
post-up route add -net 192.248.1.64 netmask 255.255.255.192 gw 192.248.1.2
```

</details>

### - LuminaSquare
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
post-up route add -net 192.248.1.128 netmask 255.255.255.128 gw 192.248.1.25
```

</details>

### - ScoutOutpost
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.248.1.3
```

</details>

### - OuterRing
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.248.1.3
```

</details>

### - BalletTwins
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.248.1.27
```

</details>

### Konfigurasi → dikerjakan setelah misi 2 nomor 1
### Fairy sebagai DHCP Server agar perangkat yang berada dalam Burnice, Caesar, Ellen, Jane, Lycaon, dan Policeboo dapat menerima alamat IP secara otomatis. OuterRing, BalletTwins, Sixstreet dan LuminaSquare Sebagai DHCP Relay HDD sebagai DNS server HIA dan HollowZero Sebagai Web server (gunakan apache) 

#### index.html
```
Welcome to {hostname}
```

### Konfigurasi
* DHCP Relay (OuterRing, SixStreet, LuminaSquare, BalletTwins)
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
apt-get update
apt-get install isc-dhcp-relay netcat -y
service isc-dhcp-relay start

echo 'SERVERS="192.248.1.10"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay restart
```
</details>

* DHCP Server (Fairy)
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
apt-get update
apt-get install isc-dhcp-server netcat -y

echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo 'subnet 192.248.1.64 netmask 255.255.255.192 {
    range 192.248.1.65 192.248.1.125;
    option routers 192.248.1.126;
    option broadcast-address 192.248.1.63;
    option domain-name-servers 192.248.1.9;
    default-lease-time 600;
    max-lease-time 7200;
}

subnet 192.248.0.0 netmask 255.255.255.0 {
    range 192.248.0.1 192.248.0.253;
    option routers 192.248.0.254;
    option broadcast-address 192.248.0.255;
    option domain-name-servers 192.248.1.9;
    default-lease-time 600;
    max-lease-time 7200;
}

subnet 192.248.1.128 netmask 255.255.255.128 {
    range 192.248.1.129 192.248.1.253;
    option routers 192.248.1.254;
    option broadcast-address 192.248.1.255;
    option domain-name-servers 192.248.1.9;
    default-lease-time 600;
    max-lease-time 7200;
}

subnet 192.248.1.8 netmask 255.255.255.248 {
}'> /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
</details>

* DNS Server (HDD)
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
apt-get update
apt-get install bind9 netcat -y

echo 'options {
        directory "/var/cache/bind";

        forwarders {
            192.168.122.1;
        };

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
</details>

* Web Server (HIA, HollowZero)
<details>
<summary><h5>Klik untuk melihat Konfigurasi</h5></summary>

```
apt-get update
apt-get install apache2 netcat -y

HOST=$(hostname)
echo "Welcome to $HOST" > /var/www/html/index.html

service apache2 restart
```
</details>

# MISSION 2
## Menemukan Jejak Sang Peretas
### Agar jaringan di New Eridu bisa terhubung ke luar (internet), kalian perlu mengkonfigurasi routing menggunakan iptables. Namun, kalian tidak diperbolehkan menggunakan MASQUERADE

* Script ini dijalankan sebelum misi 1 no 4 di router NewEridu agar dapat connect ke internet

```
ETH0_IP=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $ETH0_IP
```
### Karena Fairy adalah AI yang sangat berharga, kalian perlu memastikan bahwa tidak ada perangkat lain yang bisa melakukan ping ke Fairy. Tapi Fairy tetap dapat mengakses seluruh perangkat.
* script untuk dns server Fairy

```
iptables -A OUTPUT -j ACCEPT
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -j DROP
```
### Dokumentasi Ping Testing

### Selain itu, agar kejadian sebelumnya tidak terulang, hanya Fairy yang dapat mengakses HDD. Gunakan nc (netcat) untuk memastikan akses ini. [hapus aturan iptables setelah pengujian selesai agar internet tetap dapat diakses.]
* Konfigurasikan iptables untuk memblokir permintaan ping dari seluruh jaringan, namun tambahkan pengecualian agar IP Fairy tetap dapat melakukan ping
```
iptables -A INPUT -s 192.248.1.10 -j ACCEPT
iptables -A INPUT -j DROP
```

### Dokumentasi Ping Testing

### Fairy mendeteksi aktivitas mencurigakan di server Hollow. Namun, berdasarkan peraturan polisi New Eridu, Hollow hanya boleh diakses pada hari Senin hingga Jumat dan hanya oleh faksi SoC (Burnice & Caesar) dan PubSec (Jane & Policeboo). Karena hari ini hari Sabtu, mereka harus menunggu hingga hari Senin. Gunakan curl untuk memastikan akses ini.
* Jalankan script berikut pada web server HollowZero
```
iptables -A INPUT -s 192.248.1.64/26 -m time --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 192.248.0.0/24 -m time --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```

### Dokumentasi Ping Testing

### Sembari menunggu, Fairy menyarankan Phaethon untuk berlatih di server HIA dan meminta bantuan dari faksi Victoria (Ellen & Lycaon) dan PubSec. Akses HIA hanya diperbolehkan untuk a. Ellen dan Lycaon pada jam 08.00-21.00. b. Jane dan Policeboo pada jam 03.00-23.00. (hak kepolisian) Gunakan Curl untuk memastikan akses ini.
* Ini script iptables di web server HIA
```
iptables -A INPUT -s 192.248.0.0/24 -m time --timestart 03:00 --timestop 23:00 -j ACCEPT
iptables -A INPUT -s 192.248.1.128/25 -m time --timestart 08:00 --timestop 21:00 -j ACCEPT
iptables -A INPUT -j REJECT
```

### Dokumentasi Testing

### Sebagai bagian dari pelatihan, PubSec diminta memperketat keamanan jaringan di server HIA. Jane dan Policeboo melakukan simulasi port scan menggunakan nmap pada rentang port 1-100. a. Web server harus memblokir aktivitas scan port yang melebihi 25 port secara otomatis dalam rentang waktu 10 detik. b. Penyerang yang terblokir tidak dapat melakukan ping, nc, atau curl ke HIA. c. Catat log dari iptables untuk keperluan analisis dan dokumentasikan dalam format PDF.
* Buat konfigurasi untuk memblokir aktivitas port scanning yang melebihi 25 port dalam rentang 10 detik, penyerang yang diblokir tidak bisa ping, nc, atau curl ke HIA, log dari iptables akan tercatat untuk analisis.

```
iptables -N PORTSCAN

# Mendeteksi aktivitas baru dan menandai IP
iptables -A INPUT -p tcp -m state --state NEW -m recent --set --name portscan

# Blokir IP yang melakukan lebih dari 25 koneksi dalam 10 detik
iptables -A INPUT -p tcp -m state --state NEW -m recent --update --seconds 10 --hitcount 25 --name portscan -j DROP

# untuk melakukan logging
iptables -A INPUT -p tcp -m state --state NEW -m recent --update --seconds 10 --hitcount 25 --name portscan -j LOG --log-prefix "Port Scan Detected: " --log-level 7

#  Blokir ICMP (ping)
iptables -A INPUT -p icmp -m recent --name portscan --rcheck -j DROP

# Blokir TCP dan UDP
iptables -A INPUT -p tcp -m recent --name portscan --rcheck -j DROP
iptables -A INPUT -p udp -m recent --name portscan --rcheck -j DROP

#  Konfigurasi Forward Chain
iptables -A FORWARD -m recent --name portscan --rcheck -j DROP
```

### Dokumentasi Testing

### Hari Senin tiba, dan Fairy menyarankan membatasi akses ke server Hollow. Akses ke Hollow hanya boleh berasal dari 2 koneksi aktif dari 2 IP yang berbeda dalam waktu bersamaan. Burnice, Caesar, Jane, dan Policeboo diminta melakukan uji coba menggunakan curl.
* Script untuk membuat akses hanya boleh berasal dari 2 koneksi aktif dari 2 ip yang berbeda dalam waktu yang bersamaan pada HollowZero
```
iptables -I INPUT -p tcp --dport 80 -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
iptables -I INPUT -p tcp --dport 443 -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP

iptables -I INPUT -p tcp --dport 80 -m hashlimit --hashlimit-name ip_limit --hashlimit-above 2/sec --hashlimit-mode srcip --hashlimit-srcmask 32 -j DROP
iptables -I INPUT -p tcp --dport 443 -m hashlimit --hashlimit-name ip_limit --hashlimit-above 2/sec --hashlimit-mode srcip --hashlimit-srcmask 32 -j DROP

iptables -I INPUT -p tcp --dport 80 -m connlimit --connlimit-above 2 --connlimit-mask 32 -j DROP
iptables -I INPUT -p tcp --dport 443 -m connlimit --connlimit-above 2 --connlimit-mask 32 -j DROP

iptables -I INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

### Dokumentasi Testing






























