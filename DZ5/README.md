# **Лабораторная работа. "*Доступ к сетевым устройствам по протоколу SSH*"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/Топология.png?raw=true)

## **Задачи:**
+ ### Часть 1. Настройка основных параметров устройства
+ ### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
+ ### Часть 3. Настройка коммутатора для доступа по протоколу SSH
+ ### Часть 4. SSH через интерфейс командной строки (CLI) коммутатора


## **Решение**
## **Часть 1. Настройка основных параметров устройств**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Выполните инициализацию и перезагрузку маршрутизатора и коммутатора.**
### **Шаг 3. Настройте маршрутизатор:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/Шаг3.png?raw=true)

### **Шаг 4. Настройте компьютер PC-A:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/PC-A%20IP-адрес%20и%20маску%20подсети.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/Шлюз.png?raw=true)

### **Шаг 5. Проверьте подключение к сети:**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/Ping_R1.png?raw=true)

## **Часть 2. Настройка маршрутизатора для доступа по протоколу SSH**

### **Шаг 1. Настройте аутентификацию устройств.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/ip_domain-name.png?raw=true)



### **Шаг 2. Создайте ключ шифрования с указанием его длины**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/SSh_keygen.png?raw=true)


### **Шаг 3. Создайте имя пользователя в локальной базе учетных записей**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/username_admin.png?raw=true)

### **Шаг 4. Активируйте протокол SSH на линиях VTY.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/Transpor_input.png?raw=true)

*R1(config-line)#login local

### **Шаг 5. Сохраните текущую конфигурацию в файл загрузочной конфигурации.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/RUN.png?raw=true)

### **Шаг 6. Установите соединение с маршрутизатором по протоколу SSH**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/STAYOUT.png?raw=true)

## **Часть 3. Настройка коммутатора для доступа по протоколу SSH**

### **Шаг 1. Настройте основные параметры коммутатора.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/SW1.png?raw=true)


### **Шаг 2. Настройте коммутатор для соединения по протоколу SSH.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/SSH_S0.png?raw=true)

*S1(config)#ip ssh ver 2

*S1(config-line)#transport in telnet - не нужен(По заданию написано: **e.	Активируйте протоколы Telnet и SSH на линиях VTY.**)

### **Шаг 3. Установите соединение с коммутатором по протоколу SSH.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/PC-A_S1.png?raw=true)

## **Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора**

### **Шаг 1. Посмотрите доступные параметры для клиента SSH в Cisco IOS.**

### **Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ5/Ctrl+Shift+6.png?raw=true)










