# Практическая работа №6
## OSPF. Фильтрация 
##### В данной самостоятельной работе необходимо настроить OSPF офисе Москва, разделить сеть на зоны и настроить фильтрацию между зонами
------------

### *Для выполнения самостоятельной работы необходимо:*
1. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone
2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101
------------

### Итоговая конфигурация устройств
- Маршрутизатор R12
```
interface Ethernet0/2
 ip address 10.120.0.2 255.255.255.252
 ip ospf 1 area 10
!
interface Ethernet0/3
 ip address 10.120.0.22 255.255.255.252
 ip ospf 1 area 10
```
- Маршрутизатор R13
```
interface Ethernet0/2
 ip address 10.120.0.18 255.255.255.252
 ip ospf 1 area 10
!
interface Ethernet0/3
 ip address 10.120.0.6 255.255.255.252
 ip ospf 1 area 10
```
- Маршрутизатор R14
```
interface Ethernet0/0
 ip address 10.120.0.1 255.255.255.252
 ip ospf 1 area 10
!
interface Ethernet0/1
 ip address 10.120.0.5 255.255.255.252
 ip ospf 1 area 10
!
interface Ethernet0/3
 ip address 10.120.0.9 255.255.255.252
 ip ospf 1 area 101
!
interface Ethernet1/0
 ip address 10.120.0.13 255.255.255.252
 ip ospf 1 area 0
!
router ospf 1
 area 101 stub no-summary
 default-information originate always
```
- Маршрутизатор R15
```
interface Ethernet0/0
 ip address 10.120.0.17 255.255.255.252
 ip ospf 1 area 10
!
interface Ethernet0/1
 ip address 10.120.0.21 255.255.255.252
 ip ospf 1 area 10
!
interface Ethernet0/3
 ip address 10.120.0.25 255.255.255.252
 ip ospf 1 area 102
!
interface Ethernet1/0
 ip address 10.120.0.14 255.255.255.252
 ip ospf 1 area 0
!
router ospf 1
 area 102 filter-list prefix FILTER_INTO_AREA_102 in
 default-information originate always
!
ip prefix-list FILTER_INTO_AREA_102 seq 5 deny 10.120.0.8/30
ip prefix-list FILTER_INTO_AREA_102 seq 10 permit 0.0.0.0/0 le 32
```
- Маршрутизатор R19
```
interface Ethernet0/0
 ip address 10.120.0.10 255.255.255.252
 ip ospf 1 area 101
!
router ospf 1
 area 101 stub
```
- Маршрутизатор R20
```
interface Ethernet0/0
 ip address 10.120.0.26 255.255.255.252
 ip ospf 1 area 102
```
