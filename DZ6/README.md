# **Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями "**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Топология.png?raw=true)

## **Задачи:**
+ ### Часть 1. Создание сети и настройка основных параметров устройства
+ ### Часть 2. Создание сетей VLAN и назначение портов коммутатора
+ ### Часть 3. Настройка транка 802.1Q между коммутаторами.
+ ### Часть 4. Настройка маршрутизации между сетями VLAN
+ ### Часть 5. Проверка, что маршрутизация между VLAN работает


## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Настройте базовые параметры для маршрутизатора.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Часть1-Шаг2.png?raw=true)

### **Шаг 3. Настройте базовые параметры каждого коммутатора.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Часть1-Шаг3.png?raw=true)

### **Шаг 4. Настройте узлы ПК.**

**PC-A:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/PC-A.png?raw=true)

**PC-B:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/PC-B.png?raw=true)

## **Часть 2. Создание сетей VLAN и назначение портов коммутатора**

### **Шаг 1. Создайте сети VLAN на коммутаторах.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Часть2-Шаг1(S1).png?raw=true)

### S1(config)#int vlan 10
### S1(config-if)#ip addr 192.168.10.11 255.255.255.0
### S1(config-if)#no sh
### S1(config)#vlan 30

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Часть2-Шаг1(S2).png?raw=true)

### S2(config)#int vlan 10
### S2(config-if)#ip address 192.168.10.12 255.255.255.0
### S2(config-if)#no sh


### S2(config)#vlan 20

### **Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Часть2-Шаг2(S1).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Часть2-Шаг2(S2).png?raw=true)


## **Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами**

### **Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Native_Vlan1000(S1).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Native_Vlan1000(S2).png?raw=true)

### **Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.**



## **Часть 4. Настройка маршрутизации между сетями VLAN**

### **Шаг 1. Настройте маршрутизатор.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Sub_R1.png?raw=true)

**Для vlan 1000:** 

**R1(config)#int g0/0/1.1000**

**R1(config-subif)#encapsulation dot1Q 1000 native**

Описание:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Описание.png?raw=true)

## **Часть 5. Проверьте, работает ли маршрутизация между VLAN**

### **Шаг 1. Выполните следующие тесты с PC-A. Все должно быть успешно.**

Эхо-запрос с PC-A на шлюз по умолчанию:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/PC-А(ping_шлюза).png?raw=true)

Эхо-запрос с PC-A на PC-B:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/PC-А(ping_PC-B).png?raw=true)

Ping с компьютера PC-A на коммутатор S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/Ping%20S2.png?raw=true)

### **Шаг 2. Пройдите следующий тест с PC-B**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ6/tracert.png?raw=true)

### *Вопрос:*

+ **Какие промежуточные IP-адреса отображаются в результатах?** Ответ: адрес роутера









