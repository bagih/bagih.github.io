---
title: "Simulasi Konfigurasi Dasar Jaringan Dengan Switch Di Gns3"
date: 2023-09-15T19:58:09+08:00
draft: false
cover: "/img/skdjdsdg/3.png"
coverCaption: "dokumentasi pribadi"
---

# simulasi konfigurasi dasar jaringan dengan switch di GNS3

Hai, selamat datang. pada kesempatan kali ini saya akan membagikan tutorial tentang konfigurasi dasar pada GNS3 dengan menggunakan 3 PC dan 1 switch. Berikut adalah tutorial lengkapnya

{{< figure src="/img/skdjdsdg/3.png" alt="Cover konfigurasi dasar jaringan" position="center" style="border-radius: 8px;" caption="Gambar 1. Cover konfigurasi dasar jaringan" >}}

## Persiapan
Disini kita menggunakan 3 buah VPC (Virtual PC) dan 1 buah switch yang unmanage. VPC disini disimulasikan sebagai perangkat end user yang terhubung ke jaringan (PC pengguna) dan switch disini  adalah perangkat jaringan yang menghubungkan beberapa PC / End device pengguna tersebut. Switch yang digunakan adalah switch Ethernet Switch pada halaman all Devices, switch ini bukan termasuk managed switch, jadi kita tidak bisa melakukan konfigurasi pada switch, disini switch bertindak sebagai perantara saja antara beberapa perangkat end user

Pengalamatan yang digunakan pada jaringan adalah menggunakan 192.168.1.0/24 , dimana nanti jaringan dapat menghandle sampai dengan 254 user. ip yang digunakan pada masing masing VPC berturut turut mulai dari PC1 adalah 192.168.1.1 sampai dengan 192.168.1.3.

## Memasukkan perangkat pada area workspace

Pertama tama, silahkan masukkan device device yang akan kita gunakan pada simulasi. Buka pada tab browse end devices / browse all devices pada tab sebelah kiri, kemudian drag perangkat VPCS sebanyak 3 buah dan Ethernet Switch sebanyak 1 buah. seperti pada gambar (2) dibawah ini:

{{< figure src="/img/skdjdsdg/1.png" alt="Gambar 2. Tampilan area workspace" position="center" style="border-radius: 8px;" caption="Gambar 2. Tampilan area workspace" >}}
Gambar 5. Hasil tampilan setelah diplay
sesuaikan perangkat pada area workspace, agar terlihat rapi anda bisa mengaktifkan fungsi snap to grid dan show grid yang terdapat pada menu View -> Snap to grid dan View -> Show the grid yang terdapat pada bar pada kiri atas.

Setelah dirasa rapi silahkan menambahkan kabel ke masing masing perangkat VPC ke switch dengan menggunakan menu Add a link pada bagian kiri, kemudian tambahkan kabel dengan mengklik pada bagian VPC kemudian pilih port yang tersedia kemudian klik lagi pada bagian switch dan pilih port yang tersedia, sesuaikan berurutan agar terlihat rapi. Jika sudah, maka akan terlihat seperti gambar (3) dibawah ini

{{< figure src="/img/skdjdsdg/2.png" alt="Gambar 3. Hasil tampilan" position="center" style="border-radius: 8px;" caption="Gambar 3. Hasil tampilan" >}}

Selanjutnya klik icon Play berwarna hijau yang terdapat bagian toolbar dan link pada semua device pada workspace akan berwarna hijau yang menandakan bahwa perangkat dan link pada setiap port aktif, namun saat ini setiap PC masih belum bisa berkomunikasi karena kita belum menambahkan pengalamatan pada masing masing PC.

{{< figure src="/img/skdjdsdg/3.png" alt="Gambar 4. Hasil tampilan setelah diplay" position="center" style="border-radius: 8px;" caption="Gambar 4. Hasil tampilan setelah diplay" >}}

## Konfigurasi masing-masing VPC

Disini kita akan melakukan konfigurasi pada setiap VPC yang terhubung ke jaringan. Konfigurasi yang kita lakukan adalah konfigurasi IP address sesuai dengan ketentuan yang kita buat sebelumnya. Klik kanan pada PC1 dan pilih console maka jendela console PC1 akan terbuka seperti pada gambar (5) dibawah ini 

{{< figure src="/img/skdjdsdg/4.png" alt="Gambar 5. Tampilan awal console PC1" position="center" style="border-radius: 8px;" caption="Gambar 5. Tampilan awal console PC1" >}}

Kemudian ketikkan perintah "ip 192.168.1.1/24" kemudian enter, perintah tersebut berfungsi untuk memberikan alamat IP berupa 192.168.1.1 dengan netmask 255.255.255.0 pada PC1. Konfirmasi bahwa IP telah terpasang pada PC1 dengan menggunakan perintah "show ip", maka PC1 akan menampilkan konfigurasi IP pada PC1. Jika konfigurasinya tepat maka akan tampil seperti pada gambar (6) dibawah:

{{< figure src="/img/skdjdsdg/5.png" alt="Gambar 6. Tampilan console pada PC1" position="center" style="border-radius: 8px;" caption="Gambar 6. Tampilan console pada PC1" >}}

Lanjutkan prosesnya seperti PC1 pada PC2 dan PC3 dengan PC2 menggunakan alamat IP 192.168.1.2/24 dan PC3 menggunakan alamat IP 192.168.1.3/24 seperti pada gambar (7 dan 8) dibawah ini

{{< figure src="/img/skdjdsdg/6.png" alt="Gambar 7. Tampilan console pada PC2" position="center" style="border-radius: 8px;" caption="Gambar 7. Tampilan console pada PC2" >}}
{{< figure src="/img/skdjdsdg/7.png" alt="Gambar 8. Tampilan console pada PC3" position="center" style="border-radius: 8px;" caption="Gambar 8. Tampilan console pada PC3" >}}

## Testing Hasil Konfigurasi

Selanjutnya lakukan testing pada konfigurasi dengan melakukan ping dari PC1 ke PC2 dan PC3. Lakukan console ke PC1 kemudian ketikkan perintah "ping 192.168.1.2" untuk melakukan ping ke PC2 dan "ping 192.168.1.3" untuk melakukan ping ke PC3. Jika berhasil maka akan tampil seperti gambar (9) dibawah ini

{{< figure src="/img/skdjdsdg/8.png" alt="Gambar 9. pengujian" position="center" style="border-radius: 8px;" caption="Gambar 9. pengujian" >}}

## Penutup

Sekian tutorial yang bisa saya sampaikan kali ini, semoga bermanfaat, kita bertemu lagi di tutorial tutorial berikutnya. Terimakasih :)
