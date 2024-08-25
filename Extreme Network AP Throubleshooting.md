
##### Check the Boot Parameters
<!-- CLI Command -->
```js
show boot-param
```js

<!-- Output -->
```js
boot parameters:
Device IP:          0.0.0.0
Netmask:            0.0.0.0
TFTP Server IP:     0.0.0.0
Gateway IP:         0.0.0.0
VLAN ID:            0
Native-VLAN ID:     0
Netboot Always:     Disabled
Netboot:            Disabled
Boot File:
Netdump:            Disabled
Netdump File:       03052306022375.netdump
Region Code:        World
Country Code:       410
```





---
## CAPWAP and Provisioning

##### What if the AP is Not Connecting to ExtremeCloud IQ 

1. Make sure the AP has an IP address
<!-- CLI Command -->
```shell
show l3 interface
```

<!-- Output -->
```js
Name                  IP Address      Mode    VLAN       MAC       State
----------- --------------- -------- ------ -------------- -----
mgt0                192.168.1.42       -         1  4c23:1ad2:ea40   U
```

2. Find the default gateway and make sure it is reachable
<!-- CLI Command -->
```shell
show ip route
```

<!-- Output -->
```js
Ref=references; Iface=interface;
U=route is up;H=target is a host; G=use gateway;
Destination     Gateway         Netmask         Flags Metric Ref    Use Iface
--------------- --------------- --------------- ----- ------ ------ --- -----
0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 mgt0
127.0.0.0       0.0.0.0         255.255.255.0   U     0      0        0 lo
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 mgt0
```

<!-- CLI Command -->
```shell
ping 192.168.1.1
```

<!-- Output -->
```js
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=1.08 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.935 ms
64 bytes from 192.168.1.1: icmp_seq=3 ttl=64 time=0.631 ms
64 bytes from 192.168.1.1: icmp_seq=4 ttl=64 time=5.88 ms
64 bytes from 192.168.1.1: icmp_seq=5 ttl=64 time=18.2 ms

--- 192.168.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4003ms
rtt min/avg/max/mdev = 0.631/5.350/18.216/6.720 ms
```

3. Make sure the AP has DNS server addresses
<!-- CLI Command -->
```shell
show dns
```

<!-- Output -->
```js
DNS server from DHCP:
Domain name suffix:
Primary   : 80.58.61.250
Secondary : 80.58.61.254
Tertiary  : 0.0.0.0
Operation Status:
Tracking Primary DNS : Disable
Switch DNS           : Off
```

4. Make sure the Redirector can be resolved and is reachable
<!-- CLI Command -->
```shell
capwap ping redirector.aerohive.com
```

<!-- Output -->
```js
CAPWAP ping parameters:
    Destination server: redirector.aerohive.com (54.172.0.252)
    Destination port: 12222
    Count: 5
    Size: 56(82) bytes
    Timeout: 5 seconds
--------------------------------------------------
CAPWAP ping result:
    82 bytes from 54.172.0.252 udp port 12222: seq=1 time=109.481 ms
    82 bytes from 54.172.0.252 udp port 12222: seq=2 time=109.660 ms
    82 bytes from 54.172.0.252 udp port 12222: seq=3 time=109.213 ms
    82 bytes from 54.172.0.252 udp port 12222: seq=4 time=109.124 ms
    82 bytes from 54.172.0.252 udp port 12222: seq=5 time=109.417 ms
    ------- redirector.aerohive.com CAPWAP ping statistics -------
    5 packets transmitted, 5 received, 0.00% packet loss, time 5556.925ms
    rtt min/avg/max = 109.124/109.379/109.660 ms
```

5. Show CAPWAP client
<!-- CLI Command -->
```shell
capwap ping redirector.aerohive.com
```

<!-- Output -->
```js
CAPWAP client:   Enabled
CAPWAP transport mode:  UDP
RUN state: Connected securely to the CAPWAP server
CAPWAP client IP:        192.168.1.42
CAPWAP server IP:        3.67.81.102
HiveManager Primary Name:fra-cws-4.extremecloudiq.com
HiveManager Backup Name: fra-cwm.extremecloudiq.com
CAPWAP Default Server Name: redirector.aerohive.com
Virtual HiveManager Name:
Server destination Port: 12222
CAPWAP send event:       Enabled
CAPWAP DTLS state:       Enabled
CAPWAP DTLS negotiation: Enabled
     DTLS next connect status:   Enable
     DTLS always accept bootstrap passphrase: Enabled
     DTLS session status: Connected
     DTLS key type: passphrase
     DTLS session cut interval:     5 seconds
     DTLS handshake wait interval: 60 seconds
     DTLS Max retry count:          3
     DTLS authorize failed:         0
     DTLS reconnect count:          0
Discovery interval:      5 seconds
Heartbeat interval:     30 seconds
Max discovery interval: 10 seconds
Neighbor dead interval:105 seconds
Silent interval:        15 seconds
Wait join interval:     60 seconds
Discovery count:         0
Max discovery count:     3
Retransmit count:        0
Max retransmit count:    2
Primary server tries:    0
Backup server tries:     0
Keepalives lost/sent:    1/4127
Event packet drop due to buffer shortage: 0
Event packet drop due to loss connection: 3
```

6. Make sure the primary CAPWAP server is reachable
<!-- CLI Command -->
```shell
capwap ping fra-cws-4.extremecloudiq.com
```

<!-- Output -->
```python
CAPWAP ping parameters:
    Destination server: fra-cws-4.extremecloudiq.com (3.67.81.102)
    Destination port: 12222
    Count: 5
    Size: 56(82) bytes
    Timeout: 5 seconds
--------------------------------------------------
CAPWAP ping result:
    82 bytes from 3.67.81.102 udp port 12222: seq=1 time=42.515 ms
    82 bytes from 3.67.81.102 udp port 12222: seq=2 time=40.942 ms
    82 bytes from 3.67.81.102 udp port 12222: seq=3 time=41.324 ms
    82 bytes from 3.67.81.102 udp port 12222: seq=4 time=41.131 ms
    82 bytes from 3.67.81.102 udp port 12222: seq=5 time=46.213 ms
    ------- fra-cws-4.extremecloudiq.com CAPWAP ping statistics -------
    5 packets transmitted, 5 received, 0.00% packet loss, time 5265.764ms
    rtt min/avg/max = 40.942/42.425/46.213 ms
```

7. Restart the CAPWAP process
<!-- CLI Command -->
```shell
no capwap client enable
```

<!-- CLI Command -->
```shell
capwap client enable
```

8. Reboot the device
<!-- CLI Command -->
```shell
reboot
```

---

---
##### AMRP (Auto Mobility Routing Protocol)

1. AMRP Link State Database
<!-- CLI Command -->
`show amrp node all`

<!-- Output -->
```shell

```

2. AMRP Distributes Roaming Cache Information
<!-- CLI Command -->
`show roam cache mac xxxx:xxxx:xxxx`

<!-- Output -->
```shell

```

##### AP Roaming Cache Syncronization
<!-- CLI Command -->
`show roam cache`

<!-- Output -->
```shell

```
