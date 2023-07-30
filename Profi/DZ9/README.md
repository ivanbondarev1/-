# **BGP**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)



## **Задачи:**
+ ### Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас
+ ### Настроите eBGP между провайдерами Киторн и Ламас
+ ### Настроите eBGP между Ламас и Триада
+ ### Настроите eBGP между офисом С.-Петербург и провайдером Триада
+ ### Организуете IP доступность между пограничным роутерами офисами Москва и С.-Петербург


### К работе я прикладываю файл с лабораторной

## **Решение**


## Адресное пространство:


### **Топология**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)


## Настройки:

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
B        10.0.0.0 [200/0] via 150.150.150.150, 00:00:43
B        10.0.0.64 [200/0] via 150.150.150.150, 00:00:43
B        10.0.0.128 [200/0] via 150.150.150.150, 00:00:43
B        10.0.0.192 [200/0] via 150.150.150.150, 00:00:43
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.15.2.0/24 [200/0] via 150.150.150.150, 00:00:43
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [200/0] via 150.150.150.150, 00:00:43
B        78.26.3.0 [200/0] via 150.150.150.150, 00:00:31
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [200/0] via 150.150.150.150, 00:00:43
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [200/0] via 150.150.150.150, 00:00:43
B        101.22.2.0 [200/0] via 150.150.150.150, 00:00:31


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
!
route-map lamas permit 10
 set local-preference 200


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
B        10.0.0.0 [20/0] via 77.15.2.2, 00:06:17
B        10.0.0.64 [20/0] via 77.15.2.2, 00:06:17
B        10.0.0.128 [20/0] via 77.15.2.2, 00:06:17
B        10.0.0.192 [20/0] via 77.15.2.2, 00:06:17
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.14.2.0/24 [20/0] via 77.15.2.2, 00:06:05
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 77.15.2.2, 00:06:17
B        78.26.3.0 [20/0] via 77.15.2.2, 00:06:05
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 77.15.2.2, 00:06:17
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 77.15.2.2, 00:06:17
B        101.22.2.0 [20/0] via 77.15.2.2, 00:06:05


```

```
R15#sh ip bgp
BGP table version is 15, local router ID is 15.15.15.15
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.0.0.0/26      77.15.2.2                     200      0 301 520 i
 *>  10.0.0.64/26     77.15.2.2                     200      0 301 520 i
 *>  10.0.0.128/26    77.15.2.2                     200      0 301 520 i
 *>  10.0.0.192/26    77.15.2.2                     200      0 301 520 i
 *>  77.14.2.0/24     77.15.2.2                     200      0 301 101 i
 * i                  140.140.140.140          0    100      0 i
 *>  77.15.2.0/24     0.0.0.0                  0         32768 i
 *                    77.15.2.2                0    200      0 301 i
 *>  78.24.3.0/24     77.15.2.2                     200      0 301 520 i
 *>  78.26.3.0/24     77.15.2.2                     200      0 301 520 i
 *>  100.22.1.0/24    77.15.2.2                0    200      0 301 i
 *>  101.21.2.0/24    77.15.2.2                0    200      0 301 i
 *>  101.22.2.0/24    77.15.2.2                     200      0 301 101 i

  
```
### **Киторн**

### **R22**:

```
interface Ethernet0/0
 ip address 77.14.2.2 255.255.255.0
!
interface Ethernet0/1
 ip address 100.22.1.1 255.255.255.0
!
interface Ethernet0/2
 ip address 101.22.2.1 255.255.255.0
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
router bgp 101
 bgp router-id 22.22.22.22
 bgp log-neighbor-changes
 network 77.14.2.0 mask 255.255.255.0
 network 100.22.1.0 mask 255.255.255.0
 network 101.22.2.0 mask 255.255.255.0
 neighbor 77.14.2.1 remote-as 1001
 neighbor 100.22.1.2 remote-as 301
 neighbor 101.22.2.2 remote-as 520

```

```
R22#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/26 is subnetted, 4 subnets
B        10.0.0.0 [20/0] via 101.22.2.2, 00:08:00
B        10.0.0.64 [20/0] via 101.22.2.2, 00:08:00
B        10.0.0.128 [20/0] via 101.22.2.2, 00:08:00
B        10.0.0.192 [20/0] via 101.22.2.2, 00:08:00
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.15.2.0/24 [20/0] via 100.22.1.2, 00:08:00
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 101.22.2.2, 00:08:00
B        78.26.3.0 [20/0] via 101.22.2.2, 00:08:00
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.21.2.0/24 [20/0] via 100.22.1.2, 00:08:00

