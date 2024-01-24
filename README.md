# DEMO2024
DEMO exam 2024 for dummies  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/23dba506-fe7d-4e66-9592-fad345099039)  

## [Задание](https://sysahelper.ru/mod/resource/view.php?id=14)  
## Описание задания   
Образец задания для демонстрационного экзамена по комплекту оценочной документации.  

### Модуль 1: Выполнение работ по проектированию сетевой инфраструктуры  
## **Задание 1 модуля 1** 
**1.	Выполните базовую настройку всех устройств:**  
**a.	Присвоить имена в соответствии с топологией:**  
![image](https://github.com/NyashMan/DEMO2023/assets/1348639/9a2a7cbb-7b8f-4a12-9cab-a90b7a0c080b)  

### **ISP**
```
su -
toor
hostnamectl set-hostname ISP; exec bash
нажать enter
```

### **CLI**
```
su -
toor
hostnamectl set-hostname CLI; exec bash
нажать enter
```

### **HQ-R**
```
su -
toor
hostnamectl set-hostname HQ-R; exec bash
нажать enter
```

### **HQ-SRV**
```
su -
toor
hostnamectl set-hostname HQ-SRV; exec bash
нажать enter
```

### **BR-R**
```
su -
toor
hostnamectl set-hostname BR-R; exec bash
нажать enter
```


### **BR-SRV**

```
su -
toor
hostnamectl set-hostname BR-SRV; exec bash
нажать enter
```

**b.	Рассчитайте IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1, чтобы эксперты могли проверить ваше рабочее место.**  
**Таблица №1**  
| Имя устройств  | IPv4 | IPv6 | Примечание | 
| ------------- | ------------- | ------------- | ------------- |
| CLI  | 10.0.1.2/24 255.255.255.0  |ens33 to ISP |
| ISP | 10.0.1.1/24 255.255.255.0  |ens33 - to CLI|
|  | 192.168.0.1/24 255.255.255.0  |ens160 to HQ-R|
|  | 192.168.1.1/24 255.255.255.0  |ens224 to BR-R|
| HQ-R  | 192.168.0.2/24 255.255.255.0  |ens192 - to ISP  |
|   | 10.0.0.1/24 255.255.255.0  |ens160 - to HQ  |
| HQ-SRV  | 10.0.0.2/26 255.255.255.192  |ens33 to HQ-R  |
| BR-R  | 192.168.1.2/24 255.255.255.0  |ens33 to ISP  |
|   | 10.0.2.1/28 255.255.255.240  |ens160 to BR  |
| BR-SRV  | 10.0.2.2/28 255.255.255.240  |ens33 to BR-R  |
| HQ-CLI  | 10.0.0.3/26 255.255.255.192  |
| HQ-AD  | 10.0.0.4/26 255.255.255.192  |

**c.	Пул адресов для сети офиса BRANCH - не более 16**

255.255.255.240 = /28

**d.	Пул адресов для сети офиса HQ - не более 64**

255.255.255.192 = /26

## Настройка адресации  
**Назначаем адресацию согласно ранее заполненной таблицы №1**  
