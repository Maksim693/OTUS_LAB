# Практическая работа №14
## IPSec over DmVPN
------------

#### Цель:
- Настроить GRE поверх IPSec между офисами Москва и С.-Петербург
- Настроить DMVPN поверх IPSec между офисами Москва и Чокурдах, Лабытнанги
------------

### Настроить GRE поверх IPSec между офисами Москва и С.-Петербург
- Настройка на R15 (Москва)
```
crypto pki server CA
!
crypto pki trustpoint CA
 revocation-check crl
 rsakeypair CA
!
crypto pki trustpoint VPN
 enrollment url http://10.10.10.1:80
 serial-number
 ip-address 10.10.10.2
 subject-name CN=R4,OU=VPN,O=Server,C=RU
 revocation-check none
 rsakeypair VPN
!
crypto pki trustpoint R15_CA
 enrollment url http://10.10.10.1:80
 serial-number
 ip-address 10.10.10.1
 subject-name CN=R4,OU=VPN,O=Server,C=RU
 revocation-check none
 rsakeypair R15_CA
!
crypto pki certificate chain CA
crypto pki certificate chain VPN
crypto pki certificate chain R15_CA
!
crypto isakmp policy 5
 encr 3des
 hash sha256
 authentication pre-share
 group 19
crypto isakmp key cisco address 89.100.1.2
crypto isakmp key cisco address 0.0.0.0
!
crypto ipsec transform-set SET1 esp-3des
 mode tunnel
!
crypto ipsec profile GRE_P
 set transform-set SET1
!
crypto map MAP 5 ipsec-isakmp
 set peer 89.100.1.2
 set transform-set SET1
 match address GRE_IPSEC_MO-PETER_TUN_1
!
interface Tunnel1
 ip address 10.10.10.1 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 212.95.0.2
 tunnel destination 89.100.1.2
 tunnel protection ipsec profile GRE_P
!
ip access-list extended GRE_IPSEC_MO-PETER_TUN_1
 permit gre host 212.95.0.2 host 89.100.1.2
```
- Настройка на R18 (Питер)
```
crypto pki trustpoint VPN
 enrollment url http://212.95.0.2:80
 serial-number
 ip-address 10.10.10.2
 subject-name CN=R4,OU=VPN,O=Server,C=RU
 revocation-check none
 rsakeypair VPN
!
crypto pki certificate chain VPN
!
crypto isakmp policy 5
 encr 3des
 hash sha256
 authentication pre-share
 group 19
crypto isakmp key cisco address 212.95.0.2
!
crypto ipsec transform-set SET1 esp-3des
 mode tunnel
!
crypto ipsec profile GRE_P
 set transform-set SET1
!
crypto map MAP 5 ipsec-isakmp
 set peer 212.95.0.2
 set transform-set SET1
 match address GRE_IPSEC_MO-PETER_TUN_1
!
interface Tunnel1
 ip address 10.10.10.2 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 89.100.1.2
 tunnel destination 212.95.0.2
 tunnel protection ipsec profile GRE_P
!
ip access-list extended GRE_IPSEC_MO-PETER_TUN_1
 permit gre host 89.100.1.2 host 212.95.0.2
```
- Проверка на R15
```
R15#sh crypto isakmp sa
IPv4 Crypto ISAKMP SA
dst             src             state          conn-id status
89.100.1.2      212.95.0.2      QM_IDLE           1001 ACTIVE

IPv6 Crypto ISAKMP SA
```
### Настроить GRE поверх IPSec между офисами Москва и С.-Петербург
- Настройка на R15 (Москва)
```
```
- Настройка на R27 (Лабытнанги)
```
```
- Настройка на R28 (Чокурдах)
```
```