```

```
R22#sh ip bgp
BGP table version is 12, local router ID is 22.22.22.22
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   10.0.0.0/26      77.14.2.1                              0 1001 301 520 i
 *                    100.22.1.2                             0 301 520 i
 *>                   101.22.2.2               0             0 520 i
 *   10.0.0.64/26     77.14.2.1                              0 1001 301 520 i
 *                    100.22.1.2                             0 301 520 i
 *>                   101.22.2.2               0             0 520 i
 *   10.0.0.128/26    77.14.2.1                              0 1001 301 520 i
 *                    100.22.1.2                             0 301 520 i
 *>                   101.22.2.2                             0 520 i
 *   10.0.0.192/26    77.14.2.1                              0 1001 301 520 i
 *                    100.22.1.2                             0 301 520 i
 *>                   101.22.2.2                             0 520 i
 *   77.14.2.0/24     77.14.2.1                0             0 1001 i
 *>                   0.0.0.0                  0         32768 i
 *   77.15.2.0/24     101.22.2.2                             0 520 301 i
 *                    77.14.2.1                              0 1001 i
 *>                   100.22.1.2               0             0 301 i
 *   78.24.3.0/24     77.14.2.1                              0 1001 301 520 i
 *                    100.22.1.2                             0 301 520 i
 *>                   101.22.2.2                             0 520 i
 *   78.26.3.0/24     77.14.2.1                              0 1001 301 520 i
 *                    100.22.1.2                             0 301 520 i
 *>                   101.22.2.2                             0 520 i
 *   100.22.1.0/24    77.14.2.1                              0 1001 301 i
 *                    100.22.1.2               0             0 301 i
 *>                   0.0.0.0                  0         32768 i
 *   101.21.2.0/24    77.14.2.1                              0 1001 301 i
 *>                   100.22.1.2               0             0 301 i
 *                    101.22.2.2                             0 520 i
 *>  101.22.2.0/24    0.0.0.0                  0         32768 i


```

### **Ламас**

### **R21**:

```
interface Ethernet0/0
 ip address 77.15.2.2 255.255.255.0
!
interface Ethernet0/1
 ip address 100.22.1.2 255.255.255.0
!
interface Ethernet0/2
 ip address 101.21.2.1 255.255.255.0
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
router bgp 301
 bgp router-id 21.21.21.21
 bgp log-neighbor-changes
 network 77.15.2.0 mask 255.255.255.0
 network 100.22.1.0 mask 255.255.255.0
 network 101.21.2.0 mask 255.255.255.0
 neighbor 77.15.2.1 remote-as 1001
 neighbor 100.22.1.1 remote-as 101
 neighbor 101.21.2.2 remote-as 520



```

```
R21#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/26 is subnetted, 4 subnets
B        10.0.0.0 [20/0] via 101.21.2.2, 00:09:46
B        10.0.0.64 [20/0] via 101.21.2.2, 00:09:46
B        10.0.0.128 [20/0] via 101.21.2.2, 00:09:46
B        10.0.0.192 [20/0] via 101.21.2.2, 00:09:46
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.14.2.0/24 [20/0] via 100.22.1.1, 00:09:41
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 101.21.2.2, 00:09:46
B        78.26.3.0 [20/0] via 101.21.2.2, 00:09:18
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.22.2.0/24 [20/0] via 100.22.1.1, 00:09:41



```

```
R21#sh ip bgp
BGP table version is 13, local router ID is 21.21.21.21
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   10.0.0.0/26      100.22.1.1                             0 101 520 i
 *>                   101.21.2.2                             0 520 i
 *   10.0.0.64/26     100.22.1.1                             0 101 520 i
 *>                   101.21.2.2               0             0 520 i
 *   10.0.0.128/26    100.22.1.1                             0 101 520 i
 *>                   101.21.2.2                             0 520 i
 *   10.0.0.192/26    100.22.1.1                             0 101 520 i
 *>                   101.21.2.2               0             0 520 i
 *   77.14.2.0/24     101.21.2.2                             0 520 101 i
 *>                   100.22.1.1               0             0 101 i
 *   77.15.2.0/24     77.15.2.1                0             0 1001 i
 *>                   0.0.0.0                  0         32768 i
 *   78.24.3.0/24     100.22.1.1                             0 101 520 i
 *>                   101.21.2.2               0             0 520 i
 *   78.26.3.0/24     100.22.1.1                             0 101 520 i
 *>                   101.21.2.2                             0 520 i
 *   100.22.1.0/24    100.22.1.1               0             0 101 i
 *>                   0.0.0.0                  0         32768 i
 *   101.21.2.0/24    101.21.2.2               0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *   101.22.2.0/24    101.21.2.2                             0 520 101 i
 *>                   100.22.1.1               0             0 101 i


