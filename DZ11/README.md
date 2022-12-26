# **Лабораторная работа. "Настройка и проверка расширенных списков контроля доступа"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/Топлогия.png?raw=true)

## **Цели:**
+ ### Часть 1. Создание сети и настройка основных параметров устройства.
+ ### Часть 2. Настройка и проверка списков расширенного контроля доступа.


## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть.**
### **Шаг 2. Произведите базовую настройку маршрутизаторов.**

### R1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/R1(ст).png?raw=true)

*enable secret class

### R2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/R2(ст).png?raw=true)

*enable secret class

### **Шаг 3. Настройте базовые параметры каждого коммутатора.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S1(ст).png?raw=true)


### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S2(ст).png?raw=true)


## **Часть 2. Настройка сетей VLAN на коммутаторах.**


### **Шаг 1. Создайте сети VLAN на коммутаторах.**


a.	Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше таблицы.


b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 


c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking Lot, настройте их для статического режима доступа и административно деактивируйте их.

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S1(Park).png?raw=true)


*no switchport trunk allowed vlan 20,30,40,1000


### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S2(Park).png?raw=true)

*no switchport trunk allowed vlan 20,30,40,1000


### **Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.**

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.


b.	Выполните команду show vlan brief, чтобы убедиться, что сети VLAN назначены правильным интерфейсам.

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S1(ac).png?raw=true)


### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S2(ac).png?raw=true)




## **Часть 3. Настройте транки (магистральные каналы).**


### **Шаг 1. Вручную настройте магистральный интерфейс F0/1.**


a.	Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать магистральную связь. Не забудьте сделать это на обоих коммутаторах.


b.	В рамках конфигурации транка установите для native vlan значение 1000 на обоих коммутаторах. При настройке двух интерфейсов для разных собственных VLAN сообщения об ошибках могут отображаться временно.


c.	В качестве другой части конфигурации транка укажите, что VLAN 10, 20, 30 и 1000 разрешены в транке.

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S1(N).png?raw=true)

*switchport trunk allowed vlan 20,30,40,1000

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S2(N).png?raw=true)

*switchport trunk allowed vlan 20,30,40,1000

d.	Выполните команду show interfaces trunk для проверки портов магистрали, собственной VLAN и разрешенных VLAN через магистраль.

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S1(intTR).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S2(intTR).png?raw=true)




### **Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.**


a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.

b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

c.	Используйте команду show interfaces trunk для проверки настроек транка.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/S1(R1).png?raw=true)



## **Часть 4. Настройте маршрутизацию.**

### **Шаг 1. Настройка маршрутизации между сетями VLAN на R1.**


a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.

b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP-адреса. Включите описание для каждого подинтерфейса.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/ROS.png?raw=true)


c.	Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/LO1.png?raw=true)

d.	С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/LO_BR.png?raw=true)




### **Шаг 2. Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1**


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/R2(G001).png?raw=true)

*ip route 0.0.0.0 10.20.0.1

## **Часть 5. Настройте удаленный доступ**

### **Шаг 1. Настройте все сетевые устройства для базовой поддержки SSH.**


a.	Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!

b.	Используйте ccna-lab.com в качестве доменного имени.

c.	Генерируйте криптоключи с помощью 1024 битного модуля.

d.	Настройте первые пять линий VTY на каждом устройстве, чтобы поддерживать только SSH-соединения и с локальной аутентификацией.


### **R1:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/R1(SSH).png?raw=true)

*line vty 0 4

*transport input ssh

*login local

### **R2:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/R2(SSH).png?raw=true)

*line vty 0 4

*transport input ssh

*login local


### **Шаг 2. Включите защищенные веб-службы с проверкой подлинности на R1.**

a.	Включите сервер HTTPS на R1.


b.	Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.


## **Часть 6. Проверка подключения**


### **Шаг 1. Настройте узлы ПК.**


### **Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.**


### **PC-A(Ping):**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/PING(PC-A).png?raw=true)

### **PC-B(Ping):**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/PING(PC-B).png?raw=true)

### **PC-B(https):**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/https-PCB1.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/https-PCB2.png?raw=true)

### **PC-B(SSH):**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/SSH-PCB.png?raw=true)



## **Часть 7. Настройка и проверка списков контроля доступа (ACL)**

**Политика1.** Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен). 


**Политика 2.** Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).


**Политика 3.** Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. 


**Политика 4.** Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам. 



### **Шаг 1. Проанализируйте требования к сети и политике безопасности для планирования реализации ACL.**


### **Шаг 2. Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности.**


### **Шаг 3. Убедитесь, что политики безопасности применяются развернутыми списками доступа.**


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/ACL.png?raw=true)


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/ACL(int).png?raw=true)


### **PC-A:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/PC-A(Ping).png?raw=true)

### **PC-B:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/PC-B(Ping).png?raw=true)

### **SSH:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ11/SSH(ACL).png?raw=true)

















