# izBalancing
izBalancing allow your linux firewall to become a multihomed input/output loadbalanced internet gateway with failover facility

## What is this?
This bash script allow you to easly and fastly configure a complex Load Balancing Multi Homed Internet Gateway
for inbound and outbound traffic

## Key Features:
- Multiple balanced default gateway configuration
- Load Balanced outgoing connections from LAN to INTERNET connections
- Management of multiple incoming connection from many INTERNET ISP lines to DMZ/LAN Servers
- SystemV compliant script... you can run easly at boot up (like Red Hat, Fedora, SuSE, Mandrake, etc...)
- Automatically discover your local IP addresses... you can change your IP without reconfigure this script, just restart
- Start and Stop cleanly your multihomed configuration with simple command (izbalancing start|stop|restart)
- Adding new internet connections is very fast and easy
- You must provide the interface name, router ip address and a descriptive name only
- If the izping daemon is installed into /etc/rc.d/init.d/ it will be used for automatic failover and recovery of internet lines


## Requirements:
- GNU/Linux Firewall running Kernel >=2.6.10 (with iptables module CONNMARK available)
- Bash Shell >= 2.0
- Standard GNU/Linux coreutils utilities (cat, echo, grep, if, etc...)
- GNU Version of awk and sed utilities
- GNU/Linux Netfilter user space utilities (iptables >= 1.2.11)
- iproute2 utilities
- Two or more Internet connections (also from different ISPs and IP classes)
- An ethernet card for each ISP Router

## Tested On:
- GNU/Linux CentOS 6 with 3 internet connections

## Network Topology Example:
```
+----------+                       +-------------------+  192.168.254.254+----------+             +-------+                
| Server x +<--+                   | 192.168.254.1:eth1+<--------------->+ Router 1 +<----------->+ ISP 1 +<--+            
+----------+   |                   |                   |                 +----------+ ADSL line 1 +-------+   |            
               |                   |    GNU/Linux      |                                                      |            
+----------+   |   192.168.1.1:eth0|                   |  192.168.253.254+----------+             +-------+   |            
| Server y +<--+---+LAN+---------->+ 192.168.253.1:eth2+<--------------->+ Router 2 +<----------->+ ISP 2 +<--+---->INTERNET
+----------+   |                   |                   |                 +----------+ HDSL line 2 +-------+   |            
               |                   |     Firewall      |                                                      |            
+----------+   |                   |                   |  192.168.nnn.nnn+----------+             +-------+   |            
| Server z +<--+                   | 192.168.nnn.n:ethn+<--------------->+ Router n +<----------->+ ISP n +<--+            
+----------+                       +-------------------+                 +----------+  T3 line n  +-------+                
```

## Installation:
- If using a Red Hat Linux based distribution, just copy the izbalancing script into `/etc/init.d/` and run:
```
  chmod 755 /etc/init.d/izbalancing
  chkconfig izbalancing on
```
- Configure or add the following variables into this script:
```
  GATEWAYS="interface1:gatewayip1:name1 interface2:gatewayip2:name2 interface3:gatewayip3:name3"
  BALANCED="name1 name2"
  VERIFY_HOSTS="ip1 ip2 ip3"
  FAILOVER_HOSTS="ip4 ip5 ip6"
```
- Comment out the iptables rules as you need

- usable special variables created on demand using the order specified in the GATEWAYS variable:
```
  $GWIFx = interface name (ex. GWIF1=eth1, GWIF2=eth2 so on...)
  $GWIPx = gateway ip address (ex. GWIP1=10.1.1.1, GWIP2=10.1.2.1 so on...)
```

- If the `izping` daemon script is installed into `/etc/init.d/`, izbalancing will use it to make automatic failover and recovery of lines, checking the ip addresses specified into `VERIFY_HOSTS` and `FAILOVER_HOSTS` variables
