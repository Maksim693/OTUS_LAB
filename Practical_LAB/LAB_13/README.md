# Практическая работа №13
## VPN. GRE. DmVPN
------------

#### Цель:
- Настроить GRE между офисами Москва и С.-Петербург
- Настроить DMVPN между офисами Москва и Чокурдах, Лабытнанги
------------

### Настройка GRE между офисами Москва и С.-Петербург
- Настройка на R15 (Москва)
```
interface Tunnel1
 ip address 10.10.10.1 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 212.95.0.2
 tunnel destination 89.100.1.2
```
- Настройка на R18 (Питер)
```
interface Tunnel1
 ip address 10.10.10.2 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 89.100.1.2
 tunnel destination 212.95.0.2
```
- Проверка, ping  R15-R18
```
R15#ping 10.10.10.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.10.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
###  Настройка DMVPN между офисами Москва и Чокурдах, Лабытнанги
- Настройка на R15 (Москва)
```
interface Tunnel2
 ip address 10.20.20.1 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast dynamic
 ip nhrp network-id 50
 ip tcp adjust-mss 1360
 ip ospf network broadcast
 ip ospf priority 255
 ip ospf 1 area 0
 tunnel source Ethernet0/2
 tunnel mode gre multipoint
```
- Настройка на R27 (Лабытнанги)
```
interface Tunnel2
 ip address 10.20.20.3 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 212.95.0.2
 ip nhrp map 10.20.20.1 212.95.0.2
 ip nhrp network-id 50
 ip nhrp nhs 10.20.20.1
 ip tcp adjust-mss 1360
 ip ospf network broadcast
 ip ospf priority 0
 ip ospf 1 area 0
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
```
- Настройка на R15 (Чокурдах)
```
interface Tunnel2
 ip address 10.20.20.2 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 212.95.0.2
 ip nhrp map 10.20.20.1 212.95.0.2
 ip nhrp network-id 50
 ip nhrp nhs 10.20.20.1
 ip tcp adjust-mss 1360
 ip ospf network broadcast
 ip ospf priority 0
 ip ospf 1 area 0
 tunnel source Ethernet0/0
 tunnel mode gre multipoint
```
- Проверка, ping R15-R28
```
R15#ping 10.20.20.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.20.20.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
= Проверка, ping R15-R27
```
R15#ping 10.20.20.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.20.20.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
