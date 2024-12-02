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
