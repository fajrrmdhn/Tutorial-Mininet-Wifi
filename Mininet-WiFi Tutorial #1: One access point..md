# Mininet-WiFi Tutorial #1: One access point

Konsep jaringan paling sedarhana adalah topologi default, topologi ini terdiri dari titik akses nirkabel dengan dua stasiun. Akses point disini dianalogikan sebagai saklar yang terhubung langsung dengan pengontrol dan stasiun diibaratkan sebagai tuan rumah.
Lab sederhana ini akan memungkinkan kita untuk mendemonstrasikan cara menangkap lalu lintas kontrol nirkabel dan akan mendemonstrasikan cara titik akses berkemampuan OpenFlow menangani lalu lintas WiFi pada antarmuka wlan.

> Memantau Wireless control traffic pada Mininet-Wifi

Perintah untuk melihat wireless control traffic menggunakan WIRESHARK :
```
sudo wireshark &
```

Lalu jalankan Mininet-WiFI dengan skenario default menggunakan perintah berikut :

```
sudo mn --wifi
```
Lalu izinkan `hwsim0` interface. The `hwsim0` merupakan interface dari software interface yang dibuat oleh Mininet-WiFi yang menyalin semua wireless traffic kedalam semua interfaces jaringan nirkabel virtual ke dalam skenario jaringan. 

```
mininet-wifi> sh ifconfig hwsim0 up
```
Sekarang, di Wireshark, refresh interfaces dan kemudian mulai snapshot paket pada antarmuka `hwsim0` 

![mn-wifi-008](https://user-images.githubusercontent.com/91620434/193039839-82cf8a2b-ef73-47f4-a009-0bfdd638d005.png)

Untuk melihat traffic control wireless, selanjutnya lakukan perintah PING :
```
mininet-wifi> sta1 ping sta2
```
Pada windows wireshark lihat bingkai wireless dan pake ICMP yang dienkapsulasi dalam bingkai wireless yang melewati interfaces `hwsim0`

![mn-wifi-010](https://user-images.githubusercontent.com/91620434/193040182-f928fc9a-5f16-4027-a617-8594ace0880d.png)

Akses poin wireless dan OpenFlow

Dalam skenario sederhana ini, titik akses hanya memiliki satu antarmuka, ap1-wlan0 . Secara default, stasiun yang terkait dengan titik akses terhubung dalam mode infrastruktur sehingga lalu lintas nirkabel antar stasiun harus melewati titik akses. Jika titik akses bekerja mirip dengan sakelar di Mininet standar, kami berharap untuk melihat pesan OpenFlow yang dipertukarkan antara titik akses dan pengontrol setiap kali titik akses melihat lalu lintas yang alirannya belum ditetapkan.

Untuk melihat paket OpenFlow, hentikan pengambilan Wireshark dan alihkan ke antarmuka loopback . Mulai menangkap lagi pada antarmuka loopback . Gunakan filter OpenFlow_1.0 untuk hanya melihat pesan OpenFlow.

Kemudian, mulai beberapa lalu lintas yang berjalan dengan perintah ping dan lihat pesan OpenFlow yang ditangkap di Wireshark.

```
mininet-wifi> sta1 ping sta2 
```

Untuk memperiksa apakah flows sudah dibuat dalam akses poin

```
mininet-wifi> dpctl dump-flows
*** ap1 ------------------------------------------
NXST_FLOW reply (xid=0x4):
```

Stop Tutorial
Stop the Mininet `ping` dengan menekan `Ctrl-C`
Di jendela Wireshark, berhenti merekam dan keluar dari Wireshark.

Hentikan Mininet-Wifi dan bersihkan sistem dengan perintah berikut:
```
mininet-wifi> exit
wifi:~$ sudo mn -c
```
