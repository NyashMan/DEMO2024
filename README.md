# DEMO2024
DEMO exam 2024 for dummies  
<p align="center">
 <img src="https://github.com/NyashMan/DEMO2024/assets/1348639/f1c404ad-a564-4ea7-b407-34dc01c9c626" />
</p>  


## [Снимки ОС](https://drive.google.com/file/d/19Ox3j-zuUDHDvdscnGtJPKGIzQmyxyoi/view?usp=drive_link)  
Для удобства список LAN Segmet, который необходимо создать при развёртывании образов в VMWare WorkStation. 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fb6c00bc-76c2-4b0a-ab3e-d2778fa4df46)  
По умолчанию - все машины имеют интерфейс ens33 (NAT), для доступа к сети Интернет. При необходимости их можно отключить (убрать галочку с пункта "Connect at power on")
## [Задание](https://sysahelper.ru/mod/resource/view.php?id=14)  

## Описание задания   
Образец задания для демонстрационного экзамена по комплекту оценочной документации.  

### Модуль 1: Выполнение работ по проектированию сетевой инфраструктуры  
## **Задание 1 модуля 1** 
**1.	Выполните базовую настройку всех устройств:**  
**a.	Присвоить имена в соответствии с топологией:**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/88f09775-e51c-4113-bb6a-1336c200bf7b)  

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
| CLI             | 10.0.1.2/24 255.255.255.0     | 2001:33::33/64       | 192 to ISP         |
| ISP             | 10.0.1.1/24 255.255.255.0     | 2001:33::1/64	       | ens192 ISP to CLI          |
|                 | 192.168.0.1/24 255.255.255.0     | 2001:11::1/64                   | ens224 ISP to HQ-R               |
|                 | 192.168.1.1/24 255.255.255.0     | 2001:22::1/64                   | ens256 ISP to BR-R              |
| HQ-R            | 192.168.0.2/24 255.255.255.0        | 2000:100::3f/122        | ens192 HQ-R to ISP           |
|                 | 10.0.0.1/26 255.255.255.0        |    2001:100::1/64		                | ens224 to HQ               |
| HQ-SRV          | 10.0.0.2/26 255.255.255.192        | 2000:100::1/122       | ens192 to HQ-R           |
| BR-R            | 192.168.1.2/24 255.255.255.0     | 2000:200::f/124       | ens160 BR-R to ISP           |
|                 | 10.0.2.1/28 255.255.255.240     | 2001:100::2/64	                   | ens192 to BR               |
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2a47b1bb-fd52-4d8a-9477-d00cc2edecce)  

```
ip -c a
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c42a6bb6-4742-47fc-8e04-c50d79d34e0b)  

## **ISP**  
```
su -
toor
enter
nmtui
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/6fd69321-b8af-4fac-8af3-ce3d725c3c79)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2c688d45-2e2e-465b-b248-3bbdd35085b7)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/df296352-40b6-4b49-a803-36918ac6073a)  
После установки ip-адресов необходимо переподключить интерфейсы.  

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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9c28a552-a15a-4fb8-9b22-339a5cfd0d5d)   

## **HQ-R**  
```
su -
toor
enter
nmtui
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/563fb426-a5ae-43ad-895a-83c6cf563845)
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d6547cfc-0796-45f1-bae9-aacde55d9628)

```
ctrl-x
y
enter
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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9df4565f-7e66-46cd-a0f3-a279beac373f)

## **HQ-SRV**  
В дальнейшем на HQ-SRV подразумевается получение адреса по DHCP от HQ-R.  
Удостоверимся, что на интерфейсе установлено получение адресов через DHCP.
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7d906ddf-918d-4d48-b246-b1d8bfd5803e)  

