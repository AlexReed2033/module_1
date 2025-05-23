# module_1

## Описание задания   
Образец задания для демонстрационного экзамена по комплекту оценочной документации.  

### Модуль 1: Выполнение работ по проектированию сетевой инфраструктуры  
## **Задание 1 модуля 1** 
**1.	Выполните базовую настройку всех устройств:**  
**a.	Присвоить имена в соответствии с топологией:**  
![image](304349393-88f09775-e51c-4113-bb6a-1336c200bf7b.png)  

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

```
ip -c a
```
![image](305539916-c42a6bb6-4742-47fc-8e04-c50d79d34e0b.png)  

## **ISP**  
```
su -
toor
enter
nmtui
```  
![image](305506919-6fd69321-b8af-4fac-8af3-ce3d725c3c79.png)  
![image](305507596-2c688d45-2e2e-465b-b248-3bbdd35085b7.png)  
После установки ip-адресов необходимо переподключить интерфейсы.  

Произведём настройку маршрута для CLI
```
ip route add default via 192.168.0.2
```

Необходимо включить опцию forwarding:  
```
nano /etc/net/sysctl.conf
ctrl-x
y
enter
```
![image](299778742-c468da7e-93e8-4ab6-8512-bff6152f293e.png)  

```
systemctl restart network
ip -c a
```
![image](305508852-9c28a552-a15a-4fb8-9b22-339a5cfd0d5d.png)   

## **HQ-R**  
```
su -
toor
enter
nmtui
```
![image](305511468-563fb426-a5ae-43ad-895a-83c6cf563845.png)
![image](305512031-d6547cfc-0796-45f1-bae9-aacde55d9628.png)

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
![image](299778742-c468da7e-93e8-4ab6-8512-bff6152f293e.png)  
```
systemctl restart network
ip -c a
```
![image](299931649-9df4565f-7e66-46cd-a0f3-a279beac373f.png)

## **HQ-SRV**  
В дальнейшем на HQ-SRV подразумевается получение адреса по DHCP от HQ-R.  
Удостоверимся, что на интерфейсе установлено получение адресов через DHCP.

## **BR-R**  
```
su -
toor
enter
nmtui
```
![image](305515712-ec24eecb-51fe-464c-a8ff-0445fdabb004.png)  
![image](305514755-13fc4ab3-2428-484a-985d-a267cf38c143.png)  


Необходимо включить опцию forwarding:  
```
nano /etc/net/sysctl.conf
ctrl-x
y
enter
systemctl restart network
ip -c a
```
![image](305516706-0f3c5a5c-81c4-4372-8c36-74a4ab16e2eb.png)  

## **BR-SRV**  

```
ip -c a
```
![image](299936811-12b60453-c385-4020-a655-fde5f906b65a.png)

**2.	Настройте внутреннюю динамическую маршрутизацию по средствам FRR. Выберите и обоснуйте выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.**  
**Настройка FRR**  
Для настройки потребуется включённый forwarding, настройка выполнялась ранее.  

Предварительно настроем интерфейс туннеля:
## **HQ-R**
```
nmtui
``` 
![image](305524897-4df2a006-fa62-4cf3-aa34-2c7685c470a5.png)  

## **BR-R**
```
nmtui
``` 
![image](305525453-914a562f-36cd-4bab-989e-97f2db457044.png)  
 
## **HQ-R**

```
nano /etc/frr/daemons
меняем строчку
ospfd=no на строчку
ospfd=yes
```
![image](299781525-3e2c4d58-ea53-44a9-b222-7bf85951b9b4.png)  

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
![image](299781525-3e2c4d58-ea53-44a9-b222-7bf85951b9b4.png)  

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
Временно выключаем сервис службы **firewalld**  
```
systemctl stop firewalld.service
systemctl disable --now firewalld.service
```
```
systemctl restart frr
```
Проверим работу OSPF:  
```
show ip ospf neighbor
exit
```

**a.	Составьте топологию сети L3.**  

**Схема топологии L3**  
![image](306596174-9ad4ac5a-68a1-4fcd-ad5d-697776518faa.png)    

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
![image](299789925-16af1efa-2fb8-44ce-8e23-128430e6d46c.png)  
```
cp /etc/dhcp/dhcpd.conf{.example,}
nano /etc/dhcp/dhcpd.conf
```
поправляем файл:  
![image](309228711-9263f9b8-4ac5-41e5-aa44-4d93285e774e.png)  


