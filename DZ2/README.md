# **Лабораторная работа. "*Просмотр таблицы MAC-адресов коммутатора*"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Cisco%20Packet%20Tracer%2017.09.2022%2012_22_32.png?raw=true)

## **Задачи:**
+ ### Часть 1. Создание и настройка сети
+ ### Часть 2. Изучение таблицы МАС-адресов коммутатора


## **Решение**
## **Часть 1. Создание и настройка сети**
### **Шаг 1. Подключите сеть в соответствии с топологией:**
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Cisco%20Packet%20Tracer%2017.09.2022%2012_22_32.png?raw=true)
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.**
### **Шаг 4. Настройте базовые параметры каждого коммутатора:**

a.	Настройте имена устройств в соответствии с топологией:

**Switch1:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Switch4%2017.09.2022%2012_49_23.png?raw=true)

**Switch2:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Switch3%2017.09.2022%2012_49_53.png?raw=true)

**PC-1:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/PC%2017.09.2022%2013_01_04.png?raw=true)

**PC-2:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/PC1%2017.09.2022%2013_09_33.png?raw=true)

b.	Настройте IP-адреса, как указано в таблице адресации:

**IP для S1:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Switch4%2017.09.2022%2012_53_45.png?raw=true)

**IP для S2:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Switch3%2017.09.2022%2012_55_42.png?raw=true)

**IP для PC-A:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/PC%2017.09.2022%2013_01_18.png?raw=true)

**IP для PC-B**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/PC1%2017.09.2022%2013_09_51.png?raw=true)

c.	Назначьте cisco в качестве паролей консоли и VTY:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Пароль_vty-для_S1.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Пароль_vty-для_S2.png?raw=true)

d.	Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Режим-EXEC_SW1%20.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Режим-EXEC_SW2.png?raw=true)

Консольное подключение к коммутатору с компьютера:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ1/PC0%2005.09.2022%2011_32_52.png?raw=true) 

## **Часть 2. Изучение таблицы МАС-адресов коммутатора**
### **Шаг 1. Запишите МАС-адреса сетевых устройств.**

a.	Откройте командную строку на PC-A и PC-B и введите команду **ipconfig /all.**

PC-A:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/PC-A%20ipconfig.png?raw=true)

PC-B:
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/PC-B%20ipconfig.png?raw=true)

### *Вопрос:*
Назовите физические адреса адаптера Ethernet.

*MAC-адрес компьютера PC-A:* **0000.0C15.3BD7**

*MAC-адрес компьютера PC-B:* **0001.C9D1.9BD7**



b.	Подключитесь к коммутаторам S1 и S2 через консоль и введите команду show interface F0/1 на каждом коммутаторе.



**S2:**
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/show_intS2.png?raw=true)

**S1:**
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/show_intSW1.png?raw=true)
### *Вопросы:*

Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес — bia).

МАС-адрес коммутатора S1 Fast Ethernet 0/1: **0060.7067.4901**

МАС-адрес коммутатора S2 Fast Ethernet 0/1: **00d0.ffe7.c801**

### **Шаг 2. Просмотрите таблицу МАС-адресов коммутатора.**

a.	Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Подключение%20к%20S2%20через%20консоль.png?raw=true)


b.	В привилегированном режиме EXEC введите команду **show mac address-table** и нажмите клавишу ввода.

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ2/Таблица%20mac%20addr%20на%20S2.png?raw=true)

### *Вопросы:*

+ Записаны ли в таблице МАС-адресов какие-либо МАС-адреса?

**Ответ:** Да

+ Какие МАС-адреса записаны в таблице? 

**Ответ:** 0000.0c15.3bd7  ;  0060.7067.4901

+ С какими портами коммутатора они сопоставлены и каким устройствам принадлежат?

**Ответ:** портам Fa0/1, а принадлежат PC-A и S1

+ Если вы не записали МАС-адреса сетевых устройств в шаге 1, как можно определить, каким устройствам принадлежат МАС-адреса, используя только выходные данные команды **show mac address-table**? Работает ли это решение в любой ситуации?

**Ответ:** Надо знать к какому порту было подключено устройство. Если проводов много, то будет сложнее определять  

### **Шаг 3. Очистите таблицу МАС-адресов коммутатора S2 и снова отобразите таблицу МАС-адресов.**


a.	В привилегированном режиме EXEC введите команду **clear mac address-table** dynamic и нажмите клавишу Enter.




