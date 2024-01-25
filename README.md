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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fb6aaa33-dd89-42fe-bf0e-3485f7eb6dc4)  

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
| Имя устройства | IPv4            | IPv6                | Имя интерфейса |
|-----------------|-----------------|--------------------|----------------|
| CLI             | 10.0.1.2/24 255.255.255.0     | 2001:33::33/64       | ens192 to ISP         |
| ISP             | 10.0.1.1/24 255.255.255.0     | 2001:33::1/64	       | ens192 ISP to CLI          |
|                 | 192.168.0.1/24 255.255.255.0     | 2001:11::1/64                   | ens224 ISP to HQ-R               |
|                 | 192.168.1.1/24 255.255.255.0     | 2001:22::1/64                   | ens256 ISP to BR-R              |
| HQ-R            | 192.168.0.2/24 255.255.255.0        | 2000:100::3f/122        | ens192 HQ-R to ISP           |
|                 | 10.0.0.1/26 255.255.255.0        |    2001:100::1/64		                | ens224 to HQ               |
| HQ-SRV          | 10.0.0.2/26 255.255.255.192        | 2000:100::1/122       | ens192 to HQ-R           |
| BR-R            | 192.168.1.2/24 255.255.255.0     | 2000:200::f/124       | ens33 BR-R to ISP           |
|                 | 10.0.2.1/28 255.255.255.240     | 2001:100::2/64	                   | ens34 to BR               |
| BR-SRV          | 10.0.2.2/28 255.255.255.240     | 2000:200::1/124       | ens192 to BR-R           |
| HQ-CLI          | 10.0.0.3/26 255.255.255.192        | 2000:100::5/122       |            |
| HQ-AD           | 10.0.0.4/26 255.255.255.192       | 2000:100::2/122       |            |  

**c.	Пул адресов для сети офиса BRANCH - не более 16**  

255.255.255.240 = /28  

**d.	Пул адресов для сети офиса HQ - не более 64**  

255.255.255.192 = /26  

## Настройка адресации  
**Назначаем адресацию согласно ранее заполненной таблицы №1**  

## **CLI**  
```
su -
toor
enter
nano /etc/net/ifaces/ens192/options
```  
Файл должен содержать строки, уканазанные ниже:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  

```
ctrl-x
y
enter
echo 10.0.1.2/24 > /etc/net/ifaces/ens192/ipv4address
systemctl restart network
ip -c a
```
#скрин ip -c a

## **ISP**  
```
su -
toor
enter
mkdir /etc/net/ifaces/ens192/
mkdir /etc/net/ifaces/ens224/
mkdir /etc/net/ifaces/ens256/
nano /etc/net/ifaces/ens192/options
```  
Файл должен содержать строки, уканазанные ниже:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
nano /etc/net/ifaces/ens224/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
ctrl-x
y
enter
echo 192.168.0.1/24 > /etc/net/ifaces/ens224/ipv4address
```
```
nano /etc/net/ifaces/ens256/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)   
```
ctrl-x
y
enter
echo 192.168.1.1/24 > /etc/net/ifaces/ens256/ipv4address
systemctl restart network
ip -c a
```
#скрин ip -c a

## **HQ-R**  
```
su -
toor
enter
mkdir /etc/net/ifaces/ens192/
mkdir /etc/net/ifaces/ens224/
nano /etc/net/ifaces/ens192/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)    
```
ctrl-x
y
enter
echo 192.168.0.2/24 > /etc/net/ifaces/ens192/ipv4address
```
```
nano /etc/net/ifaces/ens224/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
ctrl-x
y
enter
echo 	10.0.0.1/26 > /etc/net/ifaces/ens224/ipv4address
systemctl restart network
ip -c a
```
#скрин ip -c a

## **HQ-SRV**  
```
su -
toor
enter
nano /etc/net/ifaces/ens192/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
ctrl-x
y
enter
echo 	10.0.0.2/26 > /etc/net/ifaces/ens224/ipv4address
systemctl restart network
ip -c a
```
## **BR-R**  
```
su -
toor
enter
mkdir /etc/net/ifaces/ens33/
mkdir /etc/net/ifaces/ens34/
nano /etc/net/ifaces/ens33/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
ctrl-x
y
enter
echo 	192.168.1.2/24 > /etc/net/ifaces/ens33/ipv4address
nano /etc/net/ifaces/ens34/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
ctrl-x
y
enter
echo 	10.0.2.1/28 > /etc/net/ifaces/ens34/ipv4address
systemctl restart network
ip -c a
```
## **BR-SRV**  
```
su -
toor
enter
nano /etc/net/ifaces/ens192/options
```
Файл должен содержать строки, уканазанные ниже: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf735dff-a882-4adb-bf91-e6fa94bca30c)  
```
ctrl-x
y
enter
echo 	10.0.2.2/28 > /etc/net/ifaces/ens192/ipv4address
systemctl restart network
ip -c a
```
