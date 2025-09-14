# Практическая работа №9
## BGP. Основы
------------

#### Цель:
- Настроить BGP между автономными системами
- Организовать доступность между офисами Москва и С.-Петербург
------------

#### Описание/Пошаговая инструкция выполнения домашнего задания:

1. Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас
2. Настроите eBGP между провайдерами Киторн и Ламас
3. Настроите eBGP между Ламас и Триада
4. Настроите eBGP между офисом С.-Петербург и провайдером Триада
5. Организуете IP доступность между пограничным роутерами офисами Москва и С.-Петербург
------------

### Итоговая конфигурация на устройствах
- Москва, AS 1001
```
R14#sh run | section bgp
router bgp 1001
 bgp log-neighbor-changes
 network 10.10.0.14 mask 255.255.255.255
 neighbor 212.100.1.1 remote-as 101

R15#sh run | sec bg
router bgp 1001
 bgp log-neighbor-changes
 network 10.10.0.15 mask 255.255.255.255
 neighbor 212.95.0.1 remote-as 301
```
- Киторн, AS 101
```
R22#sh run | sec bgp
router bgp 101
 bgp log-neighbor-changes
 network 10.15.0.22 mask 255.255.255.255
 neighbor 112.95.0.2 remote-as 301
 neighbor 212.100.1.2 remote-as 1001
```
- Ламас, AS 301
```
R21#sh running-config | section bgp
router bgp 301
 bgp log-neighbor-changes
 network 10.16.0.21 mask 255.255.255.255
 neighbor 70.155.1.2 remote-as 520
 neighbor 112.95.0.1 remote-as 101
 neighbor 212.95.0.2 remote-as 1001
```
- Триада, AS 520
```
R24#sh run | sec bgp
router bgp 520
 bgp log-neighbor-changes
 network 10.14.0.24 mask 255.255.255.255
 neighbor 70.155.1.1 remote-as 301
 neighbor 89.100.1.2 remote-as 2042

R26#sh run | sec bgp
router bgp 520
 bgp log-neighbor-changes
 network 10.14.0.26 mask 255.255.255.255
 neighbor 89.95.0.2 remote-as 2042
```
- С.-Петербург, AS 2042
```
R18#sh run | sec bgp
router bgp 2042
 bgp log-neighbor-changes
 network 10.11.0.18 mask 255.255.255.255
 neighbor 89.95.0.1 remote-as 520
 neighbor 89.100.1.1 remote-as 520
```
- Проверка IP доступности между пограничным роутерами офисами Москва и С.-Петербург
```
R14#ping 10.11.0.18 source 10.10.0.14
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.11.0.18, timeout is 2 seconds:
Packet sent with a source address of 10.10.0.14
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms

R15#ping 10.11.0.18 source 10.10.0.15
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.11.0.18, timeout is 2 seconds:
Packet sent with a source address of 10.10.0.15
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