Проверяем файл на правильность заполнения. Обратите внимание, что файл заполнен в точности со скриншотом выше. (фигурные скобки в начале и конце секции, знаки **;** и тд.)
```
dhcpd -t -cf /etc/dhcp/dhcpd.conf
```
![image](305547950-77444d5e-534b-42d9-9b55-e976e9ce13ff.png)   

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
![image](306256459-84c4ba59-3f4b-442b-a86f-bb7911b963a0.png)  

**4.	Настройте локальные учётные записи на всех устройствах в соответствии с таблицей 2.**  
**Таблица №2**  

| Логин | Пароль | Примечание |
| :---         |     :---:      |          ---: |
| Admin   | P@ssw0rd     | CLI HQ-SRV HQ-R    |
| Branch admin     | P@ssw0rd       | BR-SRV BR-R      |
| Network admin     | P@ssw0rd       | HQ-R BR-R BR-SRV      |

## **CLI**  
Открываем центр управления системой, вводим пароль
Пароль: toor  
![image](299795574-7ccf9221-f244-4b1c-b257-884ce25d27dc.png)  
![image](Wrc63Us1Qm0.jpg)

## **BR-R**

```
useradd branch-admin -m -c "Branch admin" -U
passwd branch-admin
useradd network-admin -m -c "Network admin" -U
passwd network-admin
```
![image](6Yv0KLc08m8.jpg)  

**BR-SRV**
```
useradd branch-admin -m -c "Branch admin" -U
passwd branch-admin
useradd network-admin -m -c "Network admin" -U
passwd network-admin
```
![image](6Yv0KLc08m8.jpg)  

## **HQ-R**

```
useradd network-admin -m -c "Network admin" -U
passwd network-admin
useradd admin -m -c "Admin" -U
passwd admin
```
![image](6Yv0KLc08m8.jpg)

## **HQ-SRV**
```
useradd admin -m -c "Admin" -U
passwd admin
```
![image](6Yv0KLc08m8.jpg)  

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
![image](306581016-f972d469-1875-4666-9a10-f5d9ce647f5b.png)  

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
![image](299799620-5cb84998-f6b5-4951-8d2c-095aa8e7e96d.png)  
```
ctrl-x
y
жмём enter
chmod +x backup-script.sh
./backup-script.sh
```
Проверим содержимое файла бэкапа. Обратите внимание, что название архива варьируется, в зависимости от даты, когда была сделана резервная копия!
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
![image](299799620-5cb84998-f6b5-4951-8d2c-095aa8e7e96d.png)  
```
ctrl-x
y
жмём enter
chmod +x backup-script.sh
./backup-script.sh
```
Проверим содержимое файла бэкапа. Обратите внимание, что название архива варьируется, в зависимости от даты, когда была сделана резервная копия!
```
tar -tf /opt/backup/hq-r-06.01.24.tgz | less
```

**7.	Настройте подключение по SSH для удалённого конфигурирования устройства HQ-SRV по порту 2223. Учтите, что вам необходимо перенаправить трафик на этот порт посредством контролирования трафика.**
## **HQ-SRV**

```
nano /etc/openssh/sshd_config
```
![image](Y5UfoXIZO8s.jpg)  
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
![image](307110973-4bec0562-f600-4c8f-9a14-c67fa686edc4.png)  

Выполняем проверку подключения.   

**8.	Настройте контроль доступа до HQ-SRV по SSH со всех устройств, кроме CLI.**

## **HQ-SRV**

```
systemctl enable --now nftables.service
nft add rule inet filter input ip saddr 10.0.1.2 tcp dport 2222 counter drop
nano /etc/nftables/nftables.nft
```
Удаляем всё содержимое файла до закоментированных строк:  
![image](307135980-2921301e-e2d5-464f-ad03-fc2eb33b9043.png)  

```
nft list ruleset | tee -a /etc/nftables/nftables.nft
```
![image](307136281-718a8fb8-eb1a-4874-8d7c-76a39fbe9cc2.png)  

```
systemctl restart nftables
nft list ruleset
```
![image](307136596-e1373d1d-bf3d-40fb-8aa0-2c8625ea1090.png)  

Выполняем проверку:  
## **CLI**
![image](307137997-735c0cdd-a2f7-4186-b2cd-91e5310724f1.png)  
В результате настройки, соединение с сервером **HQ-SRV** по ssh установить не удастся.  

