# **Проектирование сети**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/TOP.png?raw=true)

### В схему были внесены некоторые изменения. В Москве я убрал линк SW4 - R13 и SW5 - R12, т.к.  они не оказывают существенного влияния на схему, зато сложностей доставляют. В реальной жизни отсутствие данных линков являлось бы существенным недостатком, но здесь была цель опитимизировать схему. В Питере я сделал то же самое, но добавил линк R17 - R16, что бы компенсировать "уход" двух линков на схеме.

## **Задачи:**
+ ### Разработаете и задокументируете адресное пространство для лабораторного стенда.
+ ### Настроите ip адреса на каждом активном порту
+ ### Настроите каждый VPC в каждом офисе в своем VLAN.
+ ### Настроите VLAN/Loopbackup interface управления для сетевых устройств
+ ### Настроите сети офисов так, чтобы не возникало broadcast штормов, а использование линков было максимально оптимизировано



## **Решение**


## Адресное пространство:

### **Москва**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/MSK.png?raw=true)

### **Китор, Ламас**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Киторн%20Ламас.png?raw=true)


### **Триада**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/TRI.png?raw=true)


### **Питер**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/PIT.png?raw=true)



### **Лабытнаги, Чокурдах**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/CHOK.png?raw=true)


## Настройки:

### **Москва**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/МСК.png?raw=true)

### SW3:

```
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/3
!
interface Ethernet1/0
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan40
 ip address 192.168.1.3 255.255.255.0
!
```


### SW4:

```
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
 duplex auto
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
!
interface Vlan20
 ip address 192.168.0.146 255.255.255.240
 standby version 2
 standby 20 ip 192.168.0.145
 standby 20 preempt
!

```


### SW5:

```
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
 duplex auto
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
!
interface Vlan20
 ip address 192.168.0.147 255.255.255.240
 standby version 2
 standby 20 ip 192.168.0.145
 standby 20 priority 200
 standby 20 preempt

```



### SW2:

```
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/3
!
interface Ethernet1/0
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan40
 ip address 192.168.4.1 255.255.255.0
 ```


### R12:

```
interface Ethernet0/0
 no ip address
 shutdown
!
interface Ethernet0/1
 ip address 192.168.0.1 255.255.255.240
!
interface Ethernet0/2
 ip address 192.168.0.17 255.255.255.240
!
interface Ethernet0/3
 ip address 192.168.0.33 255.255.255.240
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
interface Ethernet2/0
 no ip address
 shutdown
!
interface Ethernet2/1
 no ip address
 shutdown
!
interface Ethernet2/2
 no ip address
 shutdown
!
interface Ethernet2/3
 no ip address
 shutdown
!



```



### R13:

```
interface Ethernet0/0
 ip address 192.168.0.162 255.255.255.240
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 ip address 192.168.0.97 255.255.255.240
!
interface Ethernet0/3
 ip address 192.168.0.65 255.255.255.240
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

```

### R14:

```
interface Ethernet0/0
 ip address 192.168.0.18 255.255.255.240
!
interface Ethernet0/1
 ip address 192.168.0.68 255.255.255.240
!
interface Ethernet0/2
 ip address 77.14.2.1 255.255.255.0
!
interface Ethernet0/3
 ip address 192.168.0.81 255.255.255.240
!
interface Ethernet1/0
 ip address 192.168.0.178 255.255.255.240
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
```

### R19:

```
interface Ethernet0/0
 ip address 192.168.0.82 255.255.255.240
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

```

### R15:

```
interface Ethernet0/0
 ip address 192.168.0.98 255.255.255.240
!
interface Ethernet0/1
 ip address 192.168.0.34 255.255.255.240
!
interface Ethernet0/2
 ip address 77.15.2.1 255.255.255.0
!
interface Ethernet0/3
 ip address 192.168.0.113 255.255.255.240
!
interface Ethernet1/0
 ip address 192.168.0.177 255.255.255.240
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
```

### R20:

```
interface Ethernet0/0
 ip address 192.168.0.114 255.255.255.240
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

```

### **Питер:**


### SW10:

```
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/2
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan40
 ip address 172.16.3.2 255.255.255.0
```

### R17:

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
!
interface Ethernet0/2
 ip address 172.16.0.193 255.255.255.192
!
interface Ethernet0/3
 no ip address
 shutdown


```

### R16:

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
!
interface Ethernet0/2
 ip address 172.16.0.194 255.255.255.192
!
interface Ethernet0/3
 ip address 172.16.1.1 255.255.255.192


```

### R18:

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

```

### **Чокурдах**

### SW29:

```
interface Ethernet0/0
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/1
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/3
```



### R28:

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
!

```













