# **OSPF**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ6/1.png?raw=true)



## **Задачи:**
+ ### Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
+ ### Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
+ ### Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
+ ### Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
+ ### Настройка для IPv6 повторяет логику IPv4.


## **Решение**


## Адресное пространство:


### **Москва**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ6/ADR.xlsx%20-%20Excel%2019.06.2023%2011_52_44.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ6/R19.png?raw=true)


## Настройки:

### **Москва:**


### **Подключение между R19-R14 я обозначил как p2p т.к. они соденины на прямую и зоне 101 нет никого кроме них, поэтому нет необходимости в выборах DR и BDR.(С R20-R15 точно так же)**


### **R14:**
```
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
router ospf 1
 router-id 14.14.14.14
 area 101 stub no-summary


```
### **R19**:

```
interface Ethernet0/0
 ip address 192.168.0.82 255.255.255.240
 ip ospf 1 area 101
!
router ospf 1
 router-id 19.19.19.19
 area 101 stub
```

### **R15**:

```
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
router ospf 1
 router-id 15.15.15.15
 priority 120
 area 102 filter-list prefix 102 in
!
ip prefix-list 102 seq 5 deny 192.168.0.80/28
ip prefix-list 102 seq 10 permit 0.0.0.0/0 le 32
!
!
```



### **R20**:

```
interface Ethernet0/0
 ip address 192.168.0.114 255.255.255.240
 ip ospf network point-to-point
 ip ospf 1 area 102
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
router ospf 1
 router-id 20.20.20.20
```
### **R20(ip route):**

```
      192.168.0.0/24 is variably subnetted, 12 subnets, 2 masks
O IA     192.168.0.0/28 [110/30] via 192.168.0.113, 01:03:58, Ethernet0/0
O IA     192.168.0.16/28 [110/30] via 192.168.0.113, 01:16:20, Ethernet0/0
O IA     192.168.0.32/28 [110/20] via 192.168.0.113, 01:16:20, Ethernet0/0
O IA     192.168.0.48/28 [110/30] via 192.168.0.113, 01:04:03, Ethernet0/0
O IA     192.168.0.64/28 [110/30] via 192.168.0.113, 01:03:58, Ethernet0/0
O IA     192.168.0.96/28 [110/20] via 192.168.0.113, 01:16:20, Ethernet0/0
C        192.168.0.112/28 is directly connected, Ethernet0/0
L        192.168.0.114/32 is directly connected, Ethernet0/0
O IA     192.168.0.128/28 [110/31] via 192.168.0.113, 00:40:34, Ethernet0/0
O IA     192.168.0.144/28 [110/31] via 192.168.0.113, 00:39:46, Ethernet0/0
O IA     192.168.0.160/28 [110/30] via 192.168.0.113, 01:04:03, Ethernet0/0
O IA     192.168.0.176/28 [110/20] via 192.168.0.113, 01:16:20, Ethernet0/0
```
### **R19**:

```
interface Ethernet0/0
 ip address 192.168.0.82 255.255.255.240
 ip ospf 1 area 101
!
router ospf 1
 router-id 19.19.19.19
 area 101 stub
```

### SW4 и SW5 я так же поместил в зону 10 вместе с R12-R13, т.к. они были заменены на l3 коммутаторы и настроены соотвествующе 


### **R12:**

```
interface Ethernet0/0
 ip address 192.168.0.50 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/1
 ip address 192.168.0.1 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/2
 ip address 192.168.0.17 255.255.255.240
 ip ospf 1 area 0
!
interface Ethernet0/3
 ip address 192.168.0.33 255.255.255.240
 ip ospf 1 area 0
!
router ospf 1
 router-id 12.12.12.12
 area 10 stub
```


### **SW4:**

```
!
interface Ethernet1/0
 no switchport
 ip address 192.168.0.2 255.255.255.240
 ip ospf 1 area 10
 duplex auto
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
!
router ospf 1
 router-id 4.4.4.4
 area 10 stub
 ```

### **R13:**

```
interface Ethernet0/0
 ip address 192.168.0.162 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/1
 ip address 192.168.0.49 255.255.255.240
 ip ospf 1 area 10
!
interface Ethernet0/2
 ip address 192.168.0.97 255.255.255.240
 ip ospf 1 area 0
!
interface Ethernet0/3
 ip address 192.168.0.65 255.255.255.240
 ip ospf 1 area 0
!
router ospf 1
 router-id 13.13.13.13
 area 10 stub
 ```

### **SW5:**

```
interface Ethernet1/0
 no switchport
 ip address 192.168.0.161 255.255.255.240
 ip ospf 1 area 10
 duplex auto
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
!
router ospf 1
 router-id 5.5.5.5
 area 10 stub
 ```










