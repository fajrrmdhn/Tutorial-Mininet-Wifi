# Mininet-WiFi Tutorial #2: More Than one access

Pada tutorial #2 ini digunakan beberapa AP, yang akan menunjukkan cara
membuat topologi jaringan yang lebih kompleks sehingga kita dapat bereksperimen
dengan skenario mobilitas (mobility) dasar. Kemudian akan membahas lebih lanjut
mengenai OpenFlow dan menunjukkan bagaimana ‘Mininet reference control
bekerja’ di Mininet-WiFi.

> Merekam traffic jaringan wireless pada Mininet-Wifi

1. Jalankan Mininet-Wifi dengan skenario linear topologi tiga buah AP
menggunakan perintah berikut:

```
wifi:~$ sudo mn --wifi --topo linear,3
```

![image](https://user-images.githubusercontent.com/91620434/193055492-6593aca8-49a3-4982-9f30-65b0b714b149.png)


Kita dapat memastikan bahwa konfigurasi Mininet-Wifi telah sesuai dengan menggunakan perintah net atau dump. Dan untuk mengecek setiap AP yang terdeteksi di setiap station, gunakan perintah `iw scan`.

![image](https://user-images.githubusercontent.com/91620434/193055640-2685fdb4-c316-4167-aa0e-f984a2b1fe55.png)

2.	Untuk melihat AP yang sedang terkoneksi dan terhubung ke station digunakan perintah `iw link`. Sebagai contoh, perintah di bawah ini digunakan untuk melihat AP yang terhubung ke sta1: 

```
mininet-wifi> sta1 iw dev sta1-wlan0 link
```

![image](https://user-images.githubusercontent.com/91620434/193055830-615f7a9e-9540-4d5d-9ba2-570422a0de2d.png)


> Skenario/konsep mobilitas sederhana 

Pada contoh ini, sebuah station akan terkoneksi ke AP yang berbeda, digunakan perintah iw untuk mengganti AP yang dikoneksikan. Misalnya, sta1 saat ini terkoneksi ke ap1 akan kita ganti menjadi terkoneksi ke ap2, dan digunakan perintah berikut:

```
mininet-wifi> sta1 iw dev sta1-wlan0 disconnect 
mininet-wifi> sta1 iw dev sta1-wlan0 connect ssid_ap2 
```
![image](https://user-images.githubusercontent.com/91620434/193056343-af405be7-d466-423d-b297-0c08fbc047cf.png)

> OpenFlow pada konsep mobilitas 
Pertama, periksa alamat IP pada sta1 dan sta3 sehingga kita tahu parameter mana yang akan digunakan dalam pengujian ini. Cara termudah untuk melihat semua alamat IP adalah dengan menjalankan perintah dump :

![image](https://user-images.githubusercontent.com/91620434/193056637-7e354652-fdd9-4694-8034-4123240d7880.png)

Dan dapat dilihat pada gambar di atas bahwa sta1 memiliki alamat IP 10.0.0.1 dan sta3 memiliki IP 10.0.0.3. Lalu, kita buka Wireshark, dan menjalankan xterm window untuk sta3 dan lakukan ping ke IP 10.0.0.1 seperti berikut: 

```
mininet-wifi> xterm sta3 
root@mininet-wifi:~# ping 10.0.0.1 (on xterm window) 
```

Kemudian, pada Wireshark digunakan interface `loopback` dengan memfilter pesan `openflow_v1` untuk menampilkan OpenFlow packet. 

![image](https://user-images.githubusercontent.com/91620434/193056972-3c2fd182-6bed-4a07-90bf-e919859ee34e.png)

Sekarang, di Mininet CLI, periksa flow pada setiap switch dengan perintah `dpctl dump-flows`. 

![image](https://user-images.githubusercontent.com/91620434/193057132-17ab16b4-f701-4f50-b674-55b0e494c456.png)

Terlihat flow pada ap2 dan ap3, tetapi tidak pada ap1. Hal ini karena sta1 terhubung ke ap2 dan sta3 terhubung ke ap3 sehingga semua lalu lintas hanya melewati ap2 dan ap3.  
Hapus terlebih dahulu flow pada AP dengan perintah berikut:

```
mininet-wifi> dpctl del-flows 
```

Kemudian, pindahkan `sta1` kembali ke titik akses `ap1` dengan perintah berikut: 

```
mininet-wifi> sta1 iw dev sta1-wlan0 disconnect 
mininet-wifi> sta1 iw dev sta1-wlan0 connect ssid_ap1 
```

![image](https://user-images.githubusercontent.com/91620434/193057476-57a8fe26-b881-4b22-9625-c4a48259a6b1.png)


Kita telah melihat bagaimana ‘Mininet reference controller’ bekerja di MininetWiFi. ‘Mininet reference controller’ tidak memiliki kemampuan untuk mendeteksi kondisi ketika stasiun berpindah dari satu AP ke AP lainnya. Ketika ini terjadi, kita harus menghapus flow yang ada sehingga flow baru dapat dibuat














