# DEMO2024
DEMO exam 2024 for dummies  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9c79f3aa-70e2-4a41-8577-7d1bf4dfe20f)  

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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  


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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
```
nano /etc/net/ifaces/ens224/options
```
Файл должен содержать строки, уканазанные ниже:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
```
ctrl-x
y
enter
echo 192.168.1.1/24 > /etc/net/ifaces/ens256/ipv4address
```
Необходимо включить опцию forwarding:  
```
nano /etc/net/sysctl.conf
ctrl-x
y
enter
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c468da7e-93e8-4ab6-8512-bff6152f293e)  

```
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
```
ctrl-x
y
enter
echo 	10.0.0.1/26 > /etc/net/ifaces/ens224/ipv4address
```
Необходимо включить опцию forwarding:  
```
nano /etc/net/sysctl.conf
ctrl-x
y
enter
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c468da7e-93e8-4ab6-8512-bff6152f293e)  
```
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
```
ctrl-x
y
enter
echo 	192.168.1.2/24 > /etc/net/ifaces/ens33/ipv4address
nano /etc/net/ifaces/ens34/options
```
Файл должен содержать строки, уканазанные ниже:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
```
ctrl-x
y
enter
echo 	10.0.2.1/28 > /etc/net/ifaces/ens34/ipv4address
```
Необходимо включить опцию forwarding:  
```
nano /etc/net/sysctl.conf
ctrl-x
y
enter
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c468da7e-93e8-4ab6-8512-bff6152f293e)  
```
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80eaa6c9-695d-4baf-add1-99966066c74c)  
```
ctrl-x
y
enter
echo 	10.0.2.2/28 > /etc/net/ifaces/ens192/ipv4address
systemctl restart network
ip -c a
```

**2.	Настройте внутреннюю динамическую маршрутизацию по средствам FRR. Выберите и обоснуйте выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.**  
**Настройка FRR**  
Для настройки потребуется включённый forwarding, настройка выполнялась ранее.  

Предварительно настроем интерфейс туннеля:
## **HQ-R**
```
mkdir /etc/net/ifaces/tun1
nano /etc/net/ifaces/tun1/options
```
Файл должен содержать строки указанные ниже:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/70345715-5f78-4ce7-a2ec-d7856df39e48)  

```
echo 172.16.0.1/24 > /etc/net/ifaces/tun1/ipv4address
systemctl restart network
modprobe gre
```
## **BR-R**
```
mkdir /etc/net/ifaces/tun1
nano /etc/net/ifaces/tun1/options
```
Файл должен содержать строки указанные ниже:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/6fe1ed3b-207c-44ee-9a53-d8a0478bf9a6)  
```
echo 172.16.0.2/24 > /etc/net/ifaces/tun1/ipv4address
systemctl restart network
modprobe gre
```
Настройку динамическое маршрутизации производим с помощью протокола **OSPF** – потому что…  
## **HQ-R**

```
nano /etc/frr/daemons
меняем строчку
ospfd=no на строчку
ospfd=yes
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3e2c4d58-ea53-44a9-b222-7bf85951b9b4)  

```
ctrl-x
y
systemctl enable --now frr
vtysh
conf t
router ospf
passive-interface default
network 10.0.0.0/26 area 0
network 192.168.0.0/24 area 0
network 172.16.0.0/24 area 0
interface tun1
no ip ospf network broadcast
no ip ospf passive
exit
do write memory
show ip ospf neighbour
exit
```
В случае возникновения проблем с интерфейсами перезапустите службу frr
```
systemctl restart frr
```

## **BR-R**

```
nano /etc/frr/daemons
меняем строчку
ospfd=no на строчку
ospfd=yes
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3e2c4d58-ea53-44a9-b222-7bf85951b9b4)  

```
ctrl-x
y
systemctl enable --now frr
vtysh
conf t
router ospf
passive-interface default
network 10.0.2.0./28 area 0
network 192.168.1.0/24 area 0
network 172.16.0.0/24 area 0
exit
interface tun1
no ip ospf network broadcast
no ip ospf passive
exit
do write memory
show ip ospf neighbour
exit
```
В случае возникновения проблем с интерфейсами перезапустите службу frr
```
systemctl restart frr
```

**a.	Составьте топологию сети L3.**

**Схема топологии L3**  
![Топология сети L3 КП11](https://github.com/NyashMan/DEMO2023/assets/1348639/749e5337-6cfa-4e8f-987c-6af31fae1a42)  
**P.S.** спасибо sysahelper за рисунок!
#Дорисовать названия интерфейсов

**3.	Настройте автоматическое распределение IP-адресов на роутере HQ-R.**
**a.	Учтите, что у сервера должен быть зарезервирован адрес.**
## **HQ-R**
```
nano /etc/sysconfig/dhcpd
DHCPDARGS=ens224
ctrl-x
y
enter
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/16af1efa-2fb8-44ce-8e23-128430e6d46c)  
```
cp /etc/dhcp/dhcpd.conf{.example,}
nano /etc/dhcp/dhcpd.conf
```
После чистки файла, должно получиться следующее содержимое:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fbf1fe2d-3e0a-4a59-9560-9e6b401bd3f8)  

Проверяем файл на правильность заполнения. Обратите внимание, что файл заполнен в точности со скриншотом выше. (фигурные скобки в начале и конце секции, знаки ; и тд.)
```
dhcpd -t -cf /etc/dhcp/dhcpd.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/0760bb4c-0b85-4ce4-b769-f1bba9d01152)  