## **BR-R**  
```
su -
toor
enter
nmtui
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/ec24eecb-51fe-464c-a8ff-0445fdabb004)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/13fc4ab3-2428-484a-985d-a267cf38c143)  


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
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/0f3c5a5c-81c4-4372-8c36-74a4ab16e2eb)  

## **BR-SRV**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/ed430966-cfd6-418d-beb6-0379cf3519a9)

```
ip -c a
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/12b60453-c385-4020-a655-fde5f906b65a)

**2.	Настройте внутреннюю динамическую маршрутизацию по средствам FRR. Выберите и обоснуйте выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.**  
**Настройка FRR**  
Для настройки потребуется включённый forwarding, настройка выполнялась ранее.  

Предварительно настроем интерфейс туннеля:
## **HQ-R**
```
nmtui
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/62a54525-cff5-46fb-9c46-1d4f0f9a1499)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/4df2a006-fa62-4cf3-aa34-2c7685c470a5)  

## **BR-R**
```
nmtui
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/62a54525-cff5-46fb-9c46-1d4f0f9a1499)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/914a562f-36cd-4bab-989e-97f2db457044)  

Настройку динамическое маршрутизации производим с помощью протокола **OSPF** – Данный протокол динамической сети позволяет разделять сеть на логические области, что делает его масштабируемым для больших сетей.  
Каждая область может иметь свою таблицу маршрутизации, что уменьшает нагрузку на маршрутизаторы и улучшает производительность сети.  
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
network 172.16.0.0/24 area 0
exit
interface tun1
no ip ospf network broadcast
no ip ospf passive
exit
do write memory
exit
```
Зададим TTL для OSPF  
```
nmcli connection edit tun1
set ip-tunnel.ttl 64
save
quit
```

Временно выключаем сервис службы **firewalld**  
```
systemctl stop firewalld.service
systemctl disable --now firewalld.service
```
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
network 10.0.2.0/28 area 0
network 172.16.0.0/24 area 0
exit
interface tun1
no ip ospf network broadcast
no ip ospf passive
exit
do write memory
exit
```
Зададим TTL для OSPF  
```
nmcli connection edit tun1
set ip-tunnel.ttl 64
save
quit
```
Проверим работу OSPF:  
```
show ip ospf neighbor
exit
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c9b9072d-3d7d-4949-8541-cb45601ccb61)
P.S. в случае, если OSPF не заработал, можно перезапустить машины **ISP** **BR-R** **HQ-R**  

**a.	Составьте топологию сети L3.**  

**Схема топологии L3**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9ad4ac5a-68a1-4fcd-ad5d-697776518faa)  
 
**P.S.** спасибо sysahelper за рисунок!  

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
nano /etc/dhcp/dhcpd.conf
```
заполняем файл:  
![305547257-105a539b-7369-47f8-9d85-60958c5916a0](https://github.com/NyashMan/DEMO2024/assets/1348639/612c2dc2-5541-4c2a-8275-81b574e02061)  

Проверяем файл на правильность заполнения. Обратите внимание, что файл заполнен в точности со скриншотом выше. (фигурные скобки в начале и конце секции, знаки **;** и тд.)
```
dhcpd -t -cf /etc/dhcp/dhcpd.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/77444d5e-534b-42d9-9b55-e976e9ce13ff)   

```
systemctl enable --now dhcpd
systemctl status dhcpd
journalctl -f -u dhcpd
```

## **HQ-SRV**
```
systemctl restart network
```
После проделанных манирпуляций HQ-SRV должен получить статический адрес.
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/84c4ba59-3f4b-442b-a86f-bb7911b963a0)  

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
## **ISP**

```
systemctl enable --now iperf3
```
## **HQ-R**
```
systemctl enable --now iperf3
iperf3 -c 192.168.0.1 -f m --get-server-output
```

По результату проверки сделать скриншот.  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/f972d469-1875-4666-9a10-f5d9ce647f5b)  

**6.	Составьте backup скрипты для сохранения конфигурации сетевых устройств, а именно HQ-R BR-R. Продемонстрируйте их работу.**  

## **HQ-R**
Cоздадим дирректорию для сохранения файла:  
```
mkdir /opt/backup/
```
Запишем скрипт:  
```
nano backup-script.sh
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5cb84998-f6b5-4951-8d2c-095aa8e7e96d)  
```
ctrl-x
y
жмём enter
chmod +x backup-script.sh
./backup-script.sh
```
Проверим содержимое файла бэкапа. Обратите внимание, что название архива варьируется, в зависимости от даты, когда была сделана резервная копия! Будьте внимательны.
```
tar -tf /opt/backup/hq-r-06.01.24.tgz | less
```

