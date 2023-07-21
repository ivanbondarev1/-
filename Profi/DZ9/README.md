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
 network 140.140.140.140 mask 255.255.255.255
 network 192.168.0.0 mask 255.255.0.0
 neighbor 77.14.2.2 remote-as 101
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Loopback0

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

      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.15.2.0/24 [20/0] via 77.14.2.2, 00:27:26
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 77.14.2.2, 00:27:26
B        78.26.3.0 [20/0] via 77.14.2.2, 00:26:56
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 77.14.2.2, 00:27:26
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 77.14.2.2, 00:27:26
B        101.22.2.0 [20/0] via 77.14.2.2, 00:27:26

```

```
R14#sh ip bgp
BGP table version is 10, local router ID is 14.14.14.14
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     0.0.0.0                  0         32768 i
 *                    77.14.2.2                0             0 101 i
 *>  77.15.2.0/24     77.14.2.2                              0 101 301 i
 *>  78.24.3.0/24     77.14.2.2                              0 101 301 520 i
 *>  78.26.3.0/24     77.14.2.2                              0 101 301 520 2042 i
 *>  100.22.1.0/24    77.14.2.2                0             0 101 i
 *>  101.21.2.0/24    77.14.2.2                              0 101 301 i
 *>  101.22.2.0/24    77.14.2.2                0             0 101 i
 *>  140.140.140.140/32
                       0.0.0.0                  0         32768 i
      L1   Et0/1       10.0.0.2        UP    7        R25.01 

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

Gateway of last resort is 192.168.0.178 to network 0.0.0.0

      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.14.2.0/24 [20/0] via 77.15.2.2, 00:31:41
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 77.15.2.2, 00:32:12
B        78.26.3.0 [20/0] via 77.15.2.2, 00:31:41
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 77.15.2.2, 00:32:12
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 77.15.2.2, 00:32:12
B        101.22.2.0 [20/0] via 77.15.2.2, 00:31:41

```

```
R15#sh ip bgp
BGP table version is 9, local router ID is 15.15.15.15
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     77.15.2.2                              0 301 101 i
 *>  77.15.2.0/24     0.0.0.0                  0         32768 i
 *                    77.15.2.2                0             0 301 i
 *>  78.24.3.0/24     77.15.2.2                              0 301 520 i
 *>  78.26.3.0/24     77.15.2.2                              0 301 520 2042 i
 *>  100.22.1.0/24    77.15.2.2                0             0 301 i
 *>  101.21.2.0/24    77.15.2.2                0             0 301 i
 *>  101.22.2.0/24    77.15.2.2                              0 301 101 i
  

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

      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.15.2.0/24 [20/0] via 100.22.1.2, 00:55:46
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 100.22.1.2, 00:55:46
B        78.26.3.0 [20/0] via 100.22.1.2, 00:55:17
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.21.2.0/24 [20/0] via 100.22.1.2, 00:55:46
      140.140.0.0/32 is subnetted, 1 subnets
B        140.140.140.140 [20/0] via 77.14.2.1, 00:55:45

```

```
R22#sh ip bgp
BGP table version is 9, local router ID is 22.22.22.22
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   77.14.2.0/24     77.14.2.1                0             0 1001 i
 *>                   0.0.0.0                  0         32768 i
 *>  77.15.2.0/24     100.22.1.2               0             0 301 i
 *>  78.24.3.0/24     100.22.1.2                             0 301 520 i
 *>  78.26.3.0/24     100.22.1.2                             0 301 520 2042 i
 *   100.22.1.0/24    100.22.1.2               0             0 301 i
 *>                   0.0.0.0                  0         32768 i
 *>  101.21.2.0/24    100.22.1.2               0             0 301 i
 *>  101.22.2.0/24    0.0.0.0                  0         32768 i
 *>  140.140.140.140/32
                       77.14.2.1                0             0 1001 i

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

      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.14.2.0/24 [20/0] via 100.22.1.1, 00:58:01
      78.0.0.0/24 is subnetted, 2 subnets
B        78.24.3.0 [20/0] via 101.21.2.2, 00:58:03
B        78.26.3.0 [20/0] via 101.21.2.2, 00:57:44
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.22.2.0/24 [20/0] via 100.22.1.1, 00:58:01
      140.140.0.0/32 is subnetted, 1 subnets
B        140.140.140.140 [20/0] via 100.22.1.1, 00:57:30


```

```
R21#sh ip bgp
BGP table version is 9, local router ID is 21.21.21.21
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     100.22.1.1               0             0 101 i
 *   77.15.2.0/24     77.15.2.1                0             0 1001 i
 *>                   0.0.0.0                  0         32768 i
 *>  78.24.3.0/24     101.21.2.2               0             0 520 i
 *>  78.26.3.0/24     101.21.2.2                             0 520 2042 i
 *   100.22.1.0/24    100.22.1.1               0             0 101 i
 *>                   0.0.0.0                  0         32768 i
 *   101.21.2.0/24    101.21.2.2               0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *>  101.22.2.0/24    100.22.1.1               0             0 101 i
 *>  140.140.140.140/32
                       100.22.1.1                             0 101 1001 i

