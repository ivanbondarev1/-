# **Базовая настройка коммутатора**
## **Топология** 
![](https://github.com/ivanbondarev1/-/blob/main/DZ1/Cisco.png?raw=true)

## **Задание:**
+ ### Проверить конфигурацию коммутатора по умолчанию
+ ### Создание сети и настройка основных параметров устройства
+ ### Проверка сетевых подключений

## **Решение**
## **Часть 1. Создание сети и проверка настроек коммутатора по умолчанию**
### **Шаг 1. Создайте сеть согласно топологии.**
### 1. Соединение коммутатора с компьютером консольным кабелем:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/Cisco%20Packet%20Tracer%2005.09.2022%2011_19_13.png?raw=true)

Консольное подключение к коммутатору с компьютера:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2005.09.2022%2011_32_52.png?raw=true) 

### *Вопросы:*
+ #### **Почему нужно использовать консольное подключение для первоначальной настройки коммутатора?** Ответ: Потому что невозможно установить удаленное соединение с коммутатором без предварительной настройки через консольный кабель

+ #### **Почему нельзя подключиться к коммутатору через Telnet или SSH?** Ответ: Потому что без предварительной настройки через консольный кабель коммутатор не может иметь ip адресс для подключения через SSH и telnet


### **Шаг 2. Проверьте настройки коммутатора по умолчанию.**
Вход в привилигированный режим:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2005.09.2022%2012_28_44.png?raw=true)

Файл *running configuration*:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2005.09.2022%2012_39_58.png?raw=true)
### *Вопросы:*
+ **Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?**
Ответ: 24
+ **Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?**
Ответ: 2
+ **Каков диапазон значений, отображаемых в vty-линиях?**
Ответ: (0 4) (5 15)

Файл *startup configuration*:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2005.09.2022%2013_01_14.png?raw=true)
### *Вопросы:*
+ **Почему появляется это сообщение?**
Ответ: Потому что не было внесено никаких изменений в файл стартовой конфигурации

Характеристики *SVI* для *VLAN 1*:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2005.09.2022%2013_12_11.png?raw=true)
### *Вопросы:*
+ **Назначен ли IP-адрес сети VLAN 1?**
Ответ: Нет
+ **Какой MAC-адрес имеет SVI?**
Ответ: 0040.0b17.a17b
+ **Данный интерфейс включен?**
Ответ: Нет

IP-свойства интерфейса *SVI* сети *VLAN 1*:
Ответ: Интерфес находится в выключенном состоянии поэтому никаких IP свойств нет 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2006.09.2022%2019_10_48.png?raw=true)


Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.

### *Вопросы:*
+ **Какие выходные данные вы видите?**
Ответ: 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2009.09.2022%2019_56_53.png?raw=true)

Cведения о версии ОС Cisco IOS на коммутаторе:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2006.09.2022%2019_21_21.png?raw=true)
### *Вопросы:*
+ **Под управлением какой версии ОС Cisco IOS работает коммутатор?**
Ответ: 15.0(2)SE4
+ **Как называется файл образа системы?**
Ответ: "flash:c2960-lanbasek9-mz.150-2.SE4"
+ **Какой базовый MAC-адрес назначен коммутатору?**
Ответ: 000A:4104.3506


Cвойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2006.09.2022%2020_06_23.png?raw=true)

### *Вопросы:*
+ **Интерфейс включен или выключен?**
Ответ: включен
+ **Что нужно сделать, чтобы включить интерфейс?**
Ответ: прописать команду "no shutdown"
+ **Какой MAC-адрес у интерфейса?**
Ответ: 000a.4104.3506
+ **Какие настройки скорости и дуплекса заданы в интерфейсе?**
Ответ: **скорость**:**auto** **дуплекс**:**auto**

Параметры сети VLAN по умолчанию на коммутаторе:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2006.09.2022%2020_30_05.png?raw=true)

### *Вопросы*
+ **Какое имя присвоено сети VLAN 1 по умолчанию?**
Ответ: default
+ **Какие порты расположены в сети VLAN 1?**
Ответ: fa0/1-24  Gig0/1-2
+ **Активна ли сеть VLAN 1?**
Ответ: да
+ **К какому типу сетей VLAN принадлежит VLAN по умолчанию?**
Ответ: к типу локальной сети

