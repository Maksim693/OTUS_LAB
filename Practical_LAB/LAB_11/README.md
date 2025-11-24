# Практическая работа №11
## BGP. Фильтрация
------------

#### Цель:
- Настроить фильтрацию для офисе Москва
- Настроить фильтрацию для офисе С.-Петербург
------------

#### Описание/Пошаговая инструкция выполнения домашнего задания:

1. Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).
2. Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).
3. Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
4. Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.
5. Все сети в лабораторной работе должны иметь IP связность
6. Файл в EVE-NG можно скачать нажава на данную [ссылку](https)
------------

### Итоговая конфигурация
- Настройка фильтрации в офисе Москва так, чтобы не появилось транзитного трафика(As-path)
```
R14#sh run | sec as-p
ip as-path access-list 1 permit ^$

R14#sh run | sec bg
router bgp 1001
 bgp log-neighbor-changes
 network 212.100.1.0 mask 255.255.255.252
 neighbor 10.10.0.15 remote-as 1001
 neighbor 10.10.0.15 update-source Loopback0
 neighbor 10.10.0.15 next-hop-self
 neighbor 212.100.1.1 remote-as 101
 neighbor 212.100.1.1 filter-list 1 out
```
```
R15#sh run | sec as-p
ip as-path access-list 1 permit ^$

R15#sh run | sec bgp
router bgp 1001
 bgp log-neighbor-changes
 network 212.95.0.0 mask 255.255.255.252
 neighbor 10.10.0.14 remote-as 1001
 neighbor 10.10.0.14 update-source Loopback0
 neighbor 10.10.0.14 next-hop-self
 neighbor 212.95.0.1 remote-as 301
 neighbor 212.95.0.1 route-map AS301_LAMAS_LP in
 neighbor 212.95.0.1 filter-list 1 out
```
- Настройка фильтрациb в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list)
```
R18#sh run | sec ip pref
ip prefix-list NO-TRANSIT seq 5 permit 89.95.0.0/30
ip prefix-list NO-TRANSIT seq 10 permit 89.100.1.0/30

R18#sh run | sec bgp
router bgp 2042
 bgp log-neighbor-changes
 bgp bestpath as-path multipath-relax
 network 89.95.0.0 mask 255.255.255.252
 network 89.100.1.0 mask 255.255.255.252
 neighbor 89.95.0.1 remote-as 520
 neighbor 89.95.0.1 prefix-list NO-TRANSIT out
 neighbor 89.100.1.1 remote-as 520
 neighbor 89.100.1.1 prefix-list NO-TRANSIT out
 maximum-paths 2
```
- Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.
```
R22#sh run | section bgp
router bgp 101
 bgp log-neighbor-changes
 network 60.122.1.0 mask 255.255.255.252
 network 112.95.0.0 mask 255.255.255.252
 network 212.100.1.0 mask 255.255.255.252
 neighbor 112.95.0.2 remote-as 301
 neighbor 212.100.1.2 remote-as 1001
 neighbor 212.100.1.2 default-originate
 neighbor 212.100.1.2 route-map ONLY_DEF out

R22#sh run | section route-m
 neighbor 212.100.1.2 route-map ONLY_DEF out
route-map ONLY_DEF deny 10
```
- Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.
```
R21#sh run | sec bgp
router bgp 301
 bgp log-neighbor-changes
 network 70.155.1.0 mask 255.255.255.252
 network 112.95.0.0 mask 255.255.255.252
 network 212.95.0.0 mask 255.255.255.252
 neighbor 70.155.1.2 remote-as 520
 neighbor 112.95.0.1 remote-as 101
 neighbor 212.95.0.2 remote-as 1001
 neighbor 212.95.0.2 default-originate
 neighbor 212.95.0.2 route-map PTR_MSK_DEF out

R21#sh run | sec route-m
 neighbor 212.95.0.2 route-map PTR_MSK_DEF out
route-map PTR-MSK_DEF permit 10
 match as-path 1

R21#sh run | sec acc
ip as-path access-list 1 permit _2042$
```
