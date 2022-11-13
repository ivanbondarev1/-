# **Лабораторная работа. "Развертывание коммутируемой сети с резервными каналами"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/Топология.png?raw=true)

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

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/S1(стандарт).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/S2(стандарт).png?raw=true)

### S3:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/S3(стандарт).png?raw=true)

### **Шаг 4. Проверьте связь.**



### Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2? **Ответ:** Да

### Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3? **Ответ:** Да

### Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3? **Ответ:** Да


## **Часть 2. Определение корневого моста**
### **Шаг 1. Отключите все порты на коммутаторах**
### **Шаг 2. Настройте подключенные порты в качестве транковых.**
### **Шаг 3. Включите порты F0/2 и F0/4 на всех коммутаторах.**
### **Шаг 4. Отобразите данные протокола spanning-tree.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S1).png?raw=true)

### S2:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S2).png?raw=true)

### S3:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S3).png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/схема1.png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/Lab___Building_a_Switched_Network_with_Redundant_Links-1801-cf158d.docx%20-%20Word%2008.11.2022%2018_52_03.png?raw=true)




## **Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов**

### **Шаг 1. Определите коммутатор с заблокированным портом.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S1).png?raw=true)

### S3:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S3).png?raw=true)

### **Шаг 2. Измените стоимость порта.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/int_cost.png?raw=true)

### **Шаг 3. Просмотрите изменения протокола spanning-tree.**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S1_8).png?raw=true)

### S3:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/sh_span(S3_8).png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/S1-S3.png?raw=true)

### **Шаг 4. Удалите изменения стоимости порта.**

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/no_span.png?raw=true)

## **Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов**

### S1:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/no_sh(S1).png?raw=true)

### S3:

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/no_sh(S3).png?raw=true)

![](https://github.com/ivanbondarev1/Otus/blob/main/DZ7/Вопросы.png?raw=true)





