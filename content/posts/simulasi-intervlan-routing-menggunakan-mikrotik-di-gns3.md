---
title: "Simulasi Intervlan Routing Menggunakan Mikrotik Di Gns3"
date: 2023-09-20T19:21:00+08:00
draft: false
tags: ['gns3', 'jaringan', 'vlan', 'mikrotik']
cover: "/img/sirmmdg/cover.png"
coverCaption: "dokumentasi pribadi"
---

## Pendahuluan

Assalamualaikum. Hai, selamat datang kembali, kali ini kita akan melanjutkan postingan sebelumnya tentang VLAN. Jika pada postingan sebelumnya kita VLAN dengan 2 buah switch dan mendefinisikan mode-mode yang ada pada switchportnya, pada tutorial kali ini kita akan melakukan metode intervlan routing pada vlan menggunakan mikrotik pada gns3, mari kita lanjutkan.

## InterVLAN Routing

Sekilas tentang interVLAN Routing, ini adalah sebuah metode routing yang digunakan agar antar VLAN dapat berkomunikasi dengan memanfaatkan perangkat Layer 3, pada kasus kali ini kita akan memanfaatkan router Mikrotik sebagai perangkat untuk melakukan interVLAN Routing. Dengan melakukan interVLAN routing, perangkat-perangkat yang berbeda VLAN ID dapat melakukan komunikasi, untuk lebih jelasnya mengenai pembahasan interVLAN routing anda bisa membacanya pada [artikel berikut](https://www.section.io/engineering-education/inter-vlan-routing/)

## Persiapan

Disini kita akan melanjutkan project pada artikel sebelumnya, jika anda belum mengikuti artikelnya ada bisa membacanya di [sini](/posts/simulasi-perbedaan-vlan-mode-akses-dan-trunk-di-gns3/) . Pada tutorial kita akan menambahkan 1 buah switch yang terhubung ke SW1, SW2, dan Mikrotik. Mikrotik yang akan kita gunakan adalah mikrotik dengan versi RouterOS 7.11.2 CHR.

Konfigurasi yang akan kita lakukan nantinya adalah melakukan konfigurasi trunk antar switch dan mikrotik, konfigurasi ip pada masing-masing VPC dengan gateway berupa IP Sub interface mikrotik, dan konfigurasi pada router mikrotik seperti pembuatan interface VLAN dan pemberian ip pada interface tersebut. Namun sebelum itu tambahkan perangkat-perangkat yang dibutuhkan seperti pada gambar dibawah ini:

{{< figure src="/img/sirmmdg/cover.png" alt="topologi" position="center" style="border-radius: 8px;" caption="topologi" >}}

## Konfigurasi masing-masing switch

Pertama tama lakukan konfigurasi vlan pada SW3 seperti pada switch-switch lainnya, hal ini karena masing-masing switch harus memiliki informasi mengenai vlan yang sama untuk dapat meneruskan paket dengan vlan id tertentu. Tambahkan juga mode trunk ke port interface yang terhubung ke mikrotik dari SW3. Jika sudah, lakukan konfigurasi masing-masing switch untuk menambahkan informasi trunk pada interface yang terhubung ke SW3 dan sebaliknya. Selanjutnya cek konfigurasi trunk pada masing masing switch dengan perintah `# show interface trunk` , jika konfigurasi benar maka akan tampil seperti gambar di bawah ini:

SW1

{{< figure src="/img/sirmmdg/sw1.png" alt="SW1" position="center" style="border-radius: 8px;" caption="SW1" >}}

SW2

{{< figure src="/img/sirmmdg/sw2.png" alt="SW2" position="center" style="border-radius: 8px;" caption="SW2" >}}

SW3

{{< figure src="/img/sirmmdg/sw3.png" alt="SW3" position="center" style="border-radius: 8px;" caption="SW3" >}}

## Konfigurasi Mikrotik

Selanjutnya lakukan konfigurasi pada mikrotik dengan melakukan console pada mikrotik. Konfigurasi yang akan disetting meliputi penambahan interface vlan dan pemberian ip pada interface vlan tersebut. Untuk melakukan penambahan vlan ketikkan perintah berikut pada console mikrotik:

{{< code language="bash" title="Penambahan vlan pada mikrotik" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
[admin@mikrotik] /interface vlan add vlan-id=10 name=finance interface=ether1
[admin@mikrotik] /interface vlan add vlan-id=20 name=IT interface=ether1
{{< /code >}}

Kemudian tambahkan ip pada masing masing interface vlan dengan menggunakan perintah berikut:

{{< code language="bash" title="konfigurasi ip vlan pada mikrotik" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
[admin@mikrotik] /ip address add address=192.168.10.254 interface=finance
[admin@mikrotik] /ip address add address=192.168.20.254 interface=IT
{{< /code >}}

lakukan print untuk melihat hasil konfigurasi yang dibuat dengan menggunakan perintah `/interface vlan print` dan `/ip address print` . Jika konfigurasi benar maka akan sesuai seperti pada gambar dibawah:

{{< figure src="/img/sirmmdg/mikrotik.png" alt="Mikrotik" position="center" style="border-radius: 8px;" caption="mikrotik" >}}

## Konfigurasi masing masing VPC

Langkah selanjutnya adalah melakukan konfigurasi pada masing masing vpc dengan menambahkan ip gateway. Pada vlan 10 ip gateway yang digunakan adalah 192.168.10.254, ini adalah ip interface vlan 10 pada mikrotik sedangkan pada vlan 20 ip gateway yang digunakan adalah 192.168.20.254 .

Untuk melakukan konfigurasi ip dengan gateway pada masing masing vpc gunakan format perintah `ip [ip dengan notation] [ip gateway]` . contoh pada PC1 kita bisa mengetikkan perintah `ip 192.168.10.1/24 192.168.10.254` . Lakukan konfigurasi untuk setiap VPC yang terhubung ke jaringan dengan ketentuan bahwa PC yang terhubung ke VLAN 10 memiliki gateway 192.168.10.254 sedangkan pada VLAN 20 memiliki gateway 192.168.20.254 . berikut adalah hasil konfigurasi pada PC1 dan PC2

PC1

{{< figure src="/img/sirmmdg/pc1-gw.png" alt="PC1" position="center" style="border-radius: 8px;" caption="PC1" >}}

PC2

{{< figure src="/img/sirmmdg/pc2-gw.png" alt="PC2" position="center" style="border-radius: 8px;" caption="PC2" >}}

## Testing

Lakukan testing dengan melakukan console ke salah satu VPC kemudian ping ke setiap VPC pada jaringan. Contohnya adalah sebagai berikut:

{{< figure src="/img/sirmmdg/testing.png" alt="testing" position="center" style="border-radius: 8px;" caption="testing" >}}

Jika berhasil maka setiap VPC yang terdapat pada jaringan akan dapat melakukan komunikasi walaupun berbeda VLAN ID, hal ini karena kita sudah melakukan konfigurasi interVLAN routing dengan memanfaatkan router Mikrotik.

## Penutup

Sekian tutorial yang bisa saya sampaikan kali ini, kurang lebihnya mohon maaf, kita bertemu lagi pada tutorial-tutorial berikutnya, Insyaallah. Wassalamualaikum.