# **EIGRP**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ8/EVE%20_%20Topology%20-%20Google%20Chrome%2031.07.2023%2018_40_24.png?raw=true)



## **Задачи:**
+ ### В офисе С.-Петербург настроить EIGRP.
+ ### R32 получает только маршрут по умолчанию.
+ ### R16-17 анонсируют только суммарные префиксы.
+ ### Использовать EIGRP named-mode для настройки сети. Настройка осуществляется одновременно для IPv4 и IPv6.



## **Решение**


## Настройки:

### **С.-Петербург:**



### **R17:**
```
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.10
 encapsulation dot1Q 10
 ip address 172.16.1.65 255.255.255.192
 standby version 2
 standby 10 ip 172.16.1.66
 standby 10 priority 200
 standby 10 preempt
!
interface Ethernet0/0.20
 encapsulation dot1Q 20
 ip address 172.16.1.129 255.255.255.192
 standby version 2
 standby 20 ip 172.16.1.130
 standby 20 preempt
!
interface Ethernet0/0.40
 encapsulation dot1Q 40
 ip address 172.16.1.193 255.255.255.192
 standby version 2
 standby 40 ip 172.16.1.194
 standby 40 preempt
!
interface Ethernet0/1
 ip address 172.16.0.66 255.255.255.192
 ipv6 address FE80::1 link-local
 ipv6 address 2001:CBD8:ACAD:4::1/64
!
interface Ethernet0/2
 ip address 172.16.0.193 255.255.255.192
 ipv6 address FE80::2 link-local
 ipv6 address 2001:CBD8:ACAD:3::1/64
!
interface Ethernet0/3
 no ip address
 shutdown
!
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 78
  !
  af-interface Ethernet0/2
   summary-address 172.16.0.0 255.255.0.0
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 172.16.0.0 255.255.0.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 172.16.0.64 0.0.0.63
  network 172.16.0.192 0.0.0.63
  network 172.16.1.64 0.0.0.63
  network 172.16.1.128 0.0.0.63
  network 172.16.1.192 0.0.0.63
  eigrp router-id 17.17.17.17
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 78
  !
  af-interface Ethernet0/2
   summary-address 2001:CBD8:ACAD::/48
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 2001:CBD8:ACAD::/48
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 17.17.17.17
 exit-address-family


```

```
R17#sh ipv6 route eigrp
IPv6 Routing Table - default - 10 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
EX  ::/0 [170/1024640]
     via FE80::3, Ethernet0/1
D   2001::180/128 [90/1024640]
     via FE80::3, Ethernet0/1
D   2001:CBD8:ACAD::/48 [90/2048000]
     via FE80::3, Ethernet0/1
D   2001:CBD8:ACAD:1::/64 [90/1536000]
     via FE80::3, Ethernet0/1
D   2001:CBD8:ACAD:2::/64 [90/1536000]
     via FE80::5, Ethernet0/2

```

```
R17#sh ip route  eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.65 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/1024640] via 172.16.0.65, 00:42:25, Ethernet0/1
      172.16.0.0/16 is variably subnetted, 13 subnets, 3 masks
D        172.16.0.0/16 is a summary, 00:42:28, Null0
D        172.16.0.0/26 [90/1536000] via 172.16.0.65, 00:42:30, Ethernet0/1
D        172.16.1.0/26 [90/1536000] via 172.16.0.194, 00:42:28, Ethernet0/2
      180.180.0.0/32 is subnetted, 1 subnets
D        180.180.180.180 [90/1024640] via 172.16.0.65, 00:42:25, Ethernet0/1


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
interface Ethernet0/0
 ip address 172.16.0.1 255.255.255.192
 ipv6 address FE80::4 link-local
 ipv6 address 2001:CBD8:ACAD:1::2/64
!
interface Ethernet0/1
 ip address 172.16.0.65 255.255.255.192
 ipv6 address FE80::3 link-local
 ipv6 address 2001:CBD8:ACAD:4::2/64
!
interface Ethernet0/2
 ip address 78.24.3.2 255.255.255.0
!
interface Ethernet0/3
 ip address 78.26.3.2 255.255.255.0
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
 network 78.24.3.0 mask 255.255.255.0
 network 78.26.3.0 mask 255.255.255.0
 neighbor 78.24.3.1 remote-as 520
 neighbor 78.26.3.1 remote-as 520
 maximum-paths 2
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Loopback0
!
ipv6 route ::/0 Loopback1


```

```
R18#sh ipv6 route eigrp
IPv6 Routing Table - default - 9 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
D   2001:CBD8:ACAD::/48 [90/1536000]
     via FE80::6, Ethernet0/0
D   2001:CBD8:ACAD:2::/64 [90/1536000]
     via FE80::6, Ethernet0/0

```

