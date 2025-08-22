# Практическая работа №8
## EIGRP
##### В данной самостоятельной работе необходимо yастроить EIGRP в С.-Петербург. Использовать named "EIGRP"
------------

### *Для выполнения самостоятельной работы необходимо:*
1. В офисе С.-Петербург настроить EIGRP.
2. R32 получает только маршрут по умолчанию.
3. R16-17 анонсируют только суммарные префиксы.
4. Использовать EIGRP named-mode для настройки сети.
------------

![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_8/Screen/Screen_%D0%A1.-%D0%9F%D0%B5%D1%82%D0%B5%D1%80%D0%B1%D1%83%D1%80%D0%B3%20-%20AS2042.png)
### Итоговая конфигурация устройств
- Маршрутизатор R16
```
router eigrp EIGRP
 !
 address-family ipv4 unicast autonomous-system 1
  !
  af-interface Ethernet0/3
   summary-address 0.0.0.0 0.0.0.0
  exit-af-interface
  !
  af-interface Ethernet0/0
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 10.121.0.0 255.255.255.0
   summary-address 192.168.64.0 255.255.252.0
  exit-af-interface
  !
  topology base
   redistribute connected
  exit-af-topology
  network 10.121.0.0 0.0.0.3
  network 10.121.0.8 0.0.0.3
  network 10.121.0.12 0.0.0.3
  network 10.243.0.0 0.0.0.255
  network 192.168.64.0
  network 192.168.65.0
 exit-address-family
```
- Маршрутизатор R17
```
router eigrp EIGRP
 !
 address-family ipv4 unicast autonomous-system 1
  !
  af-interface Ethernet0/1
   summary-address 10.121.0.0 255.255.255.0
   summary-address 192.168.64.0 255.255.252.0
  exit-af-interface
  !
  af-interface Ethernet0/0
   passive-interface
  exit-af-interface
  !
  topology base
   redistribute connected
  exit-af-topology
  network 10.121.0.4 0.0.0.3
  network 10.121.0.12 0.0.0.3
  network 10.243.0.0 0.0.0.255
  network 192.168.64.0
  network 192.168.65.0
 exit-address-family
```
- Маршрутизатор R18
```
router eigrp EIGRP
 !
 address-family ipv4 unicast autonomous-system 1
  !
  topology base
   redistribute connected
  exit-af-topology
  network 10.121.0.0 0.0.0.3
  network 10.121.0.4 0.0.0.3
 exit-address-family
```
- Маршрутизатор R32
```
router eigrp EIGRP
 !
 address-family ipv4 unicast autonomous-system 1
  !
  topology base
   redistribute connected
  exit-af-topology
  network 10.121.0.8 0.0.0.3
 exit-address-family
```