```
systemctl enable --now dhcpd
systemctl status dhcpd
journalctl -f -u dhcpd
```
## **HQ-SRV**
```
systemctl restart network
```
После проделанных манирпуляций HQ-SRV должен получить стиатический адрес.
## скрин с проверкой адреса на HQ-SRV

**4.	Настройте локальные учётные записи на всех устройствах в соответствии с таблицей 2.**  
**Таблица №2**  

| Логин | Пароль | Примечание |
| :---         |     :---:      |          ---: |
| Admin   | P@ssw0rd     | CLI HQ-SRV HQ-R    |
| Branch admin     | P@ssw0rd       | BR-SRV BR-R      |
| Network admin     | P@ssw0rd       | HQ-R BR-R BR-SRV      |

## **CLI**
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/f1abfc96-841c-4bb8-94d3-a6a991b260c0)  
Пароль: toor
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7ccf9221-f244-4b1c-b257-884ce25d27dc)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3b3b13a0-a3b0-4e5d-9104-ec74295a2dc0)
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7461c42a-578e-4618-9199-e3aed7e9088b)
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/674b3985-06f2-4a96-aff2-f645fcc1ac04)

## **BR-R**

```
useradd branch-admin -m -c "Branch admin" -U
passwd branch-admin
useradd network-admin -m -c "Network admin" -U
passwd network-admin
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/81e2dde7-a785-4d34-9b66-ddb563b8b4d8)  

**BR-SRV**
```
useradd branch-admin -m -c "Branch admin" -U
passwd branch-admin
useradd network-admin -m -c "Network admin" -U
passwd network-admin
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/81e2dde7-a785-4d34-9b66-ddb563b8b4d8)  

## **HQ-R**

```
useradd network-admin -m -c "Network admin" -U
passwd network-admin
useradd admin -m -c "Admin" -U
passwd admin
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/81e2dde7-a785-4d34-9b66-ddb563b8b4d8)

## **HQ-SRV**
```
useradd admin -m -c "Admin" -U
passwd admin
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/81e2dde7-a785-4d34-9b66-ddb563b8b4d8)  

**5.	Измерьте пропускную способность сети между двумя узлами HQ-R-ISP по средствам утилиты iperf 3. Предоставьте описание пропускной способности канала со скриншотами.**

## **HQ-R**
```
systemctl enable --now iperf3
iperf -c 192.168.0.1 -f m
iperf -c 192.168.0.1 -f m --get-server-output --logfile ~/iperf3_logfile.txt
```

## **ISP**

```
systemctl enable --now iperf3
iperf -s
```

По результату проверки сделать скриншот.  
Таким образом, пропускная способность канала между HQ-R -> ISP составляет 4.98 Гбит/с на отправку данных и 4.98 Гбит/с на получение данных. Также за 10 секунд тестирования было передано 5.80 ГБ данных.  

**6.	Составьте backup скрипты для сохранения конфигурации сетевых устройств, а именно HQ-R BR-R. Продемонстрируйте их работу.**  

## **HQ-R**

```
nano backup-script.sh
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5cb84998-f6b5-4951-8d2c-095aa8e7e96d)  
```
ctrl-x
y
enter
chmod +x backup-script.sh
./backup-script.sh
tar -tf /opt/backup/hq-r-06.01.24.tgz | less
```

## **BR-R**

```
nano backup-script.sh
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5cb84998-f6b5-4951-8d2c-095aa8e7e96d)  
```
ctrl-x
y
enter
chmod +x backup-script.sh
./backup-script.sh
tar -tf /opt/backup/hq-r-06.01.24.tgz | less
```

**7.	Настройте подключение по SSH для удалённого конфигурирования устройства HQ-SRV по порту 2222. Учтите, что вам необходимо перенаправить трафик на этот порт посредством контролирования трафика.**
## **HQ-SRV**

```
nano /etc/openssh/sshd_config
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d2036a2d-11ac-4eb3-a7ae-17b6633ac0ae)  
```
ctrl-x
y
enter
systemctl restart sshd
```
## **HQ-R**
```
systemctl enable --now firewalld
firewall-cmd --permanent --zone=public --add-interface=ens192
firewall-cmd --permanent --zone=trusted --add-interface=ens224
firewall-cmd --permanent --zone=public --add-forward-port=port=22:proto=tcp:toport=2222:toaddr=10.0.0.2
firewall-cmd --reload
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9d635161-5978-4102-9b48-926bc4b070b2)  
###скрин заменить!
Выполняем проверку подключения:  
##скриншот с проверкой подключения  

**8.	Настройте контроль доступа до HQ-SRV по SSH со всех устройств, кроме CLI.**


### Модуль 2: Организация сетевого администрирования
## **Задание модуля 2**

**1. Настройте DNS-сервер на сервере HQ-SRV:**  
**a. На DNS сервере необходимо настроить 2 зоны**

**Зона hq.work**  
| Имя | Тип записи | Адрес |
| :---         |     :---:      |          ---: |
| hq-r.hq.work   | A, PTR     | IP-адрес    |
| hq-srv.hq.work   | A, PTR     | IP-адрес    |  

**Зона branch.work**  
| Имя | Тип записи | Адрес |
| :---         |     :---:      |          ---: |
| br-r.branch.work   | A, PTR     | IP-адрес    |
| br-srv.branch.work   | A     | IP-адрес    |  

