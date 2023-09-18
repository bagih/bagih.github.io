---
title: "Simulasi Perbedaan Vlan Mode Akses Dan Trunk Di Gns3"
date: 2023-09-18T19:15:49+08:00
draft: false
tags: ['gns3', 'jaringan', 'vlan']
cover: "/img/spvmadtdg/cover.png"
coverCaption: "dokumentasi pribadi"
---

## Pendahuluan

Assalamualaikum. Hai,Selamat datang, kali ini kita akan membahas tentang perbedaan vlan mode access dan vlan mode trunk pada managed switch. Pada pembahasan kali ini juga akan disertai dengan simulasi lanjutan pada artikel sebelumnya yaitu artikel [ini](/posts/simulasi-konfigurasi-vlan-dasar-di-gns3/) . Baiklah kita lanjutkan saja ke pembahasannya.

## VLAN Mode Access VS VLAN Mode Trunk

Secara singkat perbedaan antara port switch yang dikonfigurasikan sebagai mode access dan trunk terletak pada traffic yang diizinkan melewati port tersebut. Pada port switch yang dikonfigurasikan sebagai mode access, port tersebut hanya dapat dilewati oleh traffic sebuah vlan dengan id yang diset pada port tersebut saja, sedangkan pada mode trunk, port tersebut dapat dilewati oleh beberapa traffic vlan berbeda tergantung dari konfigurasi yang di sematkan pada port tersebut. Untuk lebih jelasnya, anda bisa membacanya pada artikel [berikut ini](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus5000/sw/layer2/503_n2_1/503_n2_1nw/Cisco_n5k_layer2_config_gd_rel_503_N2_1_chapter6.html#con_1206599)

## Persiapan

Disini kita akan melanjutkan simulasi dari artikel sebelumnya dengan penambahan 1 buah switch dan 2 buah VPC. antar switch kita hubungkan dengan port yang tersedia yang nantinya akan kita set sebagai mode trunk pada masing masing switch. Kemudian pada switch yang kedua kita hubungkan dengan 2 buah VPC, dimana PC5 akan diset sebagai VLAN 10 dan PC6 akan diset sebagai VLAN 20.

{{< figure src="/img/spvmadtdg/cover.png" alt="topologi" position="center" style="border-radius: 8px;" caption="topologi" >}}

Pada switch yang kedua juga nantinya kita harus menyamakan konfigurasi vlan yang tersedia seperti pada switch pertama dengan penambahan trunk pada port yang terhubung dengan switch pertama. baiklah kita lanjutkan ke tahap konfigurasi.

## Konfigurasi pada Switch

Pertama-tama lakukan console pada SW1 kemudian masuk ke mode `configuration terminal`. Lakukan konfigurasi port switch yang terhubung ke SW2 menjadi mode trunk. Pada simulasi ini port pada SW1 yang terhubung ke SW2 adalah fa1/4 yang menjadikan port tersebut dikonfigurasi menjadi mode trunk. Perintah yang digunakan adalah sebagai berikut:

{{< code language="bash" title="Konfigurasi trunk pada SW1" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
SW1# configure terminal
(config)# interface fa1/4
(config-if)# switchport mode trunk
(config-if)# switchport trunk allowed vlan all
(config-if)# exit
{{< /code >}}

kemudian lakukan verifikasi dengan mengetikkan perintah `show interface trunk` untuk melihat konfigurasi trunk pada SW1

{{< figure src="/img/spvmadtdg/sw1.png" alt="SW1 Trunk" position="center" style="border-radius: 8px;" caption="SW1 Trunk" >}}

Selanjutnya kita perlu untuk menyamakan informasi vlan pada SW1 di SW2. Lakukan konfigurasi seperti dibawah ini:
{{< code language="bash" title="Konfigurasi vlan pada SW2" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
SW2# configure terminal
(config)# hostname SW2
(config)# no ip domain lookup
(config)# VLAN 10
(config-vlan)# name finance
(config-vlan)# exit
(config)# VLAN 20
(config-vlan)# name IT
(config-vlan)# exit
{{< /code >}}

perintah hostname digunakan untuk mengganti nama hostname dari switch ke SW1, no ip domain lookup digunakan agar switch tidak mentranslasikan perintah yang salah / tidak ditemukan ke dalam IP. kemudian kita membuat 2 buah VLAN dengan perintah selanjutnya.

Selanjutnya lakukan konfigurasi masing-masing interface yang terhubung ke SW2 dengan menggunakan perintah dibawah ini:

{{< code language="bash" title="Konfigurasi lengkap interface vlan pada fa1/0 dan fa 1/1" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
(config)# interface fa1/0
(config-if)# switchport mode access
(config-if)# switchport access vlan 10
(config-if)# exit
(config)# interface fa1/1
(config-if)# switchport mode access
(config-if)# switchport access vlan 20
(config-if)# exit
{{< /code >}}

Kemudian lakukan konfigurasi port yang terhubung ke SW1, pada simulasi ini port yang terhubung adalah fa1/2. Port tersebut kita konfigurasikan ke mode trunk, perintah yang digunakan adalah sebagai berikut:

{{< code language="bash" title="Konfigurasi trunk pada SW2" id="1" expand="Show" collapse="Hide" isCollapsed="false" >}}
SW2# configure terminal
(config)# interface fa1/2
(config-if)# switchport mode trunk
(config-if)# switchport trunk allowed vlan all
(config-if)# exit
{{< /code >}}

{{< figure src="/img/spvmadtdg/sw2.png" alt="konfigurasi SW2" position="center" style="border-radius: 8px;" caption="konfigurasi SW2" >}}

## Konfigurasi pada PC5 dan PC6

Selanjutnya kita berikan pengalamatan IP pada PC5 dan PC6 yang terhubung ke SW2. Pada PC5 kita berikan ip 192.168.10.3/24 dan pada PC6 kita berikan ip 192.168.20.3/24. Jika konfigurasi benar maka akan tampil seperti gambar dibawah

PC5
{{< figure src="/img/spvmadtdg/pc5.png" alt="pc5" position="center" style="border-radius: 8px;" caption="pc5" >}}
PC6
{{< figure src="/img/spvmadtdg/pc6.png" alt="pc6" position="center" style="border-radius: 8px;" caption="pc6" >}}

## Testing
Kemudian lakukan testing dengan ping ke masing masing VPC dari PC5 dan PC6.

PC5
{{< figure src="/img/spvmadtdg/pc5-test.png" alt="pc5-test" position="center" style="border-radius: 8px;" caption="pc5 testing" >}}
PC6
{{< figure src="/img/spvmadtdg/pc6-test.png" alt="pc6-test" position="center" style="border-radius: 8px;" caption="pc6 testing" >}}

## Penutup

Sekian tutorial singkat yang bisa saya sampaikan kali ini, semoga bermanfaat. Kita bertemu lagi pada tutorial-tutorial berikutnya. wassalamualaikum.