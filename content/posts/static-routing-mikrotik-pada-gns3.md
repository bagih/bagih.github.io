---
title: "Static Routing Mikrotik Pada Gns3"
date: 2023-10-05T23:29:27+08:00
draft: false
tags: ['gns3', 'jaringan', 'routing', 'mikrotik']
cover: "/img/srmpg/cover.png"
coverCaption: "dokumentasi pribadi"
---

## Pendahuluan

Assalamualaikum, hai selamat datang kembali, kali ini kita akan membahas tentang konfigurasi static routing pada mikrotik dan simulasinya pada gns3. Tanpa berlama lama lagi kita lanjutkan saja ke pembahasannya.

## Apa itu routing

Routing adalah sebuah metode untuk merutekan paket dari sebuah jaringan ke jaringan lainnya / jaringan yang berbeda. Tujuan dari routing ini adalah agar beberapa jaringan dapat saling berkomunikasi. Untuk dapat menggunakan metode routing maka kita harus mengaplikasikannya pada perangkat Layer 3. Perangkat yang digunakan untuk melakukan routing disebut sebagai **Router** .

Ada banyak tipe dan merek router yang beredar di pasaran, beberapa yang populer diantaranya antara lain dari: Cisco, Mikrotik dan Juniper. Pemilihan routernya pun tergantung dari kebutuhan dan budget yang tersedia.

## Static Routing vs Dynamic Routing

Berdasarkan metode routing yang dipakai pada tabel routing, metodenya dibagi menjadi 2 jenis yaitu Static routing dan Dynamic routing. Static routing adalah metode routing yang peruteannya dilakukan secara manual oleh pengelola jaringan dengan melakukan input secara langsung pada tabel routing. Sedangkan dynamic routing adalah metode routing yang penginputan rute ke tabel routing dilakukan secara otomatis oleh protokol dynamic routing yang tersedia menggunakan perhitungan algoritma tertentu untuk mendapatkan hasil yang terbaik. Contoh protokol dynamic routing antara lain seperti: RIP, OSPF, BGP, EIGRP dll.

## Perancangan Simulasi

Simulasi menggunakan 3 buah VPC dan 3 buah router mikrotik yang dirangkai pada software gns3. Setiap router memiliki 1 buah VPC yang membentuk 1 buah jaringan. Antar router dihubungkan dengan menggunakan kabel yang terhubung ke tiap interface pada masing masing router. untuk pengalamatan yang digunakan adalah sebagai berikut

```bash
PC1 = 192.168.1.1/24 gw: 192.168.1.254
PC2 = 192.168.2.1/24 gw 192.168.2.254
PC3 = 192.168.3.1/24 gw 192.168.3.254

R1 -> PC1 = 192.168.1.254/24 ether3
R1 -> R2 = 10.10.10.1/29 ether1
R1 -> R3 = 10.10.12.2/29 ether2

R2 -> PC2 = 192.168.2.254/24 ether3
R2 -> R1 = 10.10.10.2/29 ether1
R2 -> R3 = 10.10.11.1/29 ether2

R3 -> PC3 = 192.168.3.254/24 ether3
R3 -> R1 = 10.10.12.1/29 ether2
R3 -> R2 = 10.10.11.2/29 ether1
```

Silahkan rangkai setiap perangkat yang dibutuhkan pada gns3 seperti pada gambar dibawah ini.

{{< figure src="/img/srmpg/cover.png" alt="topologi" position="center" style="border-radius: 8px;" caption="topologi" >}}

## Konfigurasi Mikrotik

Pertama tama lakukan konfigurasi pada mikrotik dengan memberikan pengalamatan sesuai dengan tabel ip diatas. Konfigurasi dapat dilakukan dengan menggunakan perintah `ip address add address={alamat ip} interface={interface yang terhubung}` pada masing masing router sesuai dengan tabel ip sebelumnya.

Jika sudah, selanjutnya lakukan konfigurasi routing pada mikrotik dengan menggunakan perintah `ip route add dst-address={ip network tujuan / mask} gateway={ip / interface gateway}`, dst-address disini diisi dengan ip network tujuan yang ingin dirutekan dan gateway adalah ip / interface yang digunakan untuk melewatkan paket tersebut dalam hal ini adalah ip dari R2 yang terhubung langsung ke R1. Contoh jika ingin merutekan paket ke PC2 dari jaringan pada R1 menggunakan perintah `ip route add dst-address=192.168.2.0/24 gateway=10.10.10.2`. Untuk perintah konfigurasi routing lengkap pada setiap router dapat dilihat pada tabel kode dibawah
```bash
R1
ip route add dst-address=192.168.2.0/24 gateway=10.10.10.2
ip route add dst-address=192.168.3.0/24 gateway=10.10.12.1

R2
ip route add dst-address=192.168.1.0/24 gateway=10.10.10.1
ip route add dst-address=192.168.3.0/24 gateway=10.10.11.2

R3
ip route add dst-address=192.168.1.0/24 gateway=10.10.12.1
ip route add dst-address=192.168.2.0/24 gateway=10.10.11.1
```

jika konfigurasi benar maka setiap router akan memiliki konfigurasi sebagai berikut:

R1

{{< figure src="/img/srmpg/R1.png" alt="R1" position="center" style="border-radius: 8px;" caption="R1" >}}

R2

{{< figure src="/img/srmpg/R2.png" alt="R2" position="center" style="border-radius: 8px;" caption="R2" >}}

R3

{{< figure src="/img/srmpg/R3.png" alt="R3" position="center" style="border-radius: 8px;" caption="R3" >}}

## Konfigurasi Pada VPC

Selanjutnya lakukan konfigurasi ip address pada masing masing vpc dengan menggunakan tabel ip diatas menggunakan perintah `ip {ip address/mask} {ip gateway}`, contoh jika pada PC1 maka perintahnya `ip 192.168.1.1/24 192.168.1.254`. Jika konfigurasinya benar maka akan sesuai seperti gambar dibawah.

PC1

{{< figure src="/img/srmpg/PC1_konf.png" alt="PC1" position="center" style="border-radius: 8px;" caption="PC1" >}}

PC2

{{< figure src="/img/srmpg/PC2_konf.png" alt="PC2" position="center" style="border-radius: 8px;" caption="PC2" >}}

PC3

{{< figure src="/img/srmpg/PC3_konf.png" alt="PC3" position="center" style="border-radius: 8px;" caption="PC3" >}}

## Testing

Langkah terakhir adalah lakukan testing untuk mendapatkan hasil konfigurasi dengan melakukan ping dari PC1 ke PC2 dan PC3. Jika konfigurasinya benar maka akan tampil seperti gambar dibawah

{{< figure src="/img/srmpg/PC1.png" alt="PC1 test" position="center" style="border-radius: 8px;" caption="PC1 test" >}}

## Penutup

Sekian tutorial yang bisa saya sampaikan kali ini, semoga bisa bermanfaat. Kurang lebihnya mohon maaf, kita ketemu lagi di tutorial tutorial berikutnya insyaallah. akhir kata saya ucapkan wassalamualaikum.