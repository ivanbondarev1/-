# **Масштабируемость и дизайн iBGP**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)



## **Задачи:**
+ ### Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15.
+ ### Настроите iBGP в провайдере Триада, с использованием RR.
+ ### Настройте офиса Москва так, чтобы приоритетным провайдером стал Ламас.
+ ### Настройте офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
+ ### Все сети в лабораторной работе должны иметь IP связность.


### К работе я прикладываю файл с лабораторной

## **Решение**


## Адресное пространство:


### **Топология**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)


## Настройки:

### iBGP между маршрутизаторами R14 и R15
### Настройте офиса Москва так, чтобы приоритетным провайдером стал Ламас.

### **Москва:**


### **R14:**
```

interface Loopback0
 ip address 140.140.140.140 255.255.255.255
 ip ospf 1 area 0
!
interface Ethernet0/0
 ip address 192.168.0.18 255.255.255.240
 ip ospf 1 area 0
!
interface Ethernet0/1
 ip address 192.168.0.68 255.255.255.240
 ip ospf 1 area 0
!
interface Ethernet0/2
 ip address 77.14.2.1 255.255.255.0
!
interface Ethernet0/3
 ip address 192.168.0.81 255.255.255.240
 ip ospf network point-to-point
 ip ospf 1 area 101
!
interface Ethernet1/0
 ip address 192.168.0.178 255.255.255.240
 ip ospf 1 area 0
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
 neighbor 150.150.150.150 remote-as 1001
 neighbor 150.150.150.150 update-source Loopback0
 neighbor 150.150.150.150 next-hop-self
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
ip route 192.168.0.0 255.255.0.0 Null0



```

```
R14#sh ip route bgp
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

      10.0.0.0/26 is subnetted, 4 subnets
B        10.0.0.0 [200/0] via 150.150.150.150, 00:03:46
B        10.0.0.64 [200/0] via 150.150.150.150, 00:03:46
B        10.0.0.128 [200/0] via 150.150.150.150, 00:03:46
B        10.0.0.192 [200/0] via 150.150.150.150, 00:03:46
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.15.2.0/24 [200/0] via 150.150.150.150, 00:03:46
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [200/0] via 150.150.150.150, 00:03:46
B        78.26.3.0 [200/0] via 150.150.150.150, 00:03:30
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [200/0] via 150.150.150.150, 00:03:46
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [200/0] via 150.150.150.150, 00:03:46
B        101.22.2.0 [200/0] via 150.150.150.150, 00:03:30


```

```
R14#sh ip bgp
BGP table version is 14, local router ID is 14.14.14.14
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>i 10.0.0.0/26      150.150.150.150          0    200      0 301 520 i
 *                    77.14.2.2                              0 101 520 i
 *>i 10.0.0.64/26     150.150.150.150          0    200      0 301 520 i
 *                    77.14.2.2                              0 101 520 i
 *>i 10.0.0.128/26    150.150.150.150          0    200      0 301 520 i
 *                    77.14.2.2                              0 101 520 i
 *>i 10.0.0.192/26    150.150.150.150          0    200      0 301 520 i
 *                    77.14.2.2                              0 101 520 i
 * i 77.14.2.0/24     150.150.150.150          0    200      0 301 101 i
 *>                   0.0.0.0                  0         32768 i
 *                    77.14.2.2                0             0 101 i
 *>i 77.15.2.0/24     150.150.150.150          0    100      0 i
 *                    77.14.2.2                              0 101 301 i
 *>i 78.24.3.0/24     150.150.150.150          0    200      0 301 520 i
 *                    77.14.2.2                              0 101 520 i
 *>i 78.26.3.0/24     150.150.150.150          0    200      0 301 520 i
 *                    77.14.2.2                              0 101 520 i
 *>i 100.22.1.0/24    150.150.150.150          0    200      0 301 i
 *                    77.14.2.2                0             0 101 i
 *>i 101.21.2.0/24    150.150.150.150          0    200      0 301 i
 *                    77.14.2.2                              0 101 301 i
 *>i 101.22.2.0/24    150.150.150.150          0    200      0 301 101 i
 *                    77.14.2.2                0             0 101 i


```


