# Практическая работа №5
## Маршрутизация на основе политик (PBR) 
##### В данной самостоятельной работе необходимо настроить политику маршрутизации в офисе Чокурдах и распределить трафик между 2 линками
------------

### *Для выполнения самостоятельной работы необходимо:*
1. Настроите политику маршрутизации для сетей офиса
2. Распределите трафик между двумя линками с провайдером
3. Настроите отслеживание линка через технологию IP SLA (только для IPv4)
4. Настройте для офиса Лабытнанги маршрут по-умолчанию
5. План работы и изменения зафиксированы в документации
------------

### Конфиг настроек политик маршрутизации и распределении трафика между двумя линками с провайдером для сетей офиса. Также конфиг настроек IP-SLA для отслеживания линка.
```
interface Ethernet0/2.60
 encapsulation dot1Q 60
 ip address 192.168.128.1 255.255.255.0
 ip policy route-map PBR_USER_60
!
interface Ethernet0/2.70
 encapsulation dot1Q 70
 ip address 192.168.129.1 255.255.255.0
 ip policy route-map PBR_USER_70
!
ip access-list standard PBR_USER_Vlan_60
 permit 192.168.128.0 0.0.0.255
 deny   any
ip access-list standard PBR_USER_Vlan_70
 permit 192.168.129.0 0.0.0.255
 deny   any
!
ip sla 1
 icmp-jitter 100.100.1.1 source-ip 100.100.1.2 num-packets 5
 threshold 2
 timeout 2
 frequency 4
ip sla schedule 1 life forever start-time now
ip sla 2
 icmp-jitter 100.95.0.1 source-ip 100.95.0.2 num-packets 5
 threshold 2
 timeout 2
 frequency 4
ip sla schedule 2 life forever start-time now
!
route-map PBR_USER_70 permit 20
 match ip address PBR_USER_Vlan_70
 set ip next-hop verify-availability 100.100.1.1 10 track 1
 set ip next-hop verify-availability 100.95.0.1 20 track 2
!
route-map PBR_USER_60 permit 10
 match ip address PBR_USER_Vlan_60
 set ip next-hop verify-availability 100.95.0.1 10 track 2
 set ip next-hop verify-availability 100.100.1.1 20 track 1
```
### Настройка маршрута по-умолчанию для офиса Лабытнанги
```
ip route 0.0.0.0 0.0.0.0 50.100.1.1
```
