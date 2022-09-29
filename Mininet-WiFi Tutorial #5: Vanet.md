## Mininet-WiFi Tutorial #5: Vanet

API Python pada Mininet-Wifi menambahkan sebuah fitur yang dapat kita manfaatkan untuk membuat sebuah skenario Vehicular Ad-Hoc Network (VANET), yaitu sebuah teknologi dimana kendaraan di jalan raya dapat saling terkoneksi satu sama lain. Teknologi VANET merupakan turunan dari teknologi Mobile Ad-Hoc Network (MANET). Untuk dapat menjalankan tutorial 5, kita harus menginstall aplikasi SUMO yang dapat menampilkan simulasi VANET secara graphical. 
Berikut adalah perintah yang digunakan pada Linux Ubuntu 22.04 LTS untuk menginstall repository SUMO: 

```
sudo add-apt-repository ppa:sumo/stable sudo 
apt-get update 
sudo apt-get install sumo sumo-tools sumo-doc 
```
Berikut merupakan syntax dari `vanet-sumo.py`:

```
#!/usr/bin/env python 
 

"""Sample file for VANET 
 
***Requirements***: 
Kernel version: 5.8+ (due to the 802.11p support) sumo 1.5.0 or higher sumo-gui  
Please consider reading https://mininet-wifi.github.io/80211p/ for 802.11p support 
"""  
from mininet.log import setLogLevel, info from mn_wifi.cli import CLI from mn_wifi.net import Mininet_wifi from mn_wifi.sumo.runner import sumo from mn_wifi.link import wmediumd, ITSLink 
from mn_wifi.wmediumdConnector import interference 
  def topology(): 
 
    "Create a network." 
    net = Mininet_wifi(link=wmediumd, wmediumd_mode=interference) 
     info("*** Creating nodes\n")     for id in range(0, 10):         net.addCar('car%s' % (id+1), wlans=2, encrypt=['wpa2', '']) 
     kwargs = {'ssid': 'vanet-ssid', 'mode': 'g', 'passwd': '123456789a', 
              'encrypt': 'wpa2', 'failMode': 'standalone', 
'datapath': 'user'} 
    e1 = net.addAccessPoint('e1', mac='00:00:00:11:00:01', channel='1', 
                            position='2600,3500,0', **kwargs)     e2 = net.addAccessPoint('e2', mac='00:00:00:11:00:02', channel='6', 
                            position='2800,3500,0', **kwargs)     e3 = net.addAccessPoint('e3', mac='00:00:00:11:00:03', channel='11', 
                            position='3000,3500,0', **kwargs)     e4 = net.addAccessPoint('e4', mac='00:00:00:11:00:04', channel='1', 
                            position='2600,3300,0', **kwargs)     e5 = net.addAccessPoint('e5', mac='00:00:00:11:00:05', channel='6', 
                            position='2800,3300,0', **kwargs)     e6 = net.addAccessPoint('e6', mac='00:00:00:11:00:06', channel='11', 
                            position='3000,3300,0', **kwargs) 
 
    info("*** Configuring Propagation Model\n") 
    net.setPropagationModel(model="logDistance", exp=2.8) 
     info("*** Configuring wifi nodes\n")     net.configureWifiNodes()  
    net.addLink(e1, e2)     net.addLink(e2, e3)     net.addLink(e3, e4)     net.addLink(e4, e5)     net.addLink(e5, e6)     for car in net.cars:         net.addLink(car, intf=car.wintfs[1].name,                     cls=ITSLink, band=20, channel=181)  
    # exec_order: Tells TraCI to give the current 
    # client the given position in the execution order. 
    # We may have to change it from 0 to 1 if we want to     # load/reload the current simulation from a 2nd client     net.useExternalProgram(program=sumo, port=8813, 
                           #config_file='map.sumocfg', # optional                            extra_params=["--start --delay 1000"],                            clients=1, exec_order=0) 
 
    info("*** Starting network\n")     net.build() 
     for enb in net.aps:         enb.start([]) 
     for id, car in enumerate(net.cars):         car.setIP('192.168.0.{}/24'.format(id+1),                   intf='{}'.format(car.wintfs[0].name))         car.setIP('192.168.1.{}/24'.format(id+1),                   intf='{}'.format(car.wintfs[1].name))  
    # Track the position of the nodes     nodes = net.cars + net.aps 
    net.telemetry(nodes=nodes, data_type='position',
    min_x=2200, min_y=2800,                   max_x=3200, max_y=3900) 
 
    info("*** Running CLI\n") 
    CLI(net) 
     info("*** Stopping network\n")     
net.stop() 
  if __name__ ==
  '__main__':     
  setLogLevel('info') 
  topology() 

```
 
Lalu, jalankan program Python vanet-sumo.py dengan perintah berikut lalu akan tampil hasilnya pada grafik dan SUMO. 

![image](https://user-images.githubusercontent.com/91620434/193066082-5b9d1202-929d-4db6-91ad-73edf6adc941.png)