### **R15**:

```
interface Loopback0
 ip address 150.150.150.150 255.255.255.255
 ip ospf 1 area 0
!
interface Ethernet0/0
 ip address 192.168.0.98 255.255.255.240
 ip ospf 1 area 0
!
interface Ethernet0/1
 ip address 192.168.0.34 255.255.255.240
 ip ospf 1 area 0
!
interface Ethernet0/2
 ip address 77.15.2.1 255.255.255.0
!
interface Ethernet0/3
 ip address 192.168.0.113 255.255.255.240
 ip ospf network point-to-point
 ip ospf 1 area 102
!
interface Ethernet1/0
 ip address 192.168.0.177 255.255.255.240
 ip ospf 1 area 0
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
 neighbor 77.15.2.2 route-map lamas in
 neighbor 140.140.140.140 remote-as 1001
 neighbor 140.140.140.140 update-source Loopback0
 neighbor 140.140.140.140 next-hop-self
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
!
!
ip prefix-list 102 seq 5 deny 192.168.0.80/28
ip prefix-list 102 seq 10 permit 0.0.0.0/0 le 32



```

```
R15#sh ip route bgp
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

      10.0.0.0/26 is subnetted, 4 subnets
B        10.0.0.0 [20/0] via 77.15.2.2, 00:07:54
B        10.0.0.64 [20/0] via 77.15.2.2, 00:07:54
B        10.0.0.128 [20/0] via 77.15.2.2, 00:07:54
B        10.0.0.192 [20/0] via 77.15.2.2, 00:07:54
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.14.2.0/24 [20/0] via 77.15.2.2, 00:07:37
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 77.15.2.2, 00:07:54
B        78.26.3.0 [20/0] via 77.15.2.2, 00:07:37
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 77.15.2.2, 00:07:54
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 77.15.2.2, 00:07:54
B        101.22.2.0 [20/0] via 77.15.2.2, 00:07:37

```

### iBGP в провайдере Триада, с использованием RR.

### **Триада**

### **R25**:

```
interface Loopback0
 ip address 25.25.25.25 255.255.255.255
 ip router isis
!
interface Ethernet0/0
 ip address 10.0.0.2 255.255.255.192
 ip router isis
!
interface Ethernet0/1
 ip address 89.25.1.1 255.255.255.0
!
interface Ethernet0/2
 ip address 10.0.0.129 255.255.255.192
 ip router isis
 isis circuit-type level-2-only
!
interface Ethernet0/3
 ip address 14.25.3.1 255.255.255.0
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
router isis
 net 49.2222.0025.0025.0025.00
!
router bgp 520
 bgp log-neighbor-changes
 network 10.0.0.0 mask 255.255.255.192
 network 10.0.0.128 mask 255.255.255.192
 neighbor 23.23.23.23 remote-as 520
 neighbor 23.23.23.23 update-source Loopback0
 neighbor 23.23.23.23 route-reflector-client
 neighbor 23.23.23.23 next-hop-self
 neighbor 24.24.24.24 remote-as 520
 neighbor 24.24.24.24 update-source Loopback0
 neighbor 24.24.24.24 route-reflector-client
 neighbor 24.24.24.24 next-hop-self
 neighbor 26.26.26.26 remote-as 520
 neighbor 26.26.26.26 update-source Loopback0
 neighbor 26.26.26.26 next-hop-self
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 14.28.2.0 255.255.255.0 14.25.3.2
ip route 14.28.3.0 255.255.255.0 14.25.3.2


```

