## Tutorial Mininet-WiFi #4: Stasion yang bergerak (Mobile)

Pada tutorial keempat ini, kita akan membuat sebuah skenario dimana sebuah station terhubung ke AP dan bergerak secara bebas. Station akan secara otomatis berganti dan terhubung ke AP terdekatnya (handover).  
Sebagai contohnya, kita akan membuat skenario dimana terdapat 3 AP (ap1, ap2, dan ap3) dengan jarak yang berbeda satu sama lain. Lalu, terdapat sebuah host h1 yang bertindak sebagai test server dan sebuah mobile station bernama sta1 yang bergerak melewati ketiga AP tersebut. File coding disimpan dengan nama line.py dengan baris perintah sebagai berikut: 

![image](https://user-images.githubusercontent.com/91620434/193062399-5e290333-8b46-4463-90ab-59deaf12838a.png)

Kemudian, kita jalankan program Python tersebut dengan perintah :

```
wifi:~$ sudo python line.py
```

sehingga terlihat hasilnya akan disimulasikan dalam bentuk grafik seperti pada gambar di bawah ini: 

![image](https://user-images.githubusercontent.com/91620434/193062609-8e1d8f5e-468e-4a56-95f9-31aae56145bc.png)

Pada awalnya, sta1 akan menunggu selama 20 detik, kemudian bergerak melintasi ap1 sampai ap3 (dari kiri ke kanan) sampai sta1 berada di luar coverage area dari ketiga AP. 
Kemudian, untuk melihat bagaimana sistem merespon trafik, jalankan beberapa komunikasi data dengan menjalankan ping antara host h1 dengan station sta1 ketika skenario dijalankan. Awali dengan menjalankan ulang program Python, kemudian jalankan iperf server dengan perintah:

```
mininet-wifi> sta1 iperf --server
```

Lalu dilanjutkan dengan buka xterm windows dari host h1:

```
mininet-wifi> xterm h1 
```

![image](https://user-images.githubusercontent.com/91620434/193063040-8dbb0bc6-8583-409d-a062-98a94d3b2f15.png)

Dari xterm window, jalankan perintah iperf berikut: 
```
# iperf --client 10.0.0.2 --time 60 --interval 2 
```

![image](https://user-images.githubusercontent.com/91620434/193063180-12eb2fda-4141-4cf8-9ad4-d0c1f3af0f7a.png)

Terlihat di awal dijalankannya perintah iperf masih terdapat transfer data antara h1 dan sta1, dan seiring berjalannya waktu ketika sta1 bergerak menjauhi h1 dan coverage area, kedua perangkat tersebut sudah tidak mengalami transfer data yang dilihat dari nilai TRANSFER dan BANDWIDTH yang tertera pada xterm window dari h1. 
