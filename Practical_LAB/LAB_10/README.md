# Практическая работа №10
## iBGP
------------

#### Цель:
- Настроить iBGP в офисе Москва
- Настроить iBGP в сети провайдера Триада
- Организовать полную IP связанность всех сетей
------------

#### Описание/Пошаговая инструкция выполнения домашнего задания:

1. Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15
2. Настроите iBGP в провайдере Триада, с использованием RR
3. Настройте офиса Москва так, чтобы приоритетным провайдером стал Ламас
4. Настройте офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно
5. Все сети в лабораторной работе должны иметь IP связность
6. Файл в EVE-NG можно скачать нажавав на данную [ссылку](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_10/_Exports_unetlab_export-20251005-194525.zip)
------------

### Итоговая конфигурация
- Настроите iBGP в офисом Москва (AS 1001) между маршрутизаторами R14 и R15
```
R14#sh running-config | section bgp
router bgp 1001
 bgp log-neighbor-changes
 network 212.100.1.0 mask 255.255.255.252
 neighbor 10.10.0.15 remote-as 1001
 neighbor 10.10.0.15 update-source Loopback0
 neighbor 10.10.0.15 next-hop-self
 neighbor 212.100.1.1 remote-as 101
```
```
R15#show running-config | section bgp
router bgp 1001
 bgp log-neighbor-changes
 network 212.95.0.0 mask 255.255.255.252
 neighbor 10.10.0.14 remote-as 1001
 neighbor 10.10.0.14 update-source Loopback0
 neighbor 10.10.0.14 next-hop-self
 neighbor 212.95.0.1 remote-as 301
```
- Настроите iBGP в провайдере Триада (AS 520), с использованием RR
```
R23#show running-config | section bgp
router bgp 520
 bgp log-neighbor-changes
 network 60.122.1.0 mask 255.255.255.252
 neighbor 10.14.0.24 remote-as 520
 neighbor 10.14.0.24 update-source Loopback0
 neighbor 10.14.0.24 next-hop-self
```
```
R24#show running-config | section bgp
router bgp 520
 bgp log-neighbor-changes
 network 70.155.1.0 mask 255.255.255.252
 network 89.100.1.0 mask 255.255.255.252
 neighbor AS520 peer-group
 neighbor AS520 remote-as 520
 neighbor AS520 update-source Loopback0
 neighbor AS520 next-hop-self
 neighbor 10.14.0.23 peer-group AS520
 neighbor 10.14.0.25 peer-group AS520
 neighbor 10.14.0.26 peer-group AS520
 neighbor 70.155.1.1 remote-as 301
 neighbor 89.100.1.2 remote-as 2042
```
```
R25#show running-config | section bgp
router bgp 520
 bgp log-neighbor-changes
 network 50.100.1.0 mask 255.255.255.252
 network 100.100.1.0 mask 255.255.255.252
 neighbor 10.14.0.24 remote-as 520
 neighbor 10.14.0.24 update-source Loopback0
 neighbor 10.14.0.24 next-hop-self
```
```
R26#show running-config | section bgp
router bgp 520
 bgp log-neighbor-changes
 network 89.95.0.0 mask 255.255.255.252
 network 100.95.0.0 mask 255.255.255.252
 neighbor 10.14.0.24 remote-as 520
 neighbor 10.14.0.24 update-source Loopback0
 neighbor 10.14.0.24 next-hop-self
 neighbor 89.95.0.2 remote-as 2042
```
- Настройка офиса Москва (AS 1001) так, чтобы приоритетным провайдером стал Ламас
```
R14#show running-config
ip prefix-list AS301_LAMAS_LP seq 5 permit 0.0.0.0/0 le 32
!
route-map AS301_LAMAS_LP permit 10
 match ip address prefix-list AS301_LAMAS_LP
 set local-preference 200
!
router bgp 1001
 neighbor 212.95.0.1 route-map AS301_LAMAS_LP in
```
- Настройrf офиса С.-Петербург (AS 2042) так, чтобы трафик до любого офиса распределялся по двум линкам одновременно
```
R18#show running-config | section bgp
router bgp 2042
 bgp log-neighbor-changes
 bgp bestpath as-path multipath-relax
 network 89.95.0.0 mask 255.255.255.252
 network 89.100.1.0 mask 255.255.255.252
 neighbor 89.95.0.1 remote-as 520
 neighbor 89.100.1.1 remote-as 520
 maximum-paths 2
```
```
R18#sh ip bgp
BGP table version is 16, local router ID is 10.11.0.18
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  50.100.1.0/30    89.100.1.1                             0 520 i
 *>  60.122.1.0/30    89.100.1.1                             0 520 i
 *m  70.155.1.0/30    89.95.0.1                              0 520 i
 *>                   89.100.1.1               0             0 520 i
 *   89.95.0.0/30     89.100.1.1                             0 520 i
 *                    89.95.0.1                0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *   89.100.1.0/30    89.95.0.1                              0 520 i
 *                    89.100.1.1               0             0 520 i
 *>                   0.0.0.0                  0         32768 i
 *m  100.95.0.0/30    89.100.1.1                             0 520 i
 *>                   89.95.0.1                0             0 520 i
 *>  100.100.1.0/30   89.100.1.1                             0 520 i
 *m  112.95.0.0/30    89.95.0.1                              0 520 301 i
 *>                   89.100.1.1                             0 520 301 i
 *m  212.95.0.0/30    89.95.0.1                              0 520 301 i
 *>                   89.100.1.1                             0 520 301 i
 *m  212.100.1.0/30   89.95.0.1                              0 520 301 101 i
 *>                   89.100.1.1                             0 520 301 101 i
```
```
R18#sh ip bgp 70.155.1.0/30
BGP routing table entry for 70.155.1.0/30, version 13
Paths: (2 available, best #2, table default)
Multipath: eBGP
  Advertised to update-groups:
     1
  Refresh Epoch 1
  520
    89.95.0.1 from 89.95.0.1 (10.14.0.26)
      Origin IGP, localpref 100, valid, external, multipath(oldest)
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  520
    89.100.1.1 from 89.100.1.1 (10.14.0.24)
      Origin IGP, metric 0, localpref 100, valid, external, multipath, best
      rx pathid: 0, tx pathid: 0x0
```
- Все сети в лабораторной работе имеют IP связность, пример:
```
R18#ping 112.95.0.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 112.95.0.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
