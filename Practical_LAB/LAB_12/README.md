# Практическая работа №12
## Основные протоколы сети интернет
------------

#### Цель:
- Настроить DHCP в офисе Москва
- Настроить синхронизацию времени в офисе Москва
- Настроить NAT в офисе Москва, C.-Перетбруг и Чокурдах
------------

#### Описание/Пошаговая инструкция выполнения домашнего задания:

1. Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.
2. Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.
3. Настроите статический NAT для R20.
4. Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.
5. Настроите статический NAT(PAT) для офиса Чокурдах.
6. Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.
7. Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.
8. Все сети в лабораторной работе должны иметь IP связность
9. Файл в EVE-NG можно скачать нажав на данную [ссылку](https)
------------

### Итоговая конфигурация
- Настройка NAT(PAT) на R14 и R15. Трансляция осуществляется в адрес автономной системы AS1001.
```
access-list 88 permit 192.168.0.0 0.0.0.255
access-list 88 permit 192.168.1.0 0.0.0.255
!
ip nat inside source list 88 interface Ethernet0/2 overload
!
interface Ethernet0/0
 ip nat inside
 ip ospf 1 area 10
!
interface Ethernet0/1
 ip nat inside
!
interface Ethernet0/2
 ip nat outside
 !
interface Ethernet1/0
 ip nat inside
```
```
R15#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
udp 212.95.0.2:20944   192.168.1.4:20944  70.155.1.2:20945   70.155.1.2:20945
udp 212.95.0.2:40545   192.168.1.4:40545  70.155.1.2:40546   70.155.1.2:40546
icmp 212.95.0.2:44544  192.168.1.4:44544  70.155.1.2:44544   70.155.1.2:44544
icmp 212.95.0.2:44800  192.168.1.4:44800  70.155.1.2:44800   70.155.1.2:44800
icmp 212.95.0.2:45056  192.168.1.4:45056  70.155.1.2:45056   70.155.1.2:45056
icmp 212.95.0.2:45312  192.168.1.4:45312  70.155.1.2:45312   70.155.1.2:45312
```
- Настройка NAT(PAT) на R18. Трансляция должна осуществляется в пул из 5 адресов автономной системы AS2042.
```
route-map SPD_TRD_03 permit 10
 match ip address 88
 match interface Ethernet0/3
!
route-map SPD_TRD_02 permit 10
 match ip address 88
 match interface Ethernet0/2
!
ip nat pool SPB_TRIADA_03 89.95.0.3 89.95.0.6 netmask 255.255.255.248
ip nat pool SPB_TRIADA_02 89.100.1.3 89.100.1.6 netmask 255.255.255.248
ip nat inside source route-map SPD_TRD_02 pool SPB_TRIADA_02 overload
ip nat inside source route-map SPD_TRD_03 pool SPB_TRIADA_03 overload
!
interface Ethernet0/0
 ip address 10.121.0.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/1
 ip address 10.121.0.5 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
!
interface Ethernet0/2
 ip address 89.100.1.2 255.255.255.248
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/3
 ip address 89.95.0.2 255.255.255.248
 ip nat outside
 ip virtual-reassembly in
!
access-list 88 permit 192.168.64.0 0.0.0.255
access-list 88 permit 192.168.65.0 0.0.0.255
```
```
R18#sh ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 89.95.0.4:20878   192.168.64.4:20878 212.100.1.2:20878  212.100.1.2:20878
icmp 89.95.0.4:21390   192.168.64.4:21390 212.100.1.2:21390  212.100.1.2:21390
icmp 89.95.0.4:21902   192.168.64.4:21902 212.100.1.2:21902  212.100.1.2:21902
icmp 89.95.0.4:22414   192.168.64.4:22414 212.100.1.2:22414  212.100.1.2:22414
icmp 89.95.0.4:22926   192.168.64.4:22926 212.100.1.2:22926  212.100.1.2:22926
```
- Настройка статического NAT для R20
```
ip nat inside source static 10.120.0.26 212.95.0.2
```
- Настройка NAT так, чтобы R19 был доступен с любого узла для удаленного управления
```
ip nat inside source static 10.120.0.10 212.100.1.2
!
interface Ethernet0/3
 ip nat inside
```
- Настройка статического NAT(PAT) для офиса Чокурдах
```
```
- Настройка для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP
##### Настройка на R-12
```
ip dhcp excluded-address 192.168.0.1
ip dhcp excluded-address 192.168.0.2
ip dhcp excluded-address 192.168.0.3
ip dhcp excluded-address 192.168.1.1
ip dhcp excluded-address 192.168.1.2
ip dhcp excluded-address 192.168.1.3
ip dhcp excluded-address 192.168.1.129 192.168.1.254
ip dhcp excluded-address 192.168.0.129 192.168.0.254
!
ip dhcp pool USER20
 network 192.168.0.0 255.255.255.0
 default-router 192.168.0.1
!
ip dhcp pool USER30
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 ```
 ##### Настройка на R-13
 ```
ip dhcp excluded-address 192.168.0.1
ip dhcp excluded-address 192.168.0.2
ip dhcp excluded-address 192.168.0.3
ip dhcp excluded-address 192.168.1.1
ip dhcp excluded-address 192.168.1.2
ip dhcp excluded-address 192.168.1.3
ip dhcp excluded-address 192.168.1.4 192.168.1.128
ip dhcp excluded-address 192.168.0.4 192.168.0.128
!
ip dhcp pool USER20
 network 192.168.0.0 255.255.255.0
 default-router 192.168.0.1
!
ip dhcp pool USER30
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
```
```
VPCS> ip dhcp
DDORA IP 192.168.1.5/24 GW 192.168.1.1
```
```
VPCS> ip dhcp
DORA IP 192.168.0.129/24 GW 192.168.0.1
```
- Настройка NTP сервер на R12 и R13. Все устройства в офисе Москва синхронизируют время с R12 и R13
```
ntp master 1
ntp update-calendar
!
interface Loopback0
 ip address 10.10.0.12 255.255.255.255
 ntp broadcast
```
```
ntp server 10.10.0.12
ntp server 10.10.0.13
```
```
MLSW4#sh ntp status
Clock is synchronized, stratum 2, reference is 10.10.0.12
nominal freq is 250.0000 Hz, actual freq is 250.0000 Hz, precision is 2**10
ntp uptime is 5500 (1/100 of seconds), resolution is 4000
reference time is ECD31658.01CAC088 (18:26:32.007 UTC Thu Nov 27 2025)
clock offset is 0.0000 msec, root delay is 0.00 msec
root dispersion is 380.89 msec, peer dispersion is 189.45 msec
loopfilter state is 'CTRL' (Normal Controlled Loop), drift is 0.000000000 s/s
system poll interval is 64, last update was 44 sec ago.
```
