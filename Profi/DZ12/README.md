# **VPN. GRE. DmVPN**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/raw/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)



## **Задачи:**
+ ### Настроите GRE между офисами Москва и С.-Петербург.
+ ### Настроите DMVMN между Москва и Чокурдах, Лабытнанги





## **Решение**

 

## Настройки:

### **Москва:**



### **R14:**
```
interface Loopback0
 ip address 14.14.14.14 255.255.255.255
 ip ospf 1 area 0
!
interface Tunnel0
 ip address 99.99.99.14 255.255.255.0
 no ip redirects
 ip nhrp map multicast dynamic
 ip nhrp map 99.99.99.15 77.15.2.1
 ip nhrp map multicast 77.15.2.1
 ip nhrp network-id 99
 ip nhrp nhs 99.99.99.15
 ip nhrp registration no-unique
 tunnel source Ethernet0/2
 tunnel mode gre multipoint
!
interface Ethernet0/0
 ip address 192.168.0.18 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
 ntp broadcast client
!
interface Ethernet0/1
 ip address 192.168.0.68 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
 ntp broadcast client
!
interface Ethernet0/2
 ip address 77.14.2.1 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 192.168.0.81 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 1 area 101
!
interface Ethernet1/0
 ip address 192.168.0.178 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
 ntp broadcast client
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown
!
router ospf 1
 router-id 14.14.14.14
 area 101 stub no-summary
 default-information originate
!
router bgp 1001
 bgp router-id 14.14.14.14
 bgp log-neighbor-changes
 network 77.14.2.0 mask 255.255.255.0
 neighbor 77.14.2.2 remote-as 101
 neighbor 77.14.2.2 route-map transit out
 neighbor 150.150.150.150 remote-as 1001
 neighbor 150.150.150.150 update-source Loopback0
 neighbor 150.150.150.150 next-hop-self
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
!
no ip http server
no ip http secure-server
ip nat inside source list PAT interface Ethernet0/2 overload
ip nat inside source static 192.168.0.82 77.14.2.199
ip route 192.168.0.0 255.255.0.0 Null0
!
ip access-list standard PAT
 permit 192.168.0.0 0.0.255.255
!
!
route-map transit permit 5
 match as-path 1




```

```
R14#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel0, IPv4 NHRP Details
Type:Hub/Spoke, NHRP Peers:3,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 77.15.2.1           99.99.99.15    UP 00:30:04     S
     1 89.25.1.2           99.99.99.27    UP 00:31:41     D
     1 14.26.1.2           99.99.99.28    UP 00:30:23     D



```




### **R15**:

```
interface Loopback0
 ip address 15.15.15.15 255.255.255.255
 ip ospf 1 area 0
!
interface Tunnel0
 ip address 99.99.99.15 255.255.255.0
 no ip redirects
 ip nhrp map multicast dynamic
 ip nhrp network-id 99
 ip nhrp registration no-unique
 tunnel source Ethernet0/2
 tunnel mode gre multipoint
!
interface Tunnel1
 ip address 88.88.88.15 255.255.255.0
 tunnel source Ethernet0/2
 tunnel destination 100.100.100.100
!
interface Ethernet0/0
 ip address 192.168.0.98 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
 ntp broadcast client
!
interface Ethernet0/1
 ip address 192.168.0.34 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
 ntp broadcast client
!
interface Ethernet0/2
 ip address 77.15.2.1 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 192.168.0.113 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 1 area 102
!
interface Ethernet1/0
 ip address 192.168.0.177 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
 ntp broadcast client
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown
!
router ospf 1
 router-id 15.15.15.15
 priority 120
 area 102 filter-list prefix 102 in
 default-information originate
!
router bgp 1001
 bgp router-id 15.15.15.15
 bgp log-neighbor-changes
 network 77.15.2.0 mask 255.255.255.0
 neighbor 77.15.2.2 remote-as 301
 neighbor 77.15.2.2 route-map transit out
 neighbor 140.140.140.140 remote-as 1001
 neighbor 140.140.140.140 update-source Loopback0
 neighbor 140.140.140.140 next-hop-self
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
!
no ip http server
no ip http secure-server
ip nat inside source list PAT interface Ethernet0/2 overload
ip nat inside source static 192.168.0.114 77.15.2.20
ip route 192.168.0.0 255.255.0.0 Null0
!
ip access-list standard PAT
 permit 192.168.0.0 0.0.255.255
!
!
ip prefix-list 102 seq 5 deny 192.168.0.80/28
ip prefix-list 102 seq 10 permit 0.0.0.0/0 le 32
!
route-map transit permit 5
 match as-path 1
!
route-map transit permit 10
 set local-preference 200


```

