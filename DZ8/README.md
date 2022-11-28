# **Лабораторная работа. "Настройка DHCPv6"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Топология.png?raw=true)

## **Цели:**
+ ### Часть 1. Создание сети и настройка основных параметров устройства
+ ### Часть 2. Проверка назначения адреса SLAAC от R1
+ ### Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1
+ ### Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1
+ ### Часть 5. Настройка и проверка DHCPv6 Relay на R2



## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Настройте базовые параметры каждого коммутатора.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/S1(база).png?raw=true)

### **S1**#copy run start

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/S2(база).png?raw=true)

### **S2**#copy run start

### **Шаг 3. Произведите базовую настройку маршрутизаторов.**

### R1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/R1(база).png?raw=true)

### **R1**#copy run start

### R2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/R2(база).png?raw=true)

### **R2**#copy run start

### **Шаг 4. Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.**

### R1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/!Маршрутизация(R1).png?raw=true)

### R2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/!Маршрутизация(R2).png?raw=true)


## **Часть 2. Проверка назначения адреса SLAAC от R1**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/ipconfig(PC-A).png?raw=true)

### *Вопрос:* **Откуда взялась часть адреса с идентификатором хоста?** Ответ: сработал механизм EUI-64, который позволил хосту в IPv6 самостоятельно генерировать себе идентификатор интерфейса


## **Часть 3. Настройка и проверка сервера DHCPv6 на R1**

### **Шаг 1. Более подробно изучите конфигурацию PC-A.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/ipconfig_ALL(PC-A).png?raw=true)

### **Шаг 2. Настройте R1 для предоставления DHCPv6 без состояния для PC-A.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/DHCP(R1).png?raw=true)

### PC-A:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/PC-A(ipconfig).png?raw=true)



## **Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Часть4(а).png?raw=true)


## **Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.**


### **Шаг 1. Включите PC-B и проверьте адрес SLAAC, который он генерирует.** 

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/PC-B(ipconfig).png?raw=true)



# **Лабораторная работа. "Реализация DHCPv4"**
## **Топология** 

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Tопология1.png?raw=true)

## **Таблица адресации**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/таблица2.png?raw=true)


## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создание схемы адресации.**

### **Шаг 2. Создайте сеть согласно топологии.**

### **Шаг 3. Произведите базовую настройку маршрутизаторов.**


### R1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/ADDR.png?raw=true)

*#enable secret class

### R2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/ADDR2.png?raw=true)

*#enable secret class

### **Шаг 4. Настройка маршрутизации между сетями VLAN на маршрутизаторе R1.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/R1(sub).png?raw=true)


### **Шаг 5. Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов.**

**a.**	Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.

**b.**	Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.

**c.**	Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе.

### R2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/ipROUTE(R2).png?raw=true)

### R1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/ipROUTE(R1).png?raw=true)

**d.**	Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Ping(R1-R2).png?raw=true)



### **Шаг 6. Настройте базовые параметры каждого коммутатора.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/S1(стандарт).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/S2(стандарт).png?raw=true)


### **Шаг 7. Создайте сети VLAN на коммутаторе S1.**

**a.**	Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.

**b.**	Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Vlan(S1).png?raw=true)


**c.**	Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Vlan(S2).png?raw=true)


**d.**	Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/SWPORT(S1).png?raw=true)


### **Шаг 8. Назначьте сети VLAN соответствующим интерфейсам коммутатора.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/SWPORT(S1).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/Vlan(S2).png?raw=true)

### *Вопрос:* **Почему интерфейс F0/5 указан в VLAN 1?** Ответ: По умолчанию все интерфейсы находятся в vlan 1


### **Шаг 9. Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/TRUNK.png?raw=true)



### *Вопрос:* **Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?** Ответ: В диапазоне от 192.168.1.2-63


## **Часть 2. Настройка и проверка двух серверов DHCPv4 на R1.**


### **Шаг 1. Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP для подсети A.**

### **Шаг 2. Сохраните конфигурацию.**

### Vlan 100:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/POOL100.png?raw=true)

*#copy run start

### Vlan 200

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/POOL200.png?raw=true)

### **Шаг 3.Проверка конфигурации сервера DHCPv4.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/SH_IP_DHCP_POOL.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/BIND.png?raw=true)

### **Шаг 4. Попытка получить IP-адрес от DHCP на PC-A.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/PC111.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/PC222.png?raw=true)


## **Часть 3.	Настройка и проверка DHCP-ретрансляции на R2**

### **Шаг 1. Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/HELPER.png?raw=true)

### **Шаг 2. Попытка получить IP-адрес от DHCP на PC-B**

### ipconfig /all:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/CONFIG(B1).png?raw=true)

### ipconfig:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/IP_conf(PC-B).png?raw=true)


### Ping R1(G0/0/1):

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/PING(PCB-R1).png?raw=true)

### show ip dhcp binding:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ8/BIND(R1).png?raw=true)