## **BR-R**
Cоздадим дирректорию для сохранения файла:  
```
mkdir /opt/backup/
```
Запишем скрипт: 
```
nano backup-script.sh
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5cb84998-f6b5-4951-8d2c-095aa8e7e96d)  
```
ctrl-x
y
жмём enter
chmod +x backup-script.sh
./backup-script.sh
```
Проверим содержимое файла бэкапа. Обратите внимание, что название архива варьируется, в зависимости от даты, когда была сделана резервная копия! Будьте внимательны.
```
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
nft add table inet nat
nft add chain inet nat prerouting '{ type nat hook prerouting priority 0; }'
nft add rule inet nat prerouting ip daddr 192.168.0.2 tcp dport 22 dnat to 10.0.0.2:2222
nft list ruleset | tail -n 7 | tee -a /etc/nftables/nftables.nft
systemctl enable --now nftables
systemctl restart nftables
nft list ruleset
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/4bec0562-f600-4c8f-9a14-c67fa686edc4)  

Выполняем проверку подключения:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d63221e1-a13a-44aa-8add-908d7bcd3f47)  

**8.	Настройте контроль доступа до HQ-SRV по SSH со всех устройств, кроме CLI.**

## **HQ-SRV**

```
systemctl enable --now nftables.service
nft add rule inet filter input ip saddr 10.0.1.2 tcp dport 2222 counter drop
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/6eeaf5e0-22d3-4342-89ec-ec5fd4a7fcbd)  

```
nano /etc/nftables/nftables.nft
```
Удаляем всё содержимое файла до закоментированных строк:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2921301e-e2d5-464f-ad03-fc2eb33b9043)  

```
nft list ruleset | tee -a /etc/nftables/nftables.nft
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/718a8fb8-eb1a-4874-8d7c-76a39fbe9cc2)  

```
systemctl restart nftables
nft list ruleset
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e1373d1d-bf3d-40fb-8aa0-2c8625ea1090)  

Выполняем проверку:  
## **CLI**
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/735c0cdd-a2f7-4186-b2cd-91e5310724f1)  
В результате настройки, соединение с сервером **HQ-SRV** по ssh установить не удастся.  

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

## **HQ-SRV**  

```
nano /etc/bind/options.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d903e095-bbe6-4050-b473-a475d2fd5d46)  

```
systemctl enable --now bind
nano /etc/bind/local.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/754ad3e6-64da-4fa4-ad12-22e05b21a960)  
```
cp /etc/bind/zone/{localdomain,hq.db}
cp /etc/bind/zone/{localdomain,branch.db}
cp /etc/bind/zone/{127.in-addr.arpa,0.db}
cp /etc/bind/zone/{127.in-addr.arpa,2.db}
chown root:named /etc/bind/zone/{hq,branch,0,2}.db
nano /etc/bind/zone/hq.db
```
Обратите внимание на синтаксис. Проверьте точки в конце наименований зон!
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e81f251c-9987-4d8d-ad47-e4ae50d99e1d)  
```
nano /etc/bind/zone/branch.db
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b2740789-a372-40f4-8558-b12afeb85d78)  
```
nano /etc/bind/zone/0.db
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b4239e96-7018-4286-8861-ea53aa074e85)  
```
/etc/bind/zone/2.db
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/44051f89-75a5-44ac-a5b5-d83d9c4f8afa)  
```
systemctl restart bind
```
Выполняем проверку:  

