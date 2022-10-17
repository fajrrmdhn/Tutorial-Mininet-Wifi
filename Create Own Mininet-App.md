File Python dibawah merupakan Tugas untuk memenuhi Mata Kuliah Wireless Network
```
#!/usr/bin/env python
import sys

from mininet.log import setLogLevel, info
from mn_wifi.cli import CLI
from mn_wifi.net import Mininet_wifi

def topology(args):

    net = Mininet_wifi()

    info("* Creating nodes\n")
    h1 = net.addHost( 'h1', mac='00:00:00:00:00:01', ip='192.168.101.10/24' )
    sta1 = net.addStation( 'sta1', mac='00:00:00:00:00:02', ip='192.168.102.11/24', range='25' )
    ap1 = net.addAccessPoint( 'ap1', ssid= 'JeyCT-01', mode= 'g', channel= '1', position='50,150,0', range='60' )
    ap2 = net.addAccessPoint( 'ap2', ssid= 'JeyCT-02', mode= 'g', channel= '1', position='125,385,0', range='60' )
    ap3 = net.addAccessPoint( 'ap3', ssid= 'JeyCT-03', mode= 'g', channel= '1', position='250,250,0', range='60' )
    ap4 = net.addAccessPoint( 'ap4', ssid= 'JeyCT-04', mode= 'g', channel= '1', position='385,385,0', range='60' )
    ap5 = net.addAccessPoint( 'ap5', ssid= 'JeyCT-05', mode= 'g', channel= '1', position='465,145,0', range='60' )
    c1 = net.addController( 'c1' )

    info("* Configuring propagation model\n")
    net.setPropagationModel(model="logDistance", exp=4.5)

    info("* Configuring wifi nodes\n")
    net.configureWifiNodes()

    info("* Associating and Creating links\n")
    net.addLink(ap1, h1)
    net.addLink(ap1, ap2)
    net.addLink(ap2, ap3)
    net.addLink(ap3, ap4)
    net.addLink(ap4, ap5)
   

    if '-p' not in args:
        net.plotGraph(max_x=500, max_y=500)

    net.setMobilityModel(time=0, model='RandomDirection',
                         max_x=500, max_y=500, seed=20)
    
    info("* Starting network\n")
    net.build()
    c1.start()
    ap1.start([c1])
    ap2.start([c1])
    ap3.start([c1])
    ap4.start([c1])
    ap5.start([c1])

    info("* Running CLI\n")
    CLI( net )

    info("* Stopping network\n")
    net.stop()

if __name__ == '__main__':
    setLogLevel( 'info' )
    topology(sys.argv)
    ```
