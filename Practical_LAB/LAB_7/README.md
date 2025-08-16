# Практическая работа №7
## IS-IS. Продолжение
##### В данной самостоятельной работе необходимо настроить IS-IS офисе Триада
------------

### *Для выполнения самостоятельной работы необходимо:*
1. [Настроите IS-IS в ISP Триада](#адре)
2. [R23 и R25 находятся в зоне 2222](#адре)
3. [R24 находится в зоне 24](#адре)
4. [R26 находится в зоне 26](#адре)
------------

![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_7/Screen/Screen_%D0%A2%D1%80%D0%B8%D0%B0%D0%B4%D0%B0%20(AS%20520).png)
### Итоговая конфигурация устройств
- Маршрутизатор R23
```
router isis
 net 49.2222.0010.0014.0000.0023.00
!
interface Ethernet0/1
 ip address 10.241.0.1 255.255.255.252
 ip router isis
!
interface Ethernet0/2
 ip address 10.241.0.5 255.255.255.252
 ip router isis
```
- Маршрутизатор R24
```
router isis
 net 49.0024.0010.0014.0000.0024.00
!
interface Ethernet0/1
 ip address 10.241.0.9 255.255.255.252
 ip router isis
!
interface Ethernet0/2
 ip address 10.241.0.6 255.255.255.252
 ip router isis
```
- Маршрутизатор R25
```
router isis
 net 49.2222.0010.0014.0000.0025.00
!
interface Ethernet0/0
 ip address 10.241.0.2 255.255.255.252
 ip router isis
!
interface Ethernet0/2
 ip address 10.241.0.13 255.255.255.252
 ip router isis
```
- Маршрутизатор R26
```
interface Ethernet0/0
 ip address 10.241.0.10 255.255.255.252
 ip router isis
!
interface Ethernet0/2
 ip address 10.241.0.14 255.255.255.252
 ip router isis
!
router isis
 net 49.0026.0010.0014.0000.0026.00
```
