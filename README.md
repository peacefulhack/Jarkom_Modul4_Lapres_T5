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

## Jawaban
### Cisco packet tracer (VLSM)

1. Install [cisco packet tracer](https://www.netacad.com/courses/packet-tracer)
2. Setup topologi dengan menambahkan Cloud PT, Switch, Router dan PC
3. Sambungkan setiap komponen menggunakan Connection -> Auto Choose conn Type
4. beberapa router tidak dapat disambungkan lebih dari 2 cabang maka gunakan klik pada router, lalu pada tab physical pilih:
    - Surabaya      = NM-4E (Karena membutuhkan 5 koneksi: 2 default, 4 tambahan)
    ![screenshot][screenshot2]
    - Pasuruan      = NM-1FE-TX (karena membutuhkan 3 koneksi: 2 default, 1 tambahan)
    ![screenshot][screenshot3]
    - Probolinggo   = NM-1FE-TX (karena membutuhkan 3 koneksi: 2 default, 1 tambahan)
    ![screenshot][screenshot3]
    - Batu          = NM-2FE2W (karena membutuhkan 4 koneksi: 2 default, 2 tambahan)
    ![screenshot][screenshot4]
    - Kediri        = NM-1FE-TX (karena membutuhkan 3 koneksi: 2 default, 1 tambahan)
    ![screenshot][screenshot3]

## Pembagian IP & Netmask
kita bagi dulu koneksi yang membutuhkan network, terciptalah A1-A13 seperti pada gambar
![screenshot][screenshot5]

lalu kita hitung kebutuhan host pada setiap jaringan A1-13 lalu didapatkan seperti gambar berikut
![screenshot][screenshot6]

lalu setelah mendapatkan submasknya, kita dapat membagi IP berdasarkan IP terbesar 192.168.0.0/19 karena submask total adalah /19, lalu didapatkan ip sebagai berikut:
![screenshot][screenshot7]

## Implementasi pada Cisco
lalu setelah kita bagi, kita masukkan kedalam cisco sesuai pada pembagian ip dan subnet pada sebelumnya, saya ambil contoh pada A1, maka IP Networknya 192.168.0.16/28 maka usable IP pertamanya adalah 192.168.0.17 dimasukkan pada router pada Fa 0/1 karena Fa0/1 adalah koneksi menuju bojonegoro dimana adalah A1 lalu pada bojonegoro, kita set 192.168.0.18 lalu gatewaynya adalah 192.168.0.17.
![screenshot][screenshot8]
![screenshot][screenshot9]
dan begitu terus untuk semua dari A1-A13

## Routing
untuk routing kita harus memasukkan route yang tidak dikenali oleh router tersebut, karena tidak bertetangga-an:
- Surabaya = A1,A2,A3,A4,A5,A6,A9,A10,A11,A12,Malang = 11 + 1 default 0.0.0.0/0 = 12
- Pasuruan = A12,A11 = 2 + 1 default 0.0.0.0/0 = 3
- Probolinggo = 1 default 0.0.0.0/0
- Batu = A1,A6,A5,Malang = 4 + 1 default 0.0.0.0/0 = 5
- Madiun = 1 default 0.0.0.0/0
- Kediri = A6 + 1 default 0.0.0.0/0 = 2
- Blitar = 1 default 0.0.0.0/0

seperti gambar berikut saya beri contoh pada surabaya
![screenshot][screenshot10]

## Hasil
![screenshot][screenshot11]


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/peacefulhack/Jarkom_Modul4_Lapres_T5?style=flat-square
[contributors-url]: https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/graphs/contributors
[stars-shield]: https://img.shields.io/github/stars/peacefulhack/Jarkom_Modul4_Lapres_T5?style=flat-square
[stars-url]: https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/stargazers
[kelompok-shield]: https://img.shields.io/badge/Kelompok-T05-blue
[kelompok-url]: https://github.com/peacefulhack/Jarkom_Modul4_Lapres_T5/
[screenshot1]: images/ss1.png
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
