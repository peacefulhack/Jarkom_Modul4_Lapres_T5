<!--
*** Thanks for checking out this README Template. If you have a suggestion that would
*** make this better, please fork the repo and create a pull request or simply open
*** an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
-->





<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
[![Contributors][contributors-shield]][contributors-url]
[![Stargazers][stars-shield]][stars-url]
[![Kelompok][kelompok-shield]][kelompok-url]


<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5">
    <img src="images/logo.gif" alt="Logo" width="200" height="200">
  </a>

  <h3 align="center">Nama dan NRP kelompok</h3>

  <p align="center">
    M. Mikail Dwi K.    (05311840000028)
    <br />
    Dimas P. H.         (05311840000037)
    <br />
    <a href="https://github.com/peacefulhack/Jarkom_Modul2_Lapres_T05#daftar-isi"><strong>Lihat Daftar Isi »</strong></a>
    <br />
    <br />
    <a href="https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/tree/main/images">Gambar</a>
    ·
    <a href="https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/pulse">Grafik</a>
    ·
    <a href="https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/issues">Lihat issue</a>
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Daftar Isi

* [Soal](#Soal)
* [Persiapan](#Persiapan)
  * [Setting Topologi](#Setting-Topologi)
  * [Setting DNS](#Setting-DNS)
* [Jawaban](#Jawaban)



<!-- ABOUT THE PROJECT -->
## Soal
<br />

![screenshot][screenshot1]

<br />
Catatan

1. Deadline hari Rabu, 9 Desember 2020 pukul 22.00
2. Soal shift dikerjakan pada Cisco Packet Tracer dan UML menggunakan metode perhitungan CLASSLESS yang berbeda. Keterangan: Bila di CPT menggunakan VLSM, maka di UML menggunakan CIDR atau Sebaliknya
3. Jika tidak ada pemberitahuan revisi soal dari asisten, berarti semua soal BERSIFAT BENAR dan DAPAT DIKERJAKAN.
4. CLOUD diberikan IP TUNTAP.
5. Server diberikan IP DMZ.
6. Berikan memori sebesar 64MB pada setiap UML.
7. Pembagian IP dan routing harus SE-EFISIEN MUNGKIN.
8. Pastikan semua UML dapat melakukan ping ke its.ac.id

## Persiapan
### Setting Topologi
Sebelum mejawab pertanyaan no 1, kita harus membuat topologi yang benar, buka xming dan putty, lalu buka openvpn connect, pertama konekkan vpn menggunakan .ovpn yang diberi asisten, lalu gunakan putty logi menggunakan IP pada modul sesuai kelompok, lalu buat topologi.sh menggunakan 
```bash
nano topologi.sh
```
lalu tuliskan
```sh
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,'ip_tuntap_tiap_kelompok' eth1=daemon,,,switch2 eth2=daemon,,,switch1 mem=96M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=126M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=126M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch2 mem=126M &

# Klien
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=96M &
```
lalu seting interfaces sesuai modul, sedangkan probolinggo mirip dengan malang namun menggunakan ip probolinggo. lalu melakukan ``iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16`` pada router suarabaya lalu melakukan export.
### Setting DNS
lakukan ``apt-get update`` pada UML malang, lalu install bind9 menggunakan command ``apt-get install bind9 -y``, pada uml yang sama, gunakan konfigurasi pada ``/etc/bind/named.conf.local`` untuk menambahkan
```
zone "semerut05.pw" {
	type master;
	file "/etc/bind/jarkom/semerut05.pw";
};
```
Buat folder jarkom di dalam /etc/bind

```
mkdir /etc/bind/jarkom
```

Copykan file db.local pada path /etc/bind ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi semerut05.pw

```
cp /etc/bind/db.local /etc/bind/jarkom/semeruyyy.pw
```

Kemudian buka file semeruyyy.pw dan edit seperti gambar berikut dengan IP MALANG masing-masing kelompok:
```
nano /etc/bind/jarkom/semeruyyy.pw
```
<br />

![screenshot][screenshot2]

<br />

Restart bind9 dengan perintah

```
service bind9 restart

ATAU

named -g //Bisa digunakan untuk restart sekaligus debugging
```

Lalu membuat DNS Reverse(sesuai modul)

Lalu agar kita dapat mengakses website menggunakan semeruyyy.pw dan tidak perlu mengetikkan IP maka kita menggunakan cname
tambahkan, seperti gambar
<br />

![screenshot][screenshot2]

<br />
Kemudian restart bind9 dengan perintah

service bind9 restart
Lalu cek dengan melakukan ``host -t CNAME www.semeruyyy.pw`` atau ``ping www.semeruyyy.pw``. Hasilnya harus mengarah ke host dengan IP MALANG.

untuk membuat dns slave, kita gunakan server malang ``nano /etc/bind/named.conf.local`` lalu tambahkan
```
zone "semeruyyy.pw" {
    type master;
    notify yes;
    also-notify { "IP MOJOKERTO"; }; // Masukan IP MOJOKERTO tanpa tanda petik
    allow-transfer { "IP MOJOKERTO"; }; // Masukan IP MOJOKERTO tanpa tanda petik
    file "/etc/bind/jarkom/semeruyyy.pw";
};
```
Lakukan restart bind9

``service bind9 restart``

lalu pada mojokerto, update package lists dengan menjalankan command:

``apt-get update`` lalu ``apt-get install bind9 -y`` kemudian ``nano /etc/bind/named.conf.local`` tambahkan
```
zone "semeruyyy.pw" {
    type slave;
    masters { "IP MALANG"; }; // Masukan IP MALANG tanpa tanda petik
    file "/var/lib/bind/semeruyyy.pw";
};
```
<br />

![screenshot][screenshot3]

<br />

Lakukan restart bind9

``service bind9 restart``

membuat sub domain ``nano /etc/bind/jarkom/semerut05.pw`` tambahkan seperti gambar
<br />

![screenshot][screenshot4]

<br />

lalu ``service bind9 restart``
lalu coba ping apakah berhasil

<br />

![screenshot][screenshot5]

<br />

setelah itu membuat delegasi sub-domain ``nano /etc/bind/jarkom/semerut05.pw`` pada malang

<br />

![screenshot][screenshot2]

<br />

lalu ``service bind9 restart``

## Setting Web Service


## Jawaban
<!-- GETTING STARTED -->

 
<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/peacefulhack/Jarkom_Modul4_Lapres_T5?style=flat-square
[contributors-url]: https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/graphs/contributors
[stars-shield]: https://img.shields.io/github/stars/peacefulhack/Jarkom_Modul4_Lapres_T5?style=flat-square
[stars-url]: https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/stargazers
[kelompok-shield]: https://img.shields.io/badge/Kelompok-T05-blue
[kelompok-url]: https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/
[screenshot1]: images/screenshot1.png
[screenshot2]: images/ss2.png
[screenshot3]: images/ss3.png
[screenshot4]: images/ss4.png
[screenshot5]: images/ss5.png
[screenshot6]: images/ss6.png
[screenshot7]: images/ss7.png
[screenshot8]: images/ss8.png
[screenshot9]: images/ss9.png
[screenshot10]: images/ss10.png
[screenshot11]: images/ss11.png
[screenshot12]: images/ss12.png
[screenshot13]: images/ss13.png
[screenshot14]: images/ss14.png
[screenshot15]: images/ss15.png
[screenshot16]: images/ss16.png
