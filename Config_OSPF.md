![Карта сети ](/ospf.png)
```console
#Установка frr
yum install frr.x86_64

#Настройка демона, включаем zebra=yes и ospfd=yes
#vim /etc/frr/daemons
zebra=yes
bgpd=no
ospfd=yes
ospf6d=no
ripd=no
ripngd=no
isisd=no
pimd=no
pim6d=no
nhrpd=no
eigrpd=no
sharpd=no
pbrd=no
bfdd=no
fabricd=no
vrrpd=no
pathd=no

#Запускаем демона
systemctl start frr
systemctl enable frr

#Дополнительная информация
#Приступая к настройке OSPF, нужно помнить следующее.
#Пассивные, или тупиковые, интерфейсы.
#Пассивный, или тупиковый, интерфейс — это интерфейс, за которым далее нет марш-
#рутизатора, т. е. в той стороне только одна сеть, к которой непосредственно подключено
#устройство.

#Настройка маршрутизатиора LFR
vtysh
configure terminal
interface tun1
ip ospf network broadcast
router ospf
passive-interface default enp1s0
passive-interface default enp7s0
network 172.16.20.0/24 area 0
network 172.16.50.0/24 area 0
network 172.16.55.0/30 area 0
network 10.5.5.0/30 area 0
default-information originate 

#Настройка маршрутизатиора L-RTR-A
vtysh
configure terminal
router ospf
network 172.16.50.0/30 area 0
network 172.16.100.0/24 area 0
default-information originate

#Настройка маршрутизатиора L-RTR-B
vtysh
configure terminal
router ospf
network 172.16.55.0/30 area 0
network 172.16.200.0/24 area 0
default-information originate

#Настройка маршрутизатиора R-FR
vtysh
configure terminal
interface tun1
ip ospf network broadcast
router ospf
network 10.5.5.0/30 area 0
default-information originate
passive-interface default enp1s0

#Проверка работы
sh ip ospf neh
sh ip sopf route
```




