![image](https://github.com/NyashMan/DEMO2024/assets/1348639/030310e9-e147-4465-8266-f16a628c6152)  

Если настройка была выполнена верно, то вы увидети прямые и обратные зоны DNS.  

**2. Настройте синхронизацию времени между сетевыми устройствами по протоколу NTP**

**a. В качестве сервера должен выступать роутер HQ-R со стратумом 5**
**b. Используйте Loopback интерфейс на HQ-R, как источник сервера времени**
**c. Все остальные устройства и сервера должны синхронизировать свое время с роутером HQ-R**
**d. Все устройства и сервера настроены на московский часовой пояс (UTC +3)**

## **HQ-R**  
Настраиваем сервер времени с параметрами необходимыми в задании:  
```
nano /etc/chrony.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/88686ceb-da29-4e4e-83bb-24fcfa1e206b)  
Перезагружаем машину  
Проверяем:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b95120d5-61b4-4832-a057-746bba721b01)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/6b268d7e-13c9-42a9-8da4-7c5a06a9f635)  
Далее производим настройку клиентов (настройка на всех машинах идентична):  
## **HQ-SRV**  

```
nano /etc/chrony.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/ee598d6b-181c-430a-b2db-f72025c76c0a)  
```
systemctl enable --now chronyd
```
Перезагружаем машину  
Проверяем:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/1a3b0be2-3919-49ef-8ee7-97cf1987dfaf)  



## **CLI**  
```
nano /etc/chrony.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/dc931139-5cbf-4bf8-92d1-c6e23f4e2156)  

```
systemctl enable --now chronyd
```
Перезагружаем машину  

## **BR-R**  
# доставить пакет chrony!
```
nano /etc/chrony.conf
```
# **скрин**
```
systemctl enable --now chronyd
```
Перезагружаем машину  

## **BR-SRV**  
# доставить пакет chrony!
```
nano /etc/chrony.conf
```
# **скрин**
```
systemctl enable --now chronyd
```
Перезагружаем машину  
Выполняем проверку:  

## **HQ-R**  
```
chronyc clients
```
В таблице должны появиться все клиенты, которые синхронизируют время с сервером.  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/214d79a8-9474-42fb-a2d1-7448014f1a24)  

**3. Настройте сервер домена выбор, его типа обоснуйте, на базе HQ-SRV через web интерфейс, выбор технологий обоснуйте**  
**a. Введите машины BR-SRV и CLI в данный домен**  
**b. Организуйте отслеживание подключения к домену**  

## **ISP**
Произведём настройку маршрута для CLI
```
ip route add default via 192.168.0.2
```
## **HQ-SRV**
Произведём временное отключение интерфейсов. Обязательно перед началом настройки samba! 
```
nmtui
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/bd3570e1-b9be-4a5f-bf5a-887cf27100cd)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e807a623-e6be-4573-92b8-ca5a8227a0c2)  
```
control bind-chroot disabled
grep -q 'bind-dns' /etc/bind/named.conf || echo 'include "/var/lib/samba/bind-dns/named.conf";' >> /etc/bind/named.conf
nano /etc/bind/options.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/52f85f0f-c239-430c-89ae-db65267e00b5)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fe226eca-2c18-4af4-8547-a4ea54a27b5c)  
```
systemctl stop bind
nano /etc/sysconfig/network
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d18b6f9c-ab19-4f81-b4ea-91b161f99be8)  
```
hostnamectl set-hostname hq-srv.demo.first;exec bash
domainname demo.first
rm -f /etc/samba/smb.conf
rm -rf /var/lib/samba
rm -rf /var/cache/samba
mkdir -p /var/lib/samba/sysvol
samba-tool domain provision
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/eeb41185-579d-410f-9755-be4f2b6f7970)  

```
systemctl enable --now samba
systemctl enable --now bind
cp /var/lib/samba/private/krb5.conf /etc/krb5.conf
samba-tool domain info 127.0.0.1
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3d30b5b2-144b-4347-828a-86ffdcb56759)  
```
host -t SRV _kerberos._udp.demo.first.
host -t SRV _ldap._tcp.demo.first.
host -t A hq-srv.demo.first
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a5ac8e81-d2c5-4bc8-aa99-ee6613652bf5)  
```
kinit administrator@DEMO.FIRST
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/ab190c07-ce7f-4ed8-80c7-7fe7cc69955c)  

