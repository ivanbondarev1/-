# **BGP. Фильтрация**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/raw/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)



## **Задачи:**
+ ### Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).
+ ### Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
+ ### Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
+ ### Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.




## **Решение**

### В С.-Петербурге только один eBGP маршрутизатор, а значит транзитного трафика там нет 

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
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
!
interface Ethernet0/1
 ip address 192.168.0.68 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
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

Gateway of last resort is 77.14.2.2 to network 0.0.0.0

B*    0.0.0.0/0 [20/0] via 77.14.2.2, 00:08:00
      50.0.0.0/24 is subnetted, 1 subnets
B        50.50.50.0 [200/0] via 150.150.150.150, 00:08:00
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.15.2.0/24 [200/0] via 150.150.150.150, 00:08:00


```

```
R14#sh ip bgp neighbors 77.14.2.2 advertised-routes
BGP table version is 5, local router ID is 14.14.14.14
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  77.14.2.0/24     0.0.0.0                  0         32768 i
 *>i 77.15.2.0/24     150.150.150.150          0    100      0 i

Total number of prefixes 2


```



### **R15**:

```
interface Loopback0
 ip address 150.150.150.150 255.255.255.255
 ip ospf 1 area 0
!
interface Ethernet0/0
 ip address 192.168.0.98 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
!
interface Ethernet0/1
 ip address 192.168.0.34 255.255.255.240
 ip nat inside
 ip virtual-reassembly in
 ip ospf 1 area 0
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

Gateway of last resort is 77.15.2.2 to network 0.0.0.0

B*    0.0.0.0/0 [20/0] via 77.15.2.2, 00:09:54
      50.0.0.0/24 is subnetted, 1 subnets
B        50.50.50.0 [20/0] via 77.15.2.2, 00:09:54
      77.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        77.14.2.0/24 [200/0] via 140.140.140.140, 00:09:53

```

```

R15#sh ip bgp neighbors 77.15.2.2 advertised-routes
BGP table version is 5, local router ID is 15.15.15.15
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>i 77.14.2.0/24     140.140.140.140          0    100      0 i
 *>  77.15.2.0/24     0.0.0.0                  0         32768 i

Total number of prefixes 2

```


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
 neighbor 77.14.2.1 default-originate
 neighbor 77.14.2.1 prefix-list MSK out
 neighbor 100.22.1.2 remote-as 301
 neighbor 101.22.2.2 remote-as 520
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
!
!
ip prefix-list MSK seq 5 deny 0.0.0.0/0 le 32


```







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
 neighbor 77.15.2.1 default-originate
 neighbor 77.15.2.1 prefix-list SPB out
 neighbor 100.22.1.1 remote-as 101
 neighbor 101.21.2.2 remote-as 520
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
!
!
ip prefix-list SPB seq 5 permit 50.50.50.0/24
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
 neighbor 78.24.3.1 remote-as 520
 neighbor 78.24.3.1 route-map transit out
 neighbor 78.26.3.1 remote-as 520
 neighbor 78.26.3.1 route-map transit out
 maximum-paths 2
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
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
ipv6 route ::/0 Loopback1
!
route-map transit permit 5
 match as-path 1

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
B        10.0.0.0 [20/0] via 78.26.3.1, 00:14:11
                  [20/0] via 78.24.3.1, 00:14:11
B        10.0.0.64 [20/0] via 78.26.3.1, 00:14:11
                   [20/0] via 78.24.3.1, 00:14:11
B        10.0.0.128 [20/0] via 78.26.3.1, 00:14:11
                    [20/0] via 78.24.3.1, 00:14:11
B        10.0.0.192 [20/0] via 78.26.3.1, 00:14:11
                    [20/0] via 78.24.3.1, 00:14:11
      77.0.0.0/24 is subnetted, 2 subnets
B        77.14.2.0 [20/0] via 78.26.3.1, 00:14:11
                   [20/0] via 78.24.3.1, 00:14:11
B        77.15.2.0 [20/0] via 78.26.3.1, 00:14:11
                   [20/0] via 78.24.3.1, 00:14:11
      100.0.0.0/24 is subnetted, 1 subnets
B        100.22.1.0 [20/0] via 78.26.3.1, 00:14:11
                    [20/0] via 78.24.3.1, 00:14:11
      101.0.0.0/24 is subnetted, 2 subnets
B        101.21.2.0 [20/0] via 78.26.3.1, 00:14:11
                    [20/0] via 78.24.3.1, 00:14:11
B        101.22.2.0 [20/0] via 78.26.3.1, 00:14:11
                    [20/0] via 78.24.3.1, 00:14:11

```

```
R18#sh ip bgp neighbors 78.24.3.1 ad
R18#sh ip bgp neighbors 78.24.3.1 advertised-routes
BGP table version is 13, local router ID is 18.18.18.18
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  50.50.50.0/24    0.0.0.0                  0         32768 i
 *>  78.24.3.0/24     0.0.0.0                  0         32768 i
 *>  78.26.3.0/24     0.0.0.0                  0         32768 i

Total number of prefixes 3


```

```
R18#sh ip bgp neighbors 78.26.3.1 advertised-routes
BGP table version is 13, local router ID is 18.18.18.18
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  50.50.50.0/24    0.0.0.0                  0         32768 i
 *>  78.24.3.0/24     0.0.0.0                  0         32768 i
 *>  78.26.3.0/24     0.0.0.0                  0         32768 i

Total number of prefixes 3


```