```

### **Триада**

### **R23**:

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


```
R23#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [20/0] via 101.22.2.1, 00:26:16
B        77.15.2.0 [200/0] via 24.24.24.24, 00:26:21
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [200/0] via 24.24.24.24, 00:26:23
B        78.26.3.0 [200/0] via 26.26.26.26, 00:26:23
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 101.22.2.1, 00:26:16
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.21.2.0/24 [200/0] via 24.24.24.24, 00:26:23

```
```
R23#sh ip bgp
BGP table version is 15, local router ID is 23.23.23.23
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 * i 10.0.0.0/26      25.25.25.25              0    100      0 i
 * i                  25.25.25.25              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 * i 10.0.0.64/26     24.24.24.24              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 r i 10.0.0.128/26    26.26.26.26              0    100      0 i
 r>i                  25.25.25.25              0    100      0 i
 r>i 10.0.0.192/26    26.26.26.26              0    100      0 i
 r i                  26.26.26.26              0    100      0 i
 *>  77.14.2.0/24     101.22.2.1               0             0 101 i
 *   77.15.2.0/24     101.22.2.1                             0 101 301 i
 *>i                  24.24.24.24              0    100      0 301 i
 * i                  24.24.24.24              0    100      0 301 i
 * i 78.24.3.0/24     24.24.24.24              0    100      0 i
 *>i                  24.24.24.24              0    100      0 i
 *>i 78.26.3.0/24     26.26.26.26              0    100      0 i
 * i                  26.26.26.26              0    100      0 i
 *>  100.22.1.0/24    101.22.2.1               0             0 101 i
 * i                  24.24.24.24              0    100      0 301 i
 *   101.21.2.0/24    101.22.2.1                             0 101 301 i
 * i                  24.24.24.24              0    100      0 i
 *>i                  24.24.24.24              0    100      0 i
 r>  101.22.2.0/24    101.22.2.1               0             0 101 i
R23#


```


### **R24**:


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


```
R24#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [200/0] via 23.23.23.23, 00:27:52
B        77.15.2.0 [20/0] via 101.21.2.1, 00:27:57
      78.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        78.26.3.0/24 [200/0] via 26.26.26.26, 00:27:59
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 101.21.2.1, 00:27:57
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.22.2.0/24 [200/0] via 23.23.23.23, 00:27:52


```


```
R24#sh ip bgp
BGP table version is 16, local router ID is 24.24.24.24
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r i 10.0.0.0/26      25.25.25.25              0    100      0 i
 r>i                  25.25.25.25              0    100      0 i
 * i 10.0.0.64/26     23.23.23.23              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 r>i 10.0.0.128/26    26.26.26.26              0    100      0 i
 r i                  25.25.25.25              0    100      0 i
 * i 10.0.0.192/26    26.26.26.26              0    100      0 i
 * i                  26.26.26.26              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 *   77.14.2.0/24     101.21.2.1                             0 301 101 i
 *>i                  23.23.23.23              0    100      0 101 i
 * i                  23.23.23.23              0    100      0 101 i
 *>  77.15.2.0/24     101.21.2.1               0             0 301 i
 *   78.24.3.0/24     78.24.3.2                0             0 2042 i
 *>                   0.0.0.0                  0         32768 i
 * i 78.26.3.0/24     26.26.26.26              0    100      0 i
 *>i                  26.26.26.26              0    100      0 i
 *                    78.24.3.2                0             0 2042 i
 * i 100.22.1.0/24    23.23.23.23              0    100      0 101 i
 *>                   101.21.2.1               0             0 301 i
 *   101.21.2.0/24    101.21.2.1               0             0 301 i
 *>                   0.0.0.0                  0         32768 i
 *   101.22.2.0/24    101.21.2.1                             0 301 101 i
 *>i                  23.23.23.23              0    100      0 101 i
 * i                  23.23.23.23              0    100      0 101 i

```


### **R26**:

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
!
ip forward-protocol nd
!
!
no ip http server


```