Вводим машины в домен:  
## **CLI**
Указываем DNS сервер домена  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5a770be8-4b58-4a5e-9a0b-f0c2c5065ed2)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cf042173-7b61-45f4-805a-41748b9bbc6e)  
Отключаем и включаем сетевое соединение  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/221af92f-31cf-4443-90dd-31e48613a900)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a1ddcd99-449f-4ee3-9fb1-a14dba15cd0a)  
```
acc
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/defc901c-27ac-418e-9198-6af22620939e)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5d753e57-d2ab-433b-8f22-a48df6d62449)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/759f6dee-63c7-4a0e-819b-f297e51349f4)  
```
reboot
```
Авторизируемся под учётной записью administrator  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/8dd05567-f23c-479f-83b3-2a763d4b3e0c)  
Пароль: P@ssw0rd   
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/0da696e1-bfd2-4014-966e-9b7df6eaa484)  

## **BR-SRV**
Указываем DNS сервер домена  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5a770be8-4b58-4a5e-9a0b-f0c2c5065ed2)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d46862d7-3ce5-4ee2-9753-7221822ae075)  
Отключаем и включаем сетевое соединение  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/221af92f-31cf-4443-90dd-31e48613a900)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a1ddcd99-449f-4ee3-9fb1-a14dba15cd0a)  
```
acc
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/defc901c-27ac-418e-9198-6af22620939e)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/f1a96ed4-f52e-46e7-b3ff-6952c6093d62)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7f034e5c-9c52-4db9-ae51-dabd30f9a366)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/1d146328-0b59-49f3-a952-c5d88ccaea95)  

```
reboot
```
Авторизируемся под учётной записью administrator  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a486a543-ae64-4c3d-86b7-af40d0e0ec38)  
Пароль: P@ssw0rd  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/44323daf-1640-4067-9b9c-8226aa18f666)  


