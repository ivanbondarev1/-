# **Проектирование сети**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Топлогия.png?raw=true)

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

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Москва.png?raw=true)

### **Китор, Ламас**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Киторн%20Ламас.png?raw=true)


### **Триада**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Триада%20.png?raw=true)


### **Питер**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Питер.png?raw=true)



### **Лабытнаги, Чокурдах**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ4/Лаб%20Чоку.png?raw=true)


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
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan40
 ip address 192.168.1.1 255.255.255.0

```


### SW5:

```
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
 ip address 192.168.1.2 255.255.255.0
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
 no ip address
!
interface Ethernet0/1.10
 encapsulation dot1Q 10
 ip address 77.12.1.1 255.255.255.0
 standby version 2
 standby 10 ip 77.12.1.10
 standby 10 priority 200
 standby 10 preempt
!
interface Ethernet0/1.20
 encapsulation dot1Q 20
 ip address 77.12.11.2 255.255.255.0
 standby version 2
 standby 20 ip 77.12.11.20
 standby 20 preempt
!
interface Ethernet0/1.40
 encapsulation dot1Q 40
 ip address 192.168.1.6 255.255.255.0
 standby version 2
 standby 40 ip 192.168.1.40
 standby 40 preempt
!
interface Ethernet0/2
 ip address 77.12.2.1 255.255.255.0
!
interface Ethernet0/3
 ip address 77.12.3.1 255.255.255.0
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

```



### R13:

```
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.10
 encapsulation dot1Q 10
 ip address 77.12.1.11 255.255.255.0
 standby version 2
 standby 10 ip 77.12.1.10
 standby 10 preempt
!
interface Ethernet0/0.20
 encapsulation dot1Q 20
 ip address 77.12.11.15 255.255.255.0
 standby version 2
 standby 20 ip 77.12.11.20
 standby 20 priority 200
 standby 20 preempt
!
interface Ethernet0/0.40
 encapsulation dot1Q 40
 ip address 192.168.1.5 255.255.255.0
 standby version 2
 standby 40 ip 192.168.1.40
 standby 40 preempt
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 ip address 77.13.2.1 255.255.255.0
!
interface Ethernet0/3
 ip address 77.13.3.1 255.255.255.0
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
 ip address 78.18.2.1 255.255.255.0
 standby version 2
 standby 10 ip 78.18.2.10
 standby 10 preempt
!
interface Ethernet0/0.20
 encapsulation dot1Q 20
 ip address 78.18.3.1 255.255.255.0
 standby version 2
 standby 20 ip 78.18.3.20
 standby 20 preempt
!
interface Ethernet0/0.40
 encapsulation dot1Q 40
 ip address 172.16.3.3 255.255.255.0
 standby version 2
 standby 40 ip 172.16.3.40
 standby 40 preempt
!
interface Ethernet0/1
 ip address 78.18.1.2 255.255.255.0
!
interface Ethernet0/2
 ip address 78.17.2.1 255.255.255.0
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
 ip address 78.18.2.2 255.255.255.0
 standby version 2
 standby 10 ip 78.18.2.10
 standby 20 preempt
!
interface Ethernet0/0.20
 encapsulation dot1Q 20
 ip address 78.18.3.2 255.255.255.0
 standby version 2
 standby 20 ip 78.18.3.20
 standby 20 preempt
!
interface Ethernet0/0.40
 encapsulation dot1Q 40
 ip address 172.16.3.5 255.255.255.0
 standby version 2
 standby 40 ip 172.16.3.40
 standby 40 preempt
!
interface Ethernet0/1
 ip address 78.18.0.2 255.255.255.0
!
interface Ethernet0/2
 ip address 78.17.2.2 255.255.255.0
!
interface Ethernet0/3
 ip address 78.16.3.1 255.255.255.0
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
 ip address 14.28.2.1 255.255.255.0
!
interface Ethernet0/2.20
 encapsulation dot1Q 20
 ip address 14.28.3.1 255.255.255.0
!
interface Ethernet0/2.40
 encapsulation dot1Q 40
 ip address 172.16.4.1 255.255.255.0
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

```













