# **Лабораторная работа. "*Настройка IPv6-адресов на сетевых устройствах*"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/Топология.png?raw=true)

## **Задачи:**
+ ### Часть 1.  Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
+ ### Часть 2. Ручная настройка IPv6-адресов
+ ### Часть 3. Проверка сквозного соединения


## **Решение**
## **Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора**

### **Шаг 1. Настройте маршрутизатор:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/R1(Базовые%20настройки).png?raw=true)

### **Шаг 2. Настройте коммутатор:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/S1(Базовые%20настройки)%20.png?raw=true)

## **Часть 2. Ручная настройка IPv6-адресов**

### **Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1.**

a.	Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.
Откройте окно конфигурации

**Gig0/0/0:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/R1(G000)ip-адрес.png?raw=true)

**Gig0/0/1:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/R1(G001)ip-адрес.png?raw=true)

b.	Введите команду **show ipv6 interface brief**, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный **IPv6-адрес**.
Примечание. Отображаемый локальный адрес канала основан на адресации **EUI-64**, которая автоматически использует **MAC-адрес** интерфейса для создания 128-битного локального **IPv6-адреса** канала.

c.	Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе **Ethernet** на **R1**.
Примечание. Каждый интерфейс маршрутизатора относится к отдельной сети. Пакеты с локальным адресом канала никогда не выходят за пределы локальной сети, а значит, для обоих интерфейсов можно указывать один и тот же локальный адрес канала.



![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/R1(G000-LinkLocal).png?raw=true)

d. Используйте выбранную команду, чтобы убедиться, что локальный адрес связи изменен на **fe80::1**.  


### **Шаг 2. Активируйте IPv6-маршрутизацию на R1.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/R1(G000)ip-адрес.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/R1(G001)ip-адрес.png?raw=true)

### **Шаг 3. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/S1(vlan1).png?raw=true)

### **Шаг 4. Назначьте компьютерам статические IPv6-адреса.**

**PC-A:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/PC-A(IPv6).png?raw=true)

**PC-B:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/PC-B(IPv6-).png?raw=true)

## **Часть 3. Проверка сквозного подключения**

+ С **PC-A** отправьте эхо-запрос на **FE80::1**. Это локальный адрес канала, назначенный **G0/1** на **R1**:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/PC-A(ping_FE801).png?raw=true)

+ Отправьте эхо-запрос на интерфейс управления **S1** с **PC-A**:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/PC-A(ping_S1).png?raw=true)

+ Введите команду **tracert** на **PC-A**, чтобы проверить наличие сквозного подключения к **PC-B:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/tracert_PC-B.png?raw=true)

+ С **PC-B** отправьте эхо-запрос на **PC-A:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/Ping(PC-B-PC-A).png?raw=true)

+ С **PC-B** отправьте эхо-запрос на локальный адрес канала **G0/0** на **R1:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ4/Ping_С_PC-B_R1.png?raw=true)