```
R15#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel0, IPv4 NHRP Details
Type:Hub, NHRP Peers:3,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 77.14.2.1           99.99.99.14    UP 00:32:14     D
     1 89.25.1.2           99.99.99.27    UP 00:33:58     D
     1 14.26.1.2           99.99.99.28    UP 00:32:36     D

```
### **R15(ping R18)**:
```
R15#ping 100.100.100.100
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 100.100.100.100, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/3 ms
R15#
```


### **R18**:

```
interface Loopback0
 ip address 180.180.180.180 255.255.255.255
!
interface Loopback1
 no ip address
 ipv6 address 2001::180/128
 ipv6 enable
!
interface Loopback3
 ip address 100.100.100.100 255.255.255.255
!
interface Tunnel0
 ip address 88.88.88.18 255.255.255.0
 tunnel source Loopback3
 tunnel destination 77.15.2.1
!
interface Ethernet0/0
 ip address 172.16.0.1 255.255.255.192
 ip nat inside
 ip virtual-reassembly in
 ipv6 address FE80::4 link-local
 ipv6 address 2001:CBD8:ACAD:1::2/64
!
interface Ethernet0/1
 ip address 172.16.0.65 255.255.255.192
 ip nat inside
 ip virtual-reassembly in
 ipv6 address FE80::3 link-local
 ipv6 address 2001:CBD8:ACAD:4::2/64
!
interface Ethernet0/2
 ip address 78.24.3.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 78.26.3.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 78
  !
  topology base
   redistribute static
  exit-af-topology
  network 172.16.0.0 0.0.0.63
  network 172.16.0.64 0.0.0.63
  network 180.180.180.180 0.0.0.0
  eigrp router-id 18.18.18.18
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 78
  !
  af-interface Ethernet0/0
   summary-address 2001:CBD8:ACAD::/64
  exit-af-interface
  !
  topology base
   redistribute static
  exit-af-topology
  eigrp router-id 18.18.18.18
 exit-address-family
!
router bgp 2042
 bgp router-id 18.18.18.18
 bgp log-neighbor-changes
 bgp bestpath as-path multipath-relax
 network 50.50.50.0 mask 255.255.255.0
 network 78.24.3.0 mask 255.255.255.0
 network 78.26.3.0 mask 255.255.255.0
 network 100.100.100.100 mask 255.255.255.255
 neighbor 78.24.3.1 remote-as 520
 neighbor 78.24.3.1 prefix-list 50 out
 neighbor 78.26.3.1 remote-as 520
 neighbor 78.26.3.1 prefix-list 50 out
 maximum-paths 2
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat pool SPB 50.50.50.5 50.50.50.10 netmask 255.255.255.0
ip nat inside source list SPB pool SPB overload
ip route 0.0.0.0 0.0.0.0 Null0
ip route 50.50.50.0 255.255.255.0 Null0
ip route 172.16.0.0 255.255.0.0 Null0
!
ip access-list standard SPB
 permit 172.16.0.0 0.0.255.255
!
!
ip prefix-list 50 seq 5 permit 50.50.50.0/24
ip prefix-list 50 seq 10 permit 78.24.3.0/24
ip prefix-list 50 seq 15 permit 78.26.3.0/24
ip prefix-list 50 seq 20 permit 100.100.100.100/32
ipv6 route ::/0 Loopback1


```







### **R27**:

