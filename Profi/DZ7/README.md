# **IS-IS**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ6/EVE%20_%20Topology%20-%20Google%20Chrome%2025.06.2023%2020_54_41.png?raw=true)



## **Задачи:**
+ ### Настроите IS-IS в ISP Триада.
+ ### R23 и R25 находятся в зоне 2222.
+ ### R24 находится в зоне 24.
+ ### R26 находится в зоне 26.



## **Решение**


## Адресное пространство:


### **Москва**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/TRI.png?raw=true)




## Настройки:

### **Триада:**


### **R23 и R25 находятся в зоне 2222**




### **R23:**
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

```
```
R23#sh ip route isis
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

      10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
i L1     10.0.0.128/26 [115/20] via 10.0.0.2, 00:29:11, Ethernet0/1
i L2     10.0.0.192/26 [115/20] via 10.0.0.66, 00:29:11, Ethernet0/2
R23#
```
```
R23#sh isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/2       10.0.0.66       UP    9        R24.02           
R25            L1   Et0/1       10.0.0.2        UP    6        R25.01           
R25            L2   Et0/1       10.0.0.2        UP    7        R25.01 
```
### **R25**:

```
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
```

```
R25#sh ip route isis
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

      10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
i L1     10.0.0.64/26 [115/20] via 10.0.0.1, 00:30:40, Ethernet0/0
i L2     10.0.0.192/26 [115/20] via 10.0.0.130, 00:30:40, Ethernet0/2
R25#

```
```
R25#sh isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L1   Et0/0       10.0.0.1        UP    22       R25.01           
R23            L2   Et0/0       10.0.0.1        UP    26       R25.01           
R26            L2   Et0/2       10.0.0.130      UP    9        R26.02 
```

### **R26**:

```
interface Ethernet0/0
 ip address 10.0.0.194 255.255.255.192
 ip router isis
!
interface Ethernet0/1
 ip address 14.26.1.1 255.255.255.0
!
interface Ethernet0/2
 ip address 10.0.0.130 255.255.255.192
 ip router isis
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
```

```
R26#sh ip route isis
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

      10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
i L2     10.0.0.0/26 [115/20] via 10.0.0.129, 00:37:43, Ethernet0/2
i L2     10.0.0.64/26 [115/20] via 10.0.0.193, 00:37:43, Ethernet0/0

```

```
R26#sh isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/0       10.0.0.193      UP    22       R26.01           
R25            L2   Et0/2       10.0.0.129      UP    23       R26.02  
```


### **R24**:

```
interface Ethernet0/0
 ip address 101.21.2.2 255.255.255.0
!
interface Ethernet0/1
 ip address 10.0.0.193 255.255.255.192
 ip router isis
!
interface Ethernet0/2
 ip address 10.0.0.66 255.255.255.192
 ip router isis
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

```

```
R24#sh ip route isis
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

      10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
i L2     10.0.0.0/26 [115/20] via 10.0.0.65, 00:06:35, Ethernet0/2
i L2     10.0.0.128/26 [115/20] via 10.0.0.194, 00:39:38, Ethernet0/1
```

```
R24#sh isis neighbors

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L2   Et0/2       10.0.0.65       UP    28       R24.02           
R26            L2   Et0/1       10.0.0.194      UP    7        R26.01  
```













