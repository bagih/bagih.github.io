---
title: "Simulasi Konfigurasi Vlan Dasar Di Gns3"
date: 2023-09-16T18:04:06+08:00
draft: false
tags: ['gns3', 'jaringan']
cover: "/img/skvddg/cover.png"
coverCaption: "dokumentasi pribadi"
---

## Pengenalan

Hai, selamat datang kembali. Kali ini saya akan membagikan tutorial tentang konfigurasi dasar vlan pada gns3 menggunakan switch layer3 dari cisco dengan seri c3725. Perangkat perangkat yang digunakan pada simulasi ini adalah 4 buah VPC dan 1 L3 switch.

VLAN (Virtual LAN) adalah salah satu metode pembagian / pengelompokan jaringan menggunakan perangkat layer 2 (managed switch) yang digunakan untuk membuat jaringan virtual di dalam sebuah jaringan LAN. Secara default perangkat perangkat yang terhubung pada jaringan VLAN hanya bisa berkomunikasi dengan perangkat pada VLAN yang sama, namun kita bisa membuat setiap VLAN dapat melakukan komunikasi dengan sebuah metode yang bernama InterVLAN Routing yang akan kita bahas di tutorial-tutorial selanjutnya. Baiklah kita mulai saja pembahasannya

## Persiapan

Pada simulasi kali ini kita akan membuat 2 buah vlan yaitu VLAN dengan id 10 yang bernama "finance" dan VLAN dengan id 20 yang bernama "IT" . pengalamatan yang digunakan yaitu pada VLAN 10 menggunakan IP 192.168.10.0/24 dan pada VLAN 20 menggunakan IP 192.168.20.0/24 . PC yang terhubung pada VLAN 10 adalah PC1 dan PC3 sedangkan pada VLAN 20 PC yang terhubung adalah PC2 dan PC3.

Tambahkan perangkat-perangkat yang dibutuhkan kedalam workspace yaitu 1 buah managed switch dan 4 buah VPC kemudian atur pada area workspace agar terlihat rapi. Selanjutnya tambahkan kabel antara switch dan masing-masing PC dengan menggunakan add a link pada bagian kiri gns3. Untuk memudahkan anda mengetahui port mana saja yang terhubung pada perangkat jaringan, anda bisa mengaktifkan fitur **Show / Hide Interface Labels** yang terdapat pada toolbar bagian atas pada gns3. Selanjutnya klik icon play pada toolbar bagian atas untuk mengaktifkan semua perangkat jaringan pada area workspace.

{{< figure src="/img/skvddg/cover.png" alt="topologi" position="center" style="border-radius: 8px;" caption="topologi" >}}

## Konfigurasi Switch

Pertama-tama silahkan lakukan console ke switch. kemudian ketikkan perintah perintah seperti dibawah ini:

{{< code language="bash" title="Konfigurasi vlan pada switch" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
(config)# configure terminal
(config)# hostname SW1
(config)# no ip domain lookup
(config)# VLAN 10
(config-vlan)# name finance
(config-vlan)# exit
(config)# VLAN 20
(config-vlan)# name IT
(config-vlan)# exit
{{< /code >}}

perintah hostname digunakan untuk mengganti nama hostname dari switch ke SW1, no ip domain lookup digunakan agar switch tidak mentranslasikan perintah yang salah / tidak ditemukan ke dalam IP. kemudian kita membuat 2 buah VLAN dengan perintah selanjutnya.

Selanjutnya lakukan konfigurasi masing-masing interface yang terhubung ke switch dengan menggunakan perintah dibawah ini:

{{< code language="bash" title="Konfigurasi lengkap interface vlan pada fa1/0" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
(config)# interface fa1/0
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
(config-if)# exit
{{< /code >}}

perintah diatas digunakan untuk mengalokasikan port fa1/0 yang terhubung ke PC1 sebagai port akses untuk VLAN 10. konfigurasi lengkap untuk setiap pc adalah sebagai berikut:

{{< code language="bash" title="Konfigurasi lengkap vlan" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
(config)# interface fa1/0
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
(config-if)# exit

(config)# interface fa1/1
(config-if)# switchport mode access
(config-if)# switchport access vlan 20
(config-if)# exit

(config)# interface fa1/2
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
(config-if)# exit

(config)# interface fa1/3
(config-if)# switchport mode access
(config-if)# switchport access vlan 20
(config-if)# exit
{{< /code >}}

jika sudah melakukan konfigurasi, ketikkan perintah `show vlan-switch` untuk melihat konfigurasi vlan pada switch. Jika konfigurasi sudah tepat, ketikkan perintah `end` dan `wr` untuk menyimpan konfigurasi.

{{< figure src="/img/skvddg/sw.png" alt="sw" position="center" style="border-radius: 8px;" caption="detail konfigurasi vlan tersimpan" >}}

## Konfigurasi IP pada masing-masing VPC

Selanjutnya lakukan konfigurasi ip pada masing masing vpc.

ketikkan perintah `ip 192.168.10.2/24` untuk memberikan ip pada VPC dan `show ip` untuk melihat konfigurasi ip yang tersimpan pada PC1.

{{< figure src="/img/skvddg/pc1.png" alt="pc1" position="center" style="border-radius: 8px;" caption="pc1" >}}

Lanjutkan pemberian IP pada VPC yang lain denga alamat sebagai berikut:

```bash
PC2 192.168.20.1/24
PC3 192.168.10.2/24
PC4 192.168.20.2/24
```

PC2

{{< figure src="/img/skvddg/pc2.png" alt="pc2" position="center" style="border-radius: 8px;" caption="pc2" >}}

PC3

{{< figure src="/img/skvddg/pc3.png" alt="pc3" position="center" style="border-radius: 8px;" caption="pc3" >}}

PC4

{{< figure src="/img/skvddg/pc4.png" alt="pc4" position="center" style="border-radius: 8px;" caption="pc4" >}}

## Testing Hasil Konfigurasi

Lakukan testing pada masing-masing VPC dengan melakukan ping ke setiap VPC. lakukan console ke PC1 kemudian ping ke setiap VPC. Jika konfigurasinya tepat maka akan tampil seperti gambar dibawah.

{{< figure src="/img/skvddg/test.png" alt="testing" position="center" style="border-radius: 8px;" caption="testing" >}}

dari gambar dapat kita lihat bahwa PC1 dapat berkomunikasi dengan PC3 karena berada pada VLAN yang sama sedangkan PC1 tidak dapat berkomunikasi dengan PC 2 dan PC4 karena tidak berada pada VLAN yang sama