Cодержимое флеш-каталога:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2012_03_19.png?raw=true)

### *Вопросы:*
**Какое имя присвоено образу Cisco IOS?**
Ответ: "lanbasek9-mz.150-2.SE4"

## **Часть 2. Настройка базовых параметров сетевых устройств**
### **Шаг 1. Настройте базовые параметры коммутатора.**
Базовые параметры конфигурации:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2012_18_35.png?raw=true)

Новый баннер:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2012_24_22.png?raw=true)

Новый IP адресс для интерфейса:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2012_52_46.png?raw=true)

### *Вопросы:*
**Для чего нужна команда login?**
Ответ: Требовать заданный пароль

## **Шаг 2. Настройте IP-адрес на компьютере PC-A.**


Cвязь с адресом PC-A:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2019_17_28.png?raw=true)

## **Часть 3. Проверка сетевых подключений**
### **Шаг 1. Отобразите конфигурацию коммутатора**

+ *SW1#show run
+ Building configuration...
+
+ Current configuration : 1311 bytes
+ !
+ version 15.0
+ no service timestamps log datetime msec
+ no service timestamps debug datetime msec
+ service password-encryption
+ !
+ hostname SW1
+ !
+ enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
+ !
+ !
+ !
+ !
+ !
+ !
+ spanning-tree mode pvst
+ spanning-tree extend system-id
+ !
+ interface FastEthernet0/1
+ !
+ interface FastEthernet0/2
+ !
+ interface FastEthernet0/3
+ !
+ interface FastEthernet0/4
+ !
+ interface FastEthernet0/5
+ !
+ interface FastEthernet0/6
+ !
+ interface FastEthernet0/7
+ !
+ interface FastEthernet0/8
+ !
+ interface FastEthernet0/9
+ !
+ interface FastEthernet0/10
+ !
+ interface FastEthernet0/11
+ !
+ interface FastEthernet0/12
+ !
+ interface FastEthernet0/13
+ !
+ interface FastEthernet0/14
+ !
+ interface FastEthernet0/15
+ !
+ interface FastEthernet0/16
+ !
+ interface FastEthernet0/17
+ !
+ interface FastEthernet0/18
+ !
+ interface FastEthernet0/19
+ !
+ interface FastEthernet0/20
+ !
+ interface FastEthernet0/21
+ !
+ interface FastEthernet0/22
+ !
+ interface FastEthernet0/23
+ !
+ interface FastEthernet0/24
+ !
+ interface GigabitEthernet0/1
+ !
+ interface GigabitEthernet0/2
+ !
+ interface Vlan1
+ ip address 192.168.1.2 255.255.255.0
+ !
+ ip default-gateway 192.168.1.1
+ !
+ banner motd ^C
+ Unauthorized access is strictly prohibited. ^C
+ !
+ !
+ !
+ line con 0
+ password 7 0822455D0A16
+ login
+ !
+ line vty 0 4
+ password 7 0822455D0A16
+ login
+ line vty 5 15
+ password 7 0822455D0A16
+ login
+ !
+ !
+ !
+ !
+ end *


Параметры VLAN 1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2009.09.2022%2020_36_38.png?raw=true)

### *Вопросы:*
+ **Какова полоса пропускания этого интерфейса?** Ответ: 100000 Kbit
+ **В каком состоянии находится VLAN 1?** Ответ: в "поднятом" состоянии 
+ **В каком состоянии находится канальный протокол?** Ответ: в активном состоянии


### **Шаг 2. Протестируйте сквозное соединение, отправив эхо-запрос.**

Связь с адресом PC-A:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2019_17_28.png?raw=true)



Эхо-запрос на административный адрес интерфейса SVI коммутатора S1:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2007.09.2022%2019_20_05.png?raw=true)

## **Шаг 3. Проверьте удаленное управление коммутатором S1.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2009.09.2022%2020_53_39.png?raw=true)