```
interface Tunnel0
 ip address 99.99.99.27 255.255.255.0
 no ip redirects
 ip nhrp map 99.99.99.15 77.15.2.1
 ip nhrp map multicast 77.15.2.1
 ip nhrp map 99.99.99.14 77.14.2.1
 ip nhrp map multicast 77.14.2.1
 ip nhrp network-id 99
 ip nhrp nhs 99.99.99.15
 ip nhrp nhs 99.99.99.14
 ip nhrp registration no-unique
 tunnel source Ethernet0/0
 tunnel mode gre multipoint

```

```
R27#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel0, IPv4 NHRP Details
Type:Spoke, NHRP Peers:2,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 77.14.2.1           99.99.99.14    UP 00:38:23     S
     1 77.15.2.1           99.99.99.15    UP 00:37:41     S

```

### **R28**:

```
track 10 ip sla 10 reachability
 delay down 90 up 90
!
track 20 ip sla 20 reachability
 delay down 90 up 90
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Tunnel0
 ip address 99.99.99.28 255.255.255.0
 no ip redirects
 ip nhrp map 99.99.99.15 77.15.2.1
 ip nhrp map multicast 77.15.2.1
 ip nhrp map 99.99.99.14 77.14.2.1
 ip nhrp map multicast 77.14.2.1
 ip nhrp network-id 99
 ip nhrp nhs 99.99.99.15
 ip nhrp nhs 99.99.99.14
 ip nhrp registration no-unique
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
!
interface Ethernet0/0
 ip address 14.26.1.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/1
 ip address 14.25.3.2 255.255.255.0
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/2
 no ip address
!
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 14.28.2.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip policy route-map VLAN10
!
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 14.28.3.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly in
 ip policy route-map VLAN20
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source route-map PAT10 interface Ethernet0/0 overload
ip nat inside source route-map PAT20 interface Ethernet0/1 overload
ip route 0.0.0.0 0.0.0.0 14.26.1.1 30 track 10
ip route 0.0.0.0 0.0.0.0 14.25.3.1 20 track 20
!
ip access-list standard VLAN10
 permit 14.28.2.0 0.0.0.255
ip access-list standard VLAN20
 permit 14.28.3.0 0.0.0.255
!
ip access-list extended VLAN10-EX
 permit ip any any
ip access-list extended VLAN20-EX
 permit ip any any
!
ip sla 10
 icmp-echo 14.26.1.1 source-ip 14.26.1.2
 threshold 500
 timeout 1000
 frequency 5
ip sla schedule 10 life forever start-time now
ip sla 20
 icmp-echo 14.25.3.1 source-ip 14.25.3.2
 threshold 500
 timeout 1000
 frequency 5
ip sla schedule 20 life forever start-time now
!
route-map VLAN10 deny 10
 match ip address VLAN20
!
route-map VLAN10 permit 20
 match ip address VLAN10-EX
 set ip next-hop verify-availability 14.26.1.1 10 track 10
 set ip next-hop verify-availability 14.25.3.1 20 track 20
!
route-map VLAN20 deny 10
 match ip address VLAN10
!
route-map VLAN20 permit 20
 match ip address VLAN20-EX
 set ip next-hop verify-availability 14.25.3.1 10 track 20
 set ip next-hop verify-availability 14.26.1.1 20 track 10
!
route-map PAT10 permit 10
 match ip address VLAN10
 match interface Ethernet0/0
!
route-map PAT20 permit 10
 match ip address VLAN20
 match interface Ethernet0/1


```



```
R28#sh dmvpn
Legend: Attrb --> S - Static, D - Dynamic, I - Incomplete
        N - NATed, L - Local, X - No Socket
        # Ent --> Number of NHRP entries with same NBMA peer
        NHS Status: E --> Expecting Replies, R --> Responding, W --> Waiting
        UpDn Time --> Up or Down Time for a Tunnel
==========================================================================

Interface: Tunnel0, IPv4 NHRP Details
Type:Spoke, NHRP Peers:2,

 # Ent  Peer NBMA Addr Peer Tunnel Add State  UpDn Tm Attrb
 ----- --------------- --------------- ----- -------- -----
     1 77.14.2.1           99.99.99.14    UP 00:40:20     S
     1 77.15.2.1           99.99.99.15    UP 00:40:23     S


```