```

### **Триада**

### **R23**:

```
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
B        77.14.2.0 [20/0] via 101.22.2.1, 01:00:28
B        77.15.2.0 [20/0] via 101.22.2.1, 01:00:28
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 101.22.2.1, 01:00:28
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.21.2.0/24 [20/0] via 101.22.2.1, 01:00:28
      140.140.0.0/32 is subnetted, 1 subnets
B        140.140.140.140 [20/0] via 101.22.2.1, 00:59:57
R23#

```
```
R23#sh ip bgp
BGP table version is 7, local router ID is 23.23.23.23
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     101.22.2.1               0             0 101 i
 *>  77.15.2.0/24     101.22.2.1                             0 101 301 i
 *>  100.22.1.0/24    101.22.2.1               0             0 101 i
 *>  101.21.2.0/24    101.22.2.1                             0 101 301 i
 r>  101.22.2.0/24    101.22.2.1               0             0 101 i
 *>  140.140.140.140/32
                       101.22.2.1                             0 101 1001 i
R23#

```


### **R24**:


```
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
 network 78.24.3.0 mask 255.255.255.0
 network 101.21.2.0 mask 255.255.255.0
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
B        77.14.2.0 [20/0] via 101.21.2.1, 01:02:39
B        77.15.2.0 [20/0] via 101.21.2.1, 01:03:10
      78.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        78.26.3.0/24 [20/0] via 78.24.3.2, 01:02:51
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 101.21.2.1, 01:03:10
      101.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        101.22.2.0/24 [20/0] via 101.21.2.1, 01:02:39
      140.140.0.0/32 is subnetted, 1 subnets
B        140.140.140.140 [20/0] via 101.21.2.1, 01:02:08

```


```
R24#sh ip bgp
BGP table version is 9, local router ID is 24.24.24.24
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     101.21.2.1                             0 301 101 i
 *>  77.15.2.0/24     101.21.2.1               0             0 301 i
 *   78.24.3.0/24     78.24.3.2                0             0 2042 i
 *>                   0.0.0.0                  0         32768 i
 *>  78.26.3.0/24     78.24.3.2                0             0 2042 i
 *>  100.22.1.0/24    101.21.2.1               0             0 301 i
 *   101.21.2.0/24    101.21.2.1               0             0 301 i
 *>                   0.0.0.0                  0         32768 i
 *>  101.22.2.0/24    101.21.2.1                             0 301 101 i
 *>  140.140.140.140/32
                       101.21.2.1                             0 301 101 1001 i

```


### **R26**:

```
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
 bgp log-neighbor-changes
 network 78.26.3.0 mask 255.255.255.0
 neighbor 78.26.3.2 remote-as 2042

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

      78.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        78.24.3.0/24 [20/0] via 78.26.3.2, 01:05:20
R26#

```


```
R26#sh ip bgp
BGP table version is 3, local router ID is 26.26.26.26
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  78.24.3.0/24     78.26.3.2                0             0 2042 i
 *   78.26.3.0/24     78.26.3.2                0             0 2042 i
 *>                   0.0.0.0                  0         32768 i

```



### **Санкт-Петербург**

### **R18**:

```
interface Ethernet0/0
 ip address 172.16.0.1 255.255.255.192
!
interface Ethernet0/1
 ip address 172.16.0.65 255.255.255.192
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
router bgp 2042
 bgp router-id 18.18.18.18
 bgp log-neighbor-changes
 network 78.24.3.0 mask 255.255.255.0
 network 78.26.3.0 mask 255.255.255.0
 neighbor 78.24.3.1 remote-as 520
 neighbor 78.26.3.1 remote-as 520

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

      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [20/0] via 78.24.3.1, 01:07:31
B        77.15.2.0 [20/0] via 78.24.3.1, 01:08:01
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 78.24.3.1, 01:08:01
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 78.24.3.1, 01:08:32
B        101.22.2.0 [20/0] via 78.24.3.1, 01:07:31
      140.140.0.0/32 is subnetted, 1 subnets
B        140.140.140.140 [20/0] via 78.24.3.1, 01:07:00


```

```
R18#sh ip bgp
BGP table version is 11, local router ID is 18.18.18.18
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     78.24.3.1                              0 520 301 101 i
 *>  77.15.2.0/24     78.24.3.1                              0 520 301 i
 *>  78.24.3.0/24     0.0.0.0                  0         32768 i
 *                    78.24.3.1                0             0 520 i
 *>  78.26.3.0/24     0.0.0.0                  0         32768 i
 *                    78.26.3.1                0             0 520 i
 *>  100.22.1.0/24    78.24.3.1                              0 520 301 i
 *>  101.21.2.0/24    78.24.3.1                0             0 520 i
 *>  101.22.2.0/24    78.24.3.1                              0 520 301 101 i
 *>  140.140.140.140/32
                       78.24.3.1                              0 520 301 101 1001 i

```
