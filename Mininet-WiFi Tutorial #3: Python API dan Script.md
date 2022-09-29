## Mininet-WiFi Tutorial #3: Python API dan Script

Pada contoh di bawah ini, dibuat program Python yang akan menghasilkan dua station yang terhubung ke dua buah AP, dan mengatur posisi node dan jangkauan radio sehingga kita dapat melihat bagaimana pengaruhnya ketika program dijalankan (topologi disimulasikan). Program akan ditampilkan pada gambar di bawah ini dan disimpan dengan nama position-test.py: 

![image](https://user-images.githubusercontent.com/91620434/193061014-7203c467-70c2-44e6-8112-67c394d20646.png)

Kemudian, kita jalankan program Python tersebut dengan perintah :
```
wifi:~$ sudo python position-test.py 
```

sehingga terlihat hasilnya akan disimulasikan dalam bentuk grafik seperti pada gambar di bawah ini: 

![image](https://user-images.githubusercontent.com/91620434/193061204-28931b43-9850-47b0-b040-a3990274b754.png)

Saat program dijalankan, kita dapat memeriksa konfigurasi dan informasi mengenai jaringan yang dibangun, baik dari baris perintah Mininet-WiFi atau dari interpreter Python. 

1.	Memeriksa parameter di setiap AP mininet-wifi> py (nama_AP).params 

![image](https://user-images.githubusercontent.com/91620434/193061342-bcdafa28-e906-40dc-8e17-7bbde279135f.png)

2.	Jarak antar perangkat 
Perintah distance digunakan untuk menampilkan jarak antara dua nodes : 

```
mininet-wifi> distance ap1 sta2
```

![image](https://user-images.githubusercontent.com/91620434/193061556-1fe66a22-11b0-44c9-94b7-906bb5754052.png)

3.	Memberikan perintah pada node 
Saat menjalankan program, kita dapat membuat perubahan konfigurasi pada node untuk mengimplementasikan beberapa perintah tambahan. Misalnya, untuk melihat informasi tentang interface WLAN di sta1, jalankan perintah :

```
mininet-wifi> sta1 iw dev sta1-wlan0 link
```

![image](https://user-images.githubusercontent.com/91620434/193061745-eeb4a4b1-c662-4a9e-8150-5bd2ae1f5a1e.png)

Namun, terdapat beberapa perintah yang terdapat pada laman tutorial yang tidak dapat digunakan misalnya seperti perintah `position, info, range, associatedStations, associatedAp,` dan lainnya. Kemudian, pada contoh program yang diberikan juga masih menggunakan perintah-perintah pada `library` Mininet, sehingga beberapa format dan perintah pada program python harus diubah terlebih dahulu menyesuaikan dengan `library` atau `repository` dari Mininet-Wifi. 