```
R25#sh ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "application"
  Sending updates every 0 seconds
  Invalid after 0 seconds, hold down 0, flushed after 0
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Maximum path: 32
  Routing for Networks:
  Routing Information Sources:
    Gateway         Distance      Last Update
  Distance: (default is 4)

Routing Protocol is "isis"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Redistributing: isis
  Address Summarization:
    None
  Maximum path: 4
  Routing for Networks:
    Loopback0
    Ethernet0/0
    Ethernet0/2
  Routing Information Sources:
    Gateway         Distance      Last Update
    23.23.23.23          115      00:10:46
    24.24.24.24          115      00:10:46
    26.26.26.26          115      00:10:46
  Distance: (default is 115)

Routing Protocol is "bgp 520"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Route Reflector for address family IPv4 Unicast, 2 clients
  IGP synchronization is disabled
  Automatic route summarization is disabled
  Neighbor(s):
    Address          FiltIn FiltOut DistIn DistOut Weight RouteMap
    23.23.23.23
    24.24.24.24
    26.26.26.26
  Maximum path: 1
  Routing Information Sources:
    Gateway         Distance      Last Update
    26.26.26.26          200      00:09:49
    24.24.24.24          200      00:09:42
    23.23.23.23          200      00:09:38
  Distance: external 20 internal 200 local 200



```

### **R26:**

```
interface Loopback0
 ip address 26.26.26.26 255.255.255.255
 ip router isis
!
interface Ethernet0/0
 ip address 10.0.0.194 255.255.255.192
 ip router isis
 isis circuit-type level-2-only
!
interface Ethernet0/1
 ip address 14.26.1.1 255.255.255.0
!
interface Ethernet0/2
 ip address 10.0.0.130 255.255.255.192
 ip router isis
 isis circuit-type level-2-only
!
interface Ethernet0/3
 ip address 78.26.3.1 255.255.255.0
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
router isis
 net 49.0026.0026.0026.0026.00
!
router bgp 520
 bgp router-id 26.26.26.26
 bgp cluster-id 25.25.25.25
 bgp log-neighbor-changes
 network 10.0.0.128 mask 255.255.255.192
 network 10.0.0.192 mask 255.255.255.192
 network 78.26.3.0 mask 255.255.255.0
 neighbor 23.23.23.23 remote-as 520
 neighbor 23.23.23.23 update-source Loopback0
 neighbor 23.23.23.23 route-reflector-client
 neighbor 24.24.24.24 remote-as 520
 neighbor 24.24.24.24 update-source Loopback0
 neighbor 24.24.24.24 route-reflector-client
 neighbor 25.25.25.25 remote-as 520
 neighbor 25.25.25.25 update-source Loopback0
 neighbor 78.26.3.2 remote-as 2042


```

```
R26#sh ip protocols
*** IP Routing is NSF aware ***

Routing Protocol is "application"
  Sending updates every 0 seconds
  Invalid after 0 seconds, hold down 0, flushed after 0
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Maximum path: 32
  Routing for Networks:
  Routing Information Sources:
    Gateway         Distance      Last Update
  Distance: (default is 4)

Routing Protocol is "isis"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Redistributing: isis
  Address Summarization:
    None
  Maximum path: 4
  Routing for Networks:
    Loopback0
    Ethernet0/0
    Ethernet0/2
  Routing Information Sources:
    Gateway         Distance      Last Update
    23.23.23.23          115      00:12:43
    24.24.24.24          115      00:12:48
    25.25.25.25          115      00:12:43
  Distance: (default is 115)

Routing Protocol is "bgp 520"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Route Reflector for address family IPv4 Unicast with the cluster-id 25.25.25.25, 2 clients
  IGP synchronization is disabled
  Automatic route summarization is disabled
  Neighbor(s):
    Address          FiltIn FiltOut DistIn DistOut Weight RouteMap
    23.23.23.23
    24.24.24.24
    25.25.25.25
    78.26.3.2
  Maximum path: 1
  Routing Information Sources:
    Gateway         Distance      Last Update
    23.23.23.23          200      00:11:36
    24.24.24.24          200      00:11:43
    25.25.25.25          200      00:11:50
  Distance: external 20 internal 200 local 200


```