```
R18#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 0.0.0.0 to network 0.0.0.0

      172.16.0.0/16 is variably subnetted, 6 subnets, 3 masks
D        172.16.0.0/16 [90/1536000] via 172.16.0.66, 00:42:59, Ethernet0/1
                       [90/1536000] via 172.16.0.2, 00:42:59, Ethernet0/0
D        172.16.1.0/26 [90/1536000] via 172.16.0.2, 00:42:59, Ethernet0/0


```

### **R16**:

```
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.10
 encapsulation dot1Q 10
 ip address 172.16.1.67 255.255.255.192
 standby version 2
 standby 10 ip 172.16.1.66
 standby 10 preempt
!
interface Ethernet0/0.20
 encapsulation dot1Q 20
 ip address 172.16.1.131 255.255.255.192
 standby version 2
 standby 20 ip 172.16.1.130
 standby 20 priority 200
 standby 20 preempt
!
interface Ethernet0/0.40
 encapsulation dot1Q 40
 ip address 172.16.1.195 255.255.255.192
 standby version 2
 standby 40 ip 172.16.1.194
 standby 40 preempt
!
interface Ethernet0/1
 ip address 172.16.0.2 255.255.255.192
 ipv6 address FE80::6 link-local
 ipv6 address 2001:CBD8:ACAD:1::1/64
!
interface Ethernet0/2
 ip address 172.16.0.194 255.255.255.192
 ipv6 address FE80::5 link-local
 ipv6 address 2001:CBD8:ACAD:3::2/64
!
interface Ethernet0/3
 ip address 172.16.1.1 255.255.255.192
 ipv6 address FE80::7 link-local
 ipv6 address 2001:CBD8:ACAD:2::1/64
!
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 78
  !
  af-interface Ethernet0/2
   summary-address 172.16.0.0 255.255.0.0 leak-map MAP
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 172.16.0.0 255.255.0.0 leak-map MAP
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address 172.16.0.0 255.255.0.0
  exit-af-interface
  !
  topology base
   distribute-list SPB out Ethernet0/3
  exit-af-topology
  network 172.16.0.0 0.0.0.63
  network 172.16.0.192 0.0.0.63
  network 172.16.1.0 0.0.0.63
  network 172.16.1.64 0.0.0.63
  network 172.16.1.128 0.0.0.63
  eigrp router-id 16.16.16.16
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 78
  !
  af-interface Ethernet0/2
   summary-address 2001:CBD8:ACAD::/48 leak-map MAPv6
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address 2001:CBD8:ACAD::/48
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 2001:CBD8:ACAD::/48 leak-map MAPv6
  exit-af-interface
  !
  topology base
   distribute-list prefix-list 1 out Ethernet0/3
  exit-af-topology
  eigrp router-id 16.16.16.16
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
!
ip access-list standard R32
 permit 172.16.1.0 0.0.0.63
ip access-list standard SPB
 permit 0.0.0.0
!
!
!
ipv6 prefix-list 1 seq 5 permit ::/0
!
ipv6 prefix-list 2 seq 5 permit 2001:CBD8:ACAD:2::/64
route-map MAPv6 permit 10
 match ipv6 address prefix-list 2
!
route-map MAP permit 10
 match ip address R32


```


```
R16#sh ipv6 route eigrp
IPv6 Routing Table - default - 11 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
EX  ::/0 [170/1024640]
     via FE80::4, Ethernet0/1
D   2001::180/128 [90/1024640]
     via FE80::4, Ethernet0/1
D   2001:CBD8:ACAD::/48 [5/1024000]
     via Null0, directly connected
D   2001:CBD8:ACAD:4::/64 [90/1536000]
     via FE80::4, Ethernet0/1

```

```
R16#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.1 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/1024640] via 172.16.0.1, 00:44:02, Ethernet0/1
      172.16.0.0/16 is variably subnetted, 14 subnets, 3 masks
D        172.16.0.0/16 is a summary, 00:44:02, Null0
D        172.16.0.64/26 [90/1536000] via 172.16.0.1, 00:44:02, Ethernet0/1
      180.180.0.0/32 is subnetted, 1 subnets
D        180.180.180.180 [90/1024640] via 172.16.0.1, 00:44:02, Ethernet0/1

```




### **R32**:

```
interface Ethernet0/0
 ip address 172.16.1.2 255.255.255.192
 ipv6 address FE80::8 link-local
 ipv6 address 2001:CBD8:ACAD:2::2/64
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 78
  !
  topology base
  exit-af-topology
  network 172.16.1.0 0.0.0.63
  eigrp router-id 32.32.32.32
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 78
  !
  topology base
  exit-af-topology
  eigrp router-id 32.32.32.32
 exit-address-family


```

```
R32#sh ipv6 route eigrp
IPv6 Routing Table - default - 4 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
EX  ::/0 [170/1536640]
     via FE80::7, Ethernet0/0

```

```
R32#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.1.1 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/1536640] via 172.16.1.1, 00:45:43, Ethernet0/0


```


