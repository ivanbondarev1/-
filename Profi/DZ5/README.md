# **PBR**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/TOP.png?raw=true)



## **Задачи:**
+ ### Настроите политику маршрутизации для сетей офиса.
+ ### Распределите трафик между двумя линками с провайдером.
+ ### Настроите отслеживание линка через технологию IP SLA
+ ### Настройте для офиса Лабытнанги маршрут по-умолчанию.



## **Решение**


## Адресное пространство:


### **Триада**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/TRI.png?raw=true)


### **Лабытнаги, Чокурдах**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/CHOK.png?raw=true)


## Настройки:

### **Чокурдах:**

### **R28:**

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
interface Ethernet0/0
 ip address 14.26.1.2 255.255.255.0
!
interface Ethernet0/1
 ip address 14.25.3.2 255.255.255.0
!
interface Ethernet0/2
 no ip address
!
interface Ethernet0/2.10
 encapsulation dot1Q 10
 ip address 172.16.4.1 255.255.255.192
 ip policy route-map VLAN10
!
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 172.16.4.65 255.255.255.192
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
ip route 0.0.0.0 0.0.0.0 14.26.1.1 30 track 10
ip route 0.0.0.0 0.0.0.0 14.25.3.1 20 track 20
!
ip access-list standard VLAN10
 permit 172.16.4.0 0.0.0.63
ip access-list standard VLAN20
 permit 172.16.4.64 0.0.0.63
!
ip access-list extended VLAN10-EX
 permit ip any any
ip access-list extended VLAN20-EX
 permit ip any any
!
ip sla 10
 icmp-echo 14.26.1.1 source-ip 14.26.1.2
 frequency 5000
 timeout 10000
ip sla schedule 10 life forever start-time now
ip sla 20
 icmp-echo 14.25.3.1 source-ip 14.25.3.2
 frequency 5000
 timeout 10000
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
```



### **Триада:**

### **R26:**

### Что бы проверить работоспособность IPsla я прописал статичесчкие маршруты до офиса "Чокурдах"

```
interface Ethernet0/0
 ip address 10.0.0.194 255.255.255.192
!
interface Ethernet0/1
 ip address 14.26.1.1 255.255.255.0
!
interface Ethernet0/2
 ip address 10.0.0.130 255.255.255.192
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
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 172.16.4.0 255.255.255.192 14.26.1.2
ip route 172.16.4.64 255.255.255.192 14.26.1.2


```

### **R25:**

```
interface Ethernet0/0
 ip address 10.0.0.2 255.255.255.192
!
interface Ethernet0/1
 ip address 89.25.1.1 255.255.255.0
!
interface Ethernet0/2
 ip address 10.0.0.129 255.255.255.192
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
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 172.16.4.0 255.255.255.192 14.26.3.2
ip route 172.16.4.64 255.255.255.192 14.26.3.2

```


### **Лабытнаги:**


### **R27:**

```
interface Ethernet0/0
 ip address 89.25.1.2 255.255.255.0
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
ip route 0.0.0.0 0.0.0.0 89.25.1.1
```




