## **CLI**
Необходимо зайти обратно под пользователем user
```
kinit administrator
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/bfffad87-29d0-4a4c-b8ae-2c938764e237)  
## установить пакет admc
```
admc
```
Необходимо создать доменных пользователей из таблицы (они не конфликтуют с теми, что были созданы ранее), они необходимы для настройки доступа к сетевым хранилищам.  

| Логин | Пароль | 
| :---         |     ---:      | 
| Admin   | P@ssw0rd     | 
| Branch admin     | P@ssw0rd       | 
| Network admin     | P@ssw0rd       | 

![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fad947e9-f101-454b-91f4-c9411b220172)  
Создаём группы для только что созданных пользователей:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/49402392-a1ff-4428-a264-497e0e8cdc2f)  





**4. Реализуйте файловый SMB или NFS (выбор обоснуйте) сервер на базе сервера HQ-SRV.**  

**a. Должны быть опубликованы общие папки по названиям:**  
**i. Branch_Files - только для пользователя Branch admin;**  
**ii. Network - только для пользователя Network admin;**  
**iii. Admin_Files - только для пользователя Admin;**  
**b. Каждая папка должна монтироваться на всех серверах в папку /mnt/ (например, /mnt/All_files) автоматически при входе доменного пользователя в систему и отключаться при его выходе из сессии. Монтироваться должны только доступные пользователю каталоги**  

**5. Сконфигурируйте веб-сервер LMS Apache на сервере BR-SRV:**  

**a. На главной странице должен отражаться номер места**  
**b. Используйте базу данных mySQL**  
**c. Создайте пользователей в соответствии с таблицей**  

| Пользователь | Группа | 
| :---         |     ---:      | 
| Admin   | Admin     | 
| Manager1     | Manager       | 
| Manager2     | Manager       | 
| Manager3     | Manager       |
| User1     | WS       |
| User2     | WS       |
| User3     | WS       |
| User4     | WS       |
| User5     | TEAM       |
| User6     | TEAM       |
| User7     | TEAM       |  

## **BR-SRV**
```
systemctl enable --now httpd2.service
```
Проверяем доступность развёрнутого портала по адресу http://moodle.localhost/install.php
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/98c858ee-21ba-44de-a1b5-773dd51c381f)  

Настроим доступ по имени на клиенте:
## **CLI**
```
echo 10.0.2.2 moodle.demo.first moodle >> /etc/hosts
```
Проверяем:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3c386270-ec05-4569-a9cb-75420348db7d)  

Производим установку: 
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/10c8e350-c9d1-44ce-a067-417a83da062d)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d7a7dd69-934c-4c61-90b6-6c9433b97063)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/ed9fa557-ca17-4d5e-b7a1-ba6abe03d3f6)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a9059078-920d-4d5d-a864-191e71407b6a)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/435487cc-f7bc-4e75-af86-363bce94c161)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a5c0ad7c-7087-4f26-8076-626266f4f341)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a4291eac-c41a-4086-bbb0-ba2ee3e2e64d)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3517aa0a-b74b-4ae4-8497-e4059bd207da)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/6cf17575-c680-48e0-9efa-9c63712e504e)  
Создаём группы для пользователей заданных в таблице:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9cb5e81a-05e1-4df6-9872-64411c725070)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/be52d171-3065-4542-8995-c67299184cc1)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e1149176-aa8d-4bdb-bc02-1ff0f4014722)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/92c2fc9e-ceb3-4b9e-ad5f-b703e29089b0)  
По аналогии создаём оставшиеся группы  
Проверяем:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7180999f-9d31-4987-9a45-fed431832c57)  
Создаём пользователей:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/66529690-2e64-4a03-ae3a-2c69f4d1eff4)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/006d7ef5-6d7b-4f11-823e-c243d5531ce3)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/237d3f1b-7e20-4c45-98a4-d6ad84ab5dbd)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/1194c06c-d138-4496-8ab1-7c98b9dc230d)  
По аналогии создаём оставшихся пользователей  
Проверяем:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/abc9045b-d7af-4648-8bf0-36330bdda176)  
Добавляем пользователей в соответствующие группы:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2061b9cd-8403-4334-b7f1-42eaa98f4861)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a24eadca-7270-44cd-87f9-5a49d7ad72e8)  
По аналогии добавляем оставшихся пользователей в группы  
Проверяем:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/00b648c5-7d9f-46eb-ba90-8d31299f13fc)  
























**6. Запустите сервис MediaWiki используя docker на сервере HQ-SRV.**  

**a. Установите Docker и Docker Compose.**  
**b. Создайте в домашней директории пользователя файл wiki.yml для приложения MediaWiki:**  
**i. Средствами docker compose должен создаваться стек контейнеров с приложением MediaWiki и базой данных**  
**ii. Используйте два сервиса;**
**iii. Основной контейнер MediaWiki должен называться wiki и использовать образ mediawiki;**  
**iv. Файл LocalSettings.php с корректными настройками должен находиться в домашней папке пользователя и автоматически монтироваться в образ;**  
**v. Контейнер с базой данных должен называться db и использовать образ mysql;**  
**vi. Он должен создавать базу с названием mediawiki, доступную по стандартному порту, для пользователя wiki с паролем DEP@ssw0rd;**  
**vii. База должна храниться в отдельном volume с названием dbvolume.**  
**MediaWiki должна быть доступна извне через порт 8080.**  

## **HQ-SRV**

Необходимо выключить сервис alterator
```
systemctl disable --now ahttpd
systemctl disable --now alteratord
```
Проверяем содержимое файла:
```
nano ~/wiki.yml
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b01ce929-61b7-476e-bb53-1986727648dc)  