### **R24:**

```
interface Loopback0
 ip address 24.24.24.24 255.255.255.255
 ip router isis
!
interface Ethernet0/0
 ip address 101.21.2.2 255.255.255.0
!
interface Ethernet0/1
 ip address 10.0.0.193 255.255.255.192
 ip router isis
 isis circuit-type level-2-only
!
interface Ethernet0/2
 ip address 10.0.0.66 255.255.255.192
 ip router isis
 isis circuit-type level-2-only
!
interface Ethernet0/3
 ip address 78.24.3.1 255.255.255.0
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
router isis
 net 49.0024.0024.0024.0024.00
!
router bgp 520
 bgp router-id 24.24.24.24
 bgp log-neighbor-changes
 network 10.0.0.64 mask 255.255.255.192
 network 10.0.0.192 mask 255.255.255.192
 network 78.24.3.0 mask 255.255.255.0
 network 101.21.2.0 mask 255.255.255.0
 neighbor 25.25.25.25 remote-as 520
 neighbor 25.25.25.25 update-source Loopback0
 neighbor 25.25.25.25 next-hop-self
 neighbor 26.26.26.26 remote-as 520
 neighbor 26.26.26.26 update-source Loopback0
 neighbor 26.26.26.26 next-hop-self
 neighbor 78.24.3.2 remote-as 2042
 neighbor 101.21.2.1 remote-as 301

```



### **R23:**

```
interface Loopback0
 ip address 23.23.23.23 255.255.255.255
 ip router isis
!
interface Ethernet0/0
 ip address 101.22.2.2 255.255.255.0
!
interface Ethernet0/1
 ip address 10.0.0.1 255.255.255.192
 ip router isis
!
interface Ethernet0/2
 ip address 10.0.0.65 255.255.255.192
 ip router isis
 isis circuit-type level-2-only
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
router isis
 net 49.2222.0023.0023.0023.00
!
router bgp 520
 bgp router-id 23.23.23.23
 bgp log-neighbor-changes
 network 10.0.0.0 mask 255.255.255.192
 network 10.0.0.64 mask 255.255.255.192
 neighbor 25.25.25.25 remote-as 520
 neighbor 25.25.25.25 update-source Loopback0
 neighbor 25.25.25.25 next-hop-self
 neighbor 26.26.26.26 remote-as 520
 neighbor 26.26.26.26 update-source Loopback0
 neighbor 26.26.26.26 next-hop-self
 neighbor 101.22.2.1 remote-as 101


```




### Настройте офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.


### **С.-Петербург**

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
  af-interface Ethernet0/1
   summary-address 2001:CBD8:ACAD::/64
  exit-af-interface
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


```

```
R18#sh ip route  bgp
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

      10.0.0.0/26 is subnetted, 4 subnets
B        10.0.0.0 [20/0] via 78.26.3.1, 00:14:40
                  [20/0] via 78.24.3.1, 00:14:40
B        10.0.0.64 [20/0] via 78.26.3.1, 00:14:40
                   [20/0] via 78.24.3.1, 00:14:40
B        10.0.0.128 [20/0] via 78.26.3.1, 00:14:40
                    [20/0] via 78.24.3.1, 00:14:40
B        10.0.0.192 [20/0] via 78.26.3.1, 00:14:40
                    [20/0] via 78.24.3.1, 00:14:40
      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [20/0] via 78.26.3.1, 00:14:10
                   [20/0] via 78.24.3.1, 00:14:10
B        77.15.2.0 [20/0] via 78.26.3.1, 00:14:10
                   [20/0] via 78.24.3.1, 00:14:10
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 78.26.3.1, 00:14:10
                    [20/0] via 78.24.3.1, 00:14:10
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 78.26.3.1, 00:14:40
                    [20/0] via 78.24.3.1, 00:14:40
B        101.22.2.0 [20/0] via 78.26.3.1, 00:14:10
                    [20/0] via 78.24.3.1, 00:14:10


```