```
R26#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [200/0] via 23.23.23.23, 00:31:24
B        77.15.2.0 [200/0] via 24.24.24.24, 00:31:29
      78.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        78.24.3.0/24 [200/0] via 24.24.24.24, 00:31:31
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [200/0] via 24.24.24.24, 00:31:29
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [200/0] via 24.24.24.24, 00:31:31
B        101.22.2.0 [200/0] via 23.23.23.23, 00:31:24


```


```
R26#sh ip bgp
BGP table version is 12, local router ID is 26.26.26.26
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r i 10.0.0.0/26      23.23.23.23              0    100      0 i
 r>i                  25.25.25.25              0    100      0 i
 r i 10.0.0.64/26     23.23.23.23              0    100      0 i
 r>i                  24.24.24.24              0    100      0 i
 * i 10.0.0.128/26    25.25.25.25              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 * i 10.0.0.192/26    24.24.24.24              0    100      0 i
 *>                   0.0.0.0                  0         32768 i
 *>i 77.14.2.0/24     23.23.23.23              0    100      0 101 i
 *>i 77.15.2.0/24     24.24.24.24              0    100      0 301 i
 *>i 78.24.3.0/24     24.24.24.24              0    100      0 i
 *                    78.26.3.2                0             0 2042 i
 *   78.26.3.0/24     78.26.3.2                0             0 2042 i
 *>                   0.0.0.0                  0         32768 i
 * i 100.22.1.0/24    23.23.23.23              0    100      0 101 i
 *>i                  24.24.24.24              0    100      0 301 i
 *>i 101.21.2.0/24    24.24.24.24              0    100      0 i
 *>i 101.22.2.0/24    23.23.23.23              0    100      0 101 i

```



### **Санкт-Петербург**

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
R18#sh ip route bgp
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
B        10.0.0.0 [20/0] via 78.26.3.1, 00:33:06
                  [20/0] via 78.24.3.1, 00:33:06
B        10.0.0.64 [20/0] via 78.26.3.1, 00:33:06
                   [20/0] via 78.24.3.1, 00:33:06
B        10.0.0.128 [20/0] via 78.26.3.1, 00:33:06
                    [20/0] via 78.24.3.1, 00:33:06
B        10.0.0.192 [20/0] via 78.26.3.1, 00:33:06
                    [20/0] via 78.24.3.1, 00:33:06
      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [20/0] via 78.26.3.1, 00:32:35
                   [20/0] via 78.24.3.1, 00:32:35
B        77.15.2.0 [20/0] via 78.26.3.1, 00:32:35
                   [20/0] via 78.24.3.1, 00:32:35
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 78.26.3.1, 00:32:35
                    [20/0] via 78.24.3.1, 00:32:35
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 78.26.3.1, 00:33:06
                    [20/0] via 78.24.3.1, 00:33:06
B        101.22.2.0 [20/0] via 78.26.3.1, 00:32:35
                    [20/0] via 78.24.3.1, 00:32:35



```

```
R18#sh ip bg
R18#sh ip bgp
BGP table version is 21, local router ID is 18.18.18.18
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *m  10.0.0.0/26      78.26.3.1                              0 520 i
 *>                   78.24.3.1                              0 520 i
 *m  10.0.0.64/26     78.26.3.1                              0 520 i
 *>                   78.24.3.1                0             0 520 i
 *m  10.0.0.128/26    78.26.3.1                0             0 520 i
 *>                   78.24.3.1                              0 520 i
 *m  10.0.0.192/26    78.26.3.1                0             0 520 i
 *>                   78.24.3.1                0             0 520 i
 *m  77.14.2.0/24     78.26.3.1                              0 520 101 i
 *>                   78.24.3.1                              0 520 101 i
 *m  77.15.2.0/24     78.26.3.1                              0 520 301 i
 *>                   78.24.3.1                              0 520 301 i
 *   78.24.3.0/24     78.26.3.1                              0 520 i
 *                    78.24.3.1                0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *   78.26.3.0/24     78.24.3.1                              0 520 i
 *                    78.26.3.1                0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *m  100.22.1.0/24    78.26.3.1                              0 520 301 i
 *>                   78.24.3.1                              0 520 301 i
 *m  101.21.2.0/24    78.26.3.1                              0 520 i
 *>                   78.24.3.1                0             0 520 i
 *m  101.22.2.0/24    78.26.3.1                              0 520 101 i
 *>                   78.24.3.1                              0 520 101 i

```