Перезагружаем сервисы docker:  
```
systemctl restart docker
docker start db
docker start wiki
docker ps
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/1fe949ae-8b76-4255-a73a-74e369d56152)  
Проверяем доступность ресурса через браузер:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/38eea153-89bb-4690-a4e1-1cce7257cb39)  

Настроим доступность ресурса извне:  
## **CLI**

```
echo 10.0.0.2 mediawiki.demo.first mediawiki >> /etc/hosts
```
Проверяем доступность:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/d821eac7-f394-4702-816d-2917aacefa61)  
Производим настройку: 
## **HQ-SRV**
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/21e1e0e5-4c3f-4976-a586-47f4d7c9fe08)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5bc4f9bc-1bf0-4b87-98f5-0144c5d8993d)   
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7a56c424-d208-4b8e-a08f-9eed876f0b5f)  
Пароль от БД: DEP@ssw0rd  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a2d00579-a8f3-4bdf-aa26-7a32fe21900c)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7cd2e4c7-e435-4c51-abe8-97519f8f9bd9)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2b5c2ed9-9fc0-4df4-9233-8514deecf98e)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/34986562-91b2-4b6f-8baa-2ae82f69981f)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fb8bf074-00f5-4cf0-817c-ca4a2c7fa344)    
После настройки, необходимо скачать файл LocalLocalSettings.php  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/6c6fb61f-8684-4279-bc39-768e0e06676d)  
Переместите скачанный файл в директорию, в которой находится файл wiki.yml  
```
su -
cp путь до файла LocalLocalSettings.php ~/
nano ~/wiki.yml
```
```
docker-compose -f wiki.yml stop
docker-compose -f wiki.yml up -d
```
Проверяем доступ к http://mediawiki.demo.first:8080:
## **CLI**
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/1ee2b717-a8db-480a-bbd5-19861bc35efc)  
вход осуществляем из под пользователя **wiki** с паролем **DEP@ssw0rd** :
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b1a99892-5e4d-4ab0-95b2-cf80523b5850)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/5db17453-6a94-4244-8a4d-b29e3dcfe2f7)  

# **Under Construction**
_______________________________________________________________________________________________________________________
### Модуль 3: Эксплуатация объектов сетевой инфраструктуры
## **Задание модуля 3:**

**1. Реализуйте мониторинг по средствам rsyslog на всех Linux хостах.**
**a. Составьте отчёт о том, как работает мониторинг**

## **BR-SRV**

```
/etc/rsyslog.d/00_common.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/285ecb22-fee3-4187-b892-9f0ee7805f29)  
```
systemctl enable --now rsyslog
```
## **BR-R**

```
echo "*.* @@10.0.2.2:514" > /etc/rsyslog.d/all_log.conf
systemctl enable --now rsyslog
```
Проверим работоспособность:  
## **BR-SRV**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/3003d608-6538-4fce-8405-7edb6da15505)

Отправляем тестовый лог:  

## **BR-R**
```
logger -p 'error' 'Test'
```
Проверяем его:  
## **BR-SRV**  
**2. Выполните настройку центра сертификации на базе HQ-SRV:**  
**a. Выдайте сертификаты для SSH;**  
**b. Выдайте сертификаты для веб серверов;**  

**3. Настройте SSH на всех Linux хостах:**  
**a. Banner ( Authorized access only! );**   
**b. Установите запрет на доступ root;**   
**c. Отключите аутентификацию по паролю;**   
**d. Переведите на нестандартный порт;**   
**e. Ограничьте ввод попыток до 4;**   
**f. Отключите пустые пароли;**   
**g. Установите предел времени аутентификации до 5 минут;**  
**h. Установите авторизацию по сертификату выданным HQ-SRV**   

