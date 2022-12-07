# **Лабораторная работа. "Конфигурация безопасности коммутатора"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Топология.png?raw=true)

## **Цели:**
+ ### Часть 1. Настройка основного сетевого устройства
+ ### Часть 2. Настройка сетей VLAN
+ ### Часть 3. Настройки безопасности коммутатора.


## **Решение**
## **Часть 1. Настройка основного сетевого устройства**

### **Шаг 1. Создайте сеть.**
### **Шаг 2. Настройте маршрутизатор R1.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/R1.png?raw=true)


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/R1_brief.png?raw=true)


### **Шаг 3. Настройка и проверка основных параметров коммутатора.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/DescS1.png?raw=true)


### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/DescS2.png?raw=true)


## **Часть 2. Настройка сетей VLAN на коммутаторах.**

### **Шаг 1. Сконфигруриуйте VLAN 10.**


### **Шаг 2. Сконфигруриуйте SVI для VLAN 10.**

### **Шаг 3. Настройте VLAN 333 с именем Native на S1 и S2.**

### **Шаг 4. Настройте VLAN 999 с именем ParkingLot на S1 и S2.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Lot_S1.png?raw=true)

*int vlan 10

*desc Vlan10

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Lot_S2.png?raw=true)


*int vlan 20

*desc Vlan20

## **Часть 3. Настройки безопасности коммутатора.**

### **Шаг 1. Релизация магистральных соединений 802.1Q.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/TrunkS1.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/NON1.png?raw=true)


### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/TrunkS2.png?raw=true)


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/NON2.png?raw=true)


### **Шаг 2. Настройка портов доступа.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/AC(S1).png?raw=true)



### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/AC(S2).png?raw=true)


### **Шаг 3. Безопасность неиспользуемых портов коммутатора**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/SH1.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/ST1.png?raw=true)


### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/SH2.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/ST2.png?raw=true)



### **Шаг 4. Документирование и реализация функций безопасности порта.**

a.	На S1, введите команду **show port-security interface f0/6**  для отображения настроек по умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/SH_PORTSEC1.png?raw=true)



b.	На S1 включите защиту порта на F0/6 со следующими настройками:


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Conf_portsec1.png?raw=true)


c. Проверьте безопасность порта на S1 F0/6.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Protect(S1).png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/portsec(S1).png?raw=true)


d.	Включите безопасность порта для F0/18 на S2. Настройте каждый активный порт доступа таким образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую конфигурацию.


e.	Настройте следующие параметры безопасности порта на S2 F0/18:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Protect(S2).png?raw=true)



f.	Проверка функции безопасности портов на S2 F0/18.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/fa018(S2).png?raw=true)


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/portsec(S2).png?raw=true)




### **Шаг 5. Реализовать безопасность DHCP snooping.**


a.	На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10.


b.	Настройте магистральные порты на S2 как доверенные порты.


c.	Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Limit(S2).png?raw=true)


d.	Проверка DHCP Snooping на S2.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/SH(SNOOP)S2.png?raw=true)


e.	В командной строке на PC-B освободите, а затем обновите IP-адрес.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/release(S2).png?raw=true)


f.	Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Bind(S2).png?raw=true)



### **Шаг 6. Реализация PortFast и BPDU Guard**


a.	Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах.



b.	Включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.


### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/bpduguard(S1).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/bpduguard(S2).png?raw=true)



c.	Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/detail(S1).png?raw=true)


![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/run(S1).png?raw=true)

### **Шаг 7. Проверьте наличие сквозного ⁪подключения.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ9/Ping.png?raw=true)












