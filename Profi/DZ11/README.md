# **BGP. Фильтрация**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/raw/main/Profi/DZ9/EVE%20_%20Topology%20-%20Google%20Chrome%2021.07.2023%2011_42_48.png?raw=true)



## **Задачи:**
+ ### Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).
+ ### Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
+ ### Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
+ ### Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.




## **Решение**


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
 neighbor 150.150.150.150 route-map transit out
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
ip route 192.168.0.0 255.255.0.0 Null0
!
!
route-map transit permit 5
 match as-path 1



```

```
R14#sh ip bgp
BGP table version is 13, local router ID is 14.14.14.14
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r>  0.0.0.0          77.14.2.2                              0 101 i
 *>  77.14.2.0/24     0.0.0.0                  0         32768 i

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
 neighbor 140.140.140.140 route-map transit out
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 Null0
ip route 192.168.0.0 255.255.0.0 Null0
!
!
ip prefix-list 102 seq 5 deny 192.168.0.80/28
ip prefix-list 102 seq 10 permit 0.0.0.0/0 le 32
!
route-map transit permit 5
 match as-path 1
!
route-map lamas permit 10
 set local-preference 200


```

```
R15#sh ip bgp
BGP table version is 7, local router ID is 15.15.15.15
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r>  0.0.0.0          77.15.2.2                     200      0 301 i
 *>i 77.14.2.0/24     140.140.140.140          0    100      0 i
 *>  77.15.2.0/24     0.0.0.0                  0         32768 i
 *>  172.16.0.0       77.15.2.2                     200      0 301 520 2042 i


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
ip prefix-list SPB seq 5 permit 172.16.0.0/16