Т.к. настройка всех параметров идентична для всех устройств пример будет показан на **HQ-SRV**. 
Все настройки заключаются в изменении конфигурационного файла **sshd_config**. Для удобства выполнения пользуйтесь поиском по файлу (ctrl+w)  
**a. Banner ( Authorized access only! );**  
```
nano /etc/openssh/banner
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/12507949-634f-4855-ba51-b974393d53e6)  
```
nano /etc/openssh/sshd_config
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7f8f99e3-dcd7-460a-b097-c11f55461a66)  
**b. Установите запрет на доступ root;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/bfafff53-62de-42a6-bd0f-61d6fc431ec1)  
**c. Отключите аутентификацию по паролю;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/884b7397-0c7b-40ff-800c-14ab8bfd77b7)  
**d. Переведите на нестандартный порт;**   
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fa433de7-989c-4276-a5ef-bb9d70c0bb9b)  
**e. Ограничьте ввод попыток до 4;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9a941af5-e7c0-498f-ab40-b9dd304b6c0b)  
**f. Отключите пустые пароли;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80a71064-3e7f-4b9b-a006-596d4d931449)  
**g. Установите предел времени аутентификации до 5 минут;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a0ec9ef0-8481-4602-b8ac-217e315c3b9a)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e311bd90-1427-456f-ae6f-835d3ba74348)  
**h. Установите авторизацию по сертификату выданным HQ-SRV**   
## расписать пункт

6. Настройте виртуальный принтер с помощью **CUPS** для возможности печати документов из Linux-системы на сервере BR-SRV.
Предположим, что печать должна осуществляться с устройства **CLI**
## **BR-SRV**
Открываем в браузере **http://localhost:631/admin** :
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2e5308fe-e651-4ff1-a6bb-15bbea4cbc33)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c4100fc6-0fad-45e4-8a63-72bbe4272163)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/540dce0c-8826-4085-8584-bcc181d5793e)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/095fa6bb-2e41-4c8d-bccc-92c110bcf0e8)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/13de7d6d-efdc-41ec-8ba8-fb14ee7f15d0)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/40c189bb-7d65-4d63-8058-e815604dbbd7)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/71e2ca26-5441-425d-8172-f7997eac6e0c)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/8741d74e-07c5-4342-b570-ef671f3c1fb7)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/4352d232-3669-4eea-b23f-dfbb623cf050)  

## **CLI**
Добавляем принтер:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b439af24-a068-4dc8-a4af-184e13cf51ce)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e6ddb986-f59a-4e65-8668-c4252f0c0604)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9c9dd491-a171-46d3-9098-68cd43e356e7)  
После поиска появится окно аутентификации, игнорируем его.
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/40274e25-60c1-4dc4-a084-bedc97d1e8de)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cb9e9e41-98d4-4f1f-9a67-7fe77e1eba62)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7c530dac-46c4-4d4a-a286-79ca943cfed5)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/45f7d6a2-9cd6-4728-9587-4af2af385b0f)  

Проверяем результат:  
## **BR-SRV**
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/58b11fc3-e81a-4fff-982e-1fc83ab1697f)  


**9. Настройте программный RAID 5 из дисков по 1 Гб, которые подключены к машине BR-SRV.**

## **BR-SRV**

```
lsblk
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2e20c3f2-2ece-4229-832e-a288622b8ede)  

```
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd
watch cat /proc/mdstat
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/777719db-054f-4c0d-b444-94fc894ae1cc)  

```
mkfs.ext4 /dev/md0
mkdir /mnt/raid5
mount /dev/md0 /mnt/raid5
nano /etc/fstab
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c4fcf214-b4ef-467f-8523-808804b407d1)  

Проверяем работоспособность:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7875afe5-027e-46ca-aaa4-85ed688f0528)  

### Модуль 4: Организация сетевого администрирования 
## **Задание модуля 4** 
**a. Установите и настройте сервер мониторинга на базе машины
CLI с использованием БД PostgreSQL. Выбор технологии
решения обоснуйте.**


