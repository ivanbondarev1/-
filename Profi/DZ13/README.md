# **Основные протоколы сети интернет**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/raw/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)



## **Задачи:**
+ ### Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
+ ### Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
+ ### Настроите статический NAT для R20.
+ ### Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
+ ### Настроите статический NAT(PAT) для офиса Чокурдах.
+ ### Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
+ ### Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.




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
R14#sh run | sec ntp
 ntp broadcast client
 ntp broadcast client
 ntp broadcast client
ntp update-calendar
ntp server 12.12.12.12
ntp server 13.13.13.13

```
```
R14#sh ntp associations

  address         ref clock       st   when   poll reach  delay  offset   disp
* 192.168.0.65    127.127.1.1      5      0     64   377  0.000  -1.000  2.901
+ 192.168.0.17    127.127.1.1      5     29     64   376  0.000   0.000  2.903
+~12.12.12.12     127.127.1.1      5     47     64   377  0.000   0.000  3.033
+~13.13.13.13     127.127.1.1      5     31     64   377  0.000   0.000  4.170
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

```

### **R28 ping R19**:

```
R14#sh ip nat tra
Pro Inside global      Inside local       Outside local      Outside global
icmp 77.14.2.199:956   192.168.0.82:956   14.25.3.2:956      14.25.3.2:956
--- 77.14.2.199        192.168.0.82       ---                ---
R14#

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




### **R28 ping R20**:

```
R15#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 77.15.2.20:895    192.168.0.114:895  14.25.3.2:895      14.25.3.2:895
--- 77.15.2.20         192.168.0.114      ---                ---
R15#

```

### **SW4**:

```
ip dhcp excluded-address 192.168.0.130
ip dhcp excluded-address 192.168.0.129
ip dhcp excluded-address 192.168.0.146
ip dhcp excluded-address 192.168.0.145
!
ip dhcp pool vlan10
 network 192.168.0.128 255.255.255.240
 default-router 192.168.0.129
!
!
no ip domain-lookup
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 10 priority 0
spanning-tree vlan 20 priority 4096
!
vlan internal allocation policy ascending
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
interface Loopback0
 ip address 4.4.4.4 255.255.255.255
 ip ospf 1 area 0
!
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet1/0
 no switchport
 ip address 192.168.0.2 255.255.255.240
 ip ospf 1 area 10
 duplex auto
 ntp broadcast client
!
interface Ethernet1/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan10
 ip address 192.168.0.130 255.255.255.240
 standby version 2
 standby 10 ip 192.168.0.129
 standby 10 priority 200
 standby 10 preempt
 ip ospf 1 area 10
!
interface Vlan20
 ip address 192.168.0.146 255.255.255.240
 standby version 2
 standby 20 ip 192.168.0.145
 standby 20 preempt
 ip ospf 1 area 10
!
interface Vlan40
 ip address 192.168.1.1 255.255.255.0
 ip ospf 1 area 10
!
router ospf 1
 router-id 4.4.4.4
 area 10 stub
 passive-interface Vlan10
 passive-interface Vlan20
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
ntp master 3
ntp update-calendar
ntp server 12.12.12.12


```

```
SW4#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address      Client-ID/              Lease expiration        Type       State      Interface
                Hardware address/
                User name
192.168.0.132   0100.5079.6668.01       Sep 18 2023 06:46 AM    Automatic  Active     Vlan10


```

### **SW5**:

```
ip dhcp excluded-address 192.168.0.130
ip dhcp excluded-address 192.168.0.129
ip dhcp excluded-address 192.168.0.146
ip dhcp excluded-address 192.168.0.145
ip dhcp excluded-address 192.168.0.131
ip dhcp excluded-address 192.168.0.147
!
ip dhcp pool vlan20
 network 192.168.0.144 255.255.255.240
 default-router 192.168.0.145
!
!
no ip domain-lookup
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 10 priority 4096
spanning-tree vlan 20 priority 0
!
vlan internal allocation policy ascending
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
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet1/0
 no switchport
 ip address 192.168.0.161 255.255.255.240
 ip ospf 1 area 10
 duplex auto
 ntp broadcast client
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan10
 ip address 192.168.0.131 255.255.255.240
 standby version 2
 standby 10 ip 192.168.0.129
 standby 10 preempt
 ip ospf 1 area 10
!
interface Vlan20
 ip address 192.168.0.147 255.255.255.240
 standby version 2
 standby 20 ip 192.168.0.145
 standby 20 priority 200
 standby 20 preempt
 ip ospf 1 area 10
!
interface Vlan40
 ip address 192.168.1.2 255.255.255.0
 ip ospf 1 area 10
!
router ospf 1
 router-id 5.5.5.5
 area 10 stub
 passive-interface Vlan10
 passive-interface Vlan20
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
ntp update-calendar
ntp server 12.12.12.12
ntp server 13.13.13.13

```



### **SW5**:


```
SW5#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address      Client-ID/              Lease expiration        Type       State      Interface
                Hardware address/
                User name
192.168.0.148   0100.5079.6668.07       Sep 18 2023 06:46 AM    Automatic  Active     Vlan20


```

### **Питер:**

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


```
R18#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 50.50.50.6:0      172.16.0.66:0      101.21.2.1:0       101.21.2.1:0
R18#


```




### **Чокурдах:**

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
R28#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 14.26.1.2:55444   14.28.2.15:55444   101.21.2.1:55444   101.21.2.1:55444
icmp 14.26.1.2:55700   14.28.2.15:55700   101.21.2.1:55700   101.21.2.1:55700
icmp 14.26.1.2:55956   14.28.2.15:55956   101.21.2.1:55956   101.21.2.1:55956
icmp 14.26.1.2:56212   14.28.2.15:56212   101.21.2.1:56212   101.21.2.1:56212
icmp 14.26.1.2:56468   14.28.2.15:56468   101.21.2.1:56468   101.21.2.1:56468
icmp 14.25.3.2:49812   14.28.3.15:49812   101.21.1.1:49812   101.21.1.1:49812
icmp 14.25.3.2:50068   14.28.3.15:50068   101.21.1.1:50068   101.21.1.1:50068
icmp 14.25.3.2:50324   14.28.3.15:50324   101.21.1.1:50324   101.21.1.1:50324
icmp 14.25.3.2:50580   14.28.3.15:50580   101.21.1.1:50580   101.21.1.1:50580
icmp 14.25.3.2:50836   14.28.3.15:50836   101.21.1.1:50836   101.21.1.1:50836
icmp 14.25.3.2:52628   14.28.3.15:52628   101.21.2.1:52628   101.21.2.1:52628
icmp 14.25.3.2:52884   14.28.3.15:52884   101.21.2.1:52884   101.21.2.1:52884
icmp 14.25.3.2:53140   14.28.3.15:53140   101.21.2.1:53140   101.21.2.1:53140
icmp 14.25.3.2:53396   14.28.3.15:53396   101.21.2.1:53396   101.21.2.1:53396
icmp 14.25.3.2:53652   14.28.3.15:53652   101.21.2.1:53652   101.21.2.1:53652
R28#

```










