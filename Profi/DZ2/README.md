# **Лабораторная работа. "Развертывание коммутируемой сети с резервными каналами"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ2/Топология.png?raw=true)
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ2/Таблица.png?raw=true)

## **Цели:**
+ ### Часть 1. Создание сети и настройка основных параметров устройства
+ ### Часть 2. Выбор корневого моста
+ ### Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
+ ### Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов



## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Выполните инициализацию и перезагрузку коммутаторов.**

### **Шаг 3. Настройте базовые параметры каждого коммутатора.**

### S1:

```no ip domain-lookup
enable sec class
line con 0
pass cisco
login
exit
line vty 0 4
pass cisco
login
exit
service pas
logging synchronous 
banner motd *STAY_OUT*
int vlan 1
ip addr 192.168.1.1 255.255.255.0
end
copy run start
```

### S2:

```no ip domain-lookup
enable sec class
line con 0
pass cisco
login
exit
line vty 0 4
pass cisco
login
exit
service pas
logging synchronous 
banner motd *STAY_OUT*
int vlan 1
ip addr 192.168.1.2 255.255.255.0
end
copy run start
```

### S3:

```no ip domain-lookup
enable sec class
line con 0
pass cisco
login
exit
line vty 0 4
pass cisco
login
exit
service pas
logging synchronous 
banner motd *STAY_OUT*
int vlan 1
ip addr 192.168.1.3 255.255.255.0
end
copy run start
```

### **Шаг 4. Проверьте связь.**


### Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2? **Ответ:** Да

### Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3? **Ответ:** Да

### Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3? **Ответ:** Да


## **Часть 2. Определение корневого моста**
### **Шаг 1. Отключите все порты на коммутаторах**
### **Шаг 2. Настройте подключенные порты в качестве транковых.**
### **Шаг 3. Включите порты F0/2 и F0/4 на всех коммутаторах.**
### **Шаг 4. Отобразите данные протокола spanning-tree.**


``` 
S1#sh spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.434C.B255
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```



```
S2#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0010.110D.44BD
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/2            Root FWD 19        128.2    P2p
```

```
S3#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        4(FastEthernet0/4)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.9744.5573
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 19        128.2    P2p
Fa0/4            Root FWD 19        128.4    P2p
```


![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ2/Таблица.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ2/вопросы1.png?raw=true)


## **Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов**

### **Шаг 1. Определите коммутатор с заблокированным портом.**


```
S3#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        4(FastEthernet0/4)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.9744.5573
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 19        128.2    P2p
Fa0/4            Root FWD 19        128.4    P2p
```


### **Шаг 2. Измените стоимость порта.**

### **Шаг 3. Просмотрите изменения протокола spanning-tree.**

```
S3#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        4(FastEthernet0/4)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.9744.5573
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 18        128.2    P2p
Fa0/4            Root FWD 19        128.4    P2p
```

```
S2#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0010.110D.44BD
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Root FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```


### **Шаг 4. Удалите изменения стоимости порта.**

``
S3(config-if)#no spanning-tree cost 18 
``

## **Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов**

```
S2#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0010.110D.44BD
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/2            Altn BLK 19        128.2    P2p
Fa0/3            Desg FWD 19        128.3    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```

```
S3#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.434C.B255
             Cost        19
             Port        3(FastEthernet0/3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.9744.5573
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Altn BLK 19        128.1    P2p
Fa0/2            Altn BLK 18        128.2    P2p
Fa0/4            Altn BLK 19        128.4    P2p
Fa0/3            Root FWD 19        128.3    P2p
```

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ2/Конец.png?raw=true)



























































