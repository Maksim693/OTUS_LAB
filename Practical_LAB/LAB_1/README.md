# Практическая работа №1
## "VLAN и маршрутизация между VLAN"
![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_1/Pictures_LAB_1/Pict_LAB1)

## *Table of Contents*
- [Build the Network and Configure Basic Device Settings](#Build-the-Network-and-Configure-Basic-Device-Settings)
    - [Cable the network as show in the topology](#1-cable-the-network-as-show-in-the-topology)
    - [Configure basic settings for the router](#2-configure-basic-settings-for-the-router)
    - [Configure basic settings for each switch](#3-configure-basic-settings-for-each-switch)
    - [Configure PC hosts](#4-configure-pc-hosts)
- [Create VLANs and Assign Switch Ports](#Create-VLANs-and-Assign-Switch-Ports)
    - [Create VLANs on both switches](#1-create-vlans-on-both-switches)
    - [Assign VLANs to the correct switch interfaces](#2-assign-vlans-to-the-correct-switch-interfaces)
- [Configure an 802.1Q Trunk Between the Switches](#configure-an-8021q-trunk-between-the-switches)
    - [Manually configure trunk interface F0/1](#1-manually-configure-trunk-interface-f01)
    - [Manually configure S1’s trunk interface F0/5](#2-manually-configure-s1s-trunk-interface-f05)
- [Configure Inter-VLAN Routing on the Router](#Configure-Inter-VLAN-Routing-on-the-Router)
- [Verify Inter-VLAN Routing is Working](#verify-inter-vlan-routing-is-working)
    - [Complete the following tests from PC-A. All should be successful](#1-complete-the-following-tests-from-pc-a-all-should-be-successful)
    - [Complete the following test from PC-B](#2-complete-the-following-test-from-pc-b)

## Build the Network and Configure Basic Device Settings
##### In Part 1, you will set up the network topology and configure basic settings on the PC hosts and switches.
#### 1. Cable the network as show in the topology.
- [x] Attach the devices as shown in the topology diagram, and cable as necessary.
#### 2. Configure basic settings for the router.
- [x] Console into the router and enable privileged EXEC mode.
- [x] Enter configuration mode.
- [x] Assign a device name to the router.
```
R1(config)#hostname R1
```
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
```
R1(config)#no ip domain-lookup
```
- [x] Assign class as the privileged EXEC encrypted password.
```
R1(config)#enable secret class
```
- [x] Assign cisco as the console password and enable login.
```
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
```
- [x] Assign cisco as the VTY password and enable login.
```
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
```
- [x] Encrypt the plaintext passwords.
```
R1(config)#service password-encryption 
```
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
```
R1(config)#banner motd $ Authorized Users Only! $
```
- [x] Save the running configuration to the startup configuration file.
```
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
- [x] Set the clock on the router.
>Непонятно как настроить время в CPT, кроме timezone и настройки с помощью NTP вариантов нет. 
>Первый вариант приложил команды для "ручной" настройки времени на Сisco 
```
R1(config)#clock timezone msk 3
R1(config)#clock set 10:30:00 18 june 2025
```
>Второй вариант это настроить NTP.
```
R1(config)#clock timezone msk 3
R1(config)#ntp server 10.10.10.10
```
###### Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this command.
#### 3. Configure basic settings for each switch
>Дальнейшие команды для свитчей S1 и S2 идентичны настройкам на маршрутизатору R1, поэтому отмечу выполнения данных пуктов.
>Итоговую конфигурацию можно посмотреть в [Приложения](#ссылка)
- [x] Open configuration window
- [x] Console into the switch and enable privileged EXEC mode.
- [x] Enter configuration mode.
- [x] Assign a device name to the switch.
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
- [x] Assign class as the privileged EXEC encrypted password.
- [x] Assign cisco as the console password and enable login.
- [x] Assign cisco as the vty password and enable login.
- [x] Encrypt the plaintext passwords.
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
- [x] Set the clock on the switch.
##### Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this
command.
- [x] Copy the running configuration to the startup configuration.
- [x] Close configuration window
#### 4. Configure PC hosts.
- [x] Refer to the Addressing Table for PC host address information.
![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_1/Pictures_LAB_1/Pict_LAB1_PC)

## Create VLANs and Assign Switch Ports
##### In Part 2, you will create VLANs, as specified in the table above, on both switches. You will then assign the VLANs to the appropriate interface. The show vlan command is used to verify your configuration settings. Complete the following tasks on each switch.
#### 1. Create VLANs on both switches.
- [x] Open configuration window
- [x] Create and name the required VLANs on each switch from the table above.
- [x] Configure the management interface and default gateway on each switch using the IP address
information in the Addressing Table.
- [x] Assign all unused ports on both switches to the ParkingLot VLAN, configure them for static access mode,
and administratively deactivate them.
#### 2. Assign VLANs to the correct switch interfaces.
- [x] Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for
static access mode. Be sure to do this on both switches
- [x] Issue the show vlan brief command and verify that the VLANs are assigned to the correct interfaces.
Close configuration window
> Конфигурация vlan на S1
```
S1#show vlan 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
3    MGMT_3                           active    Fa0/6
4    Operatinons_4                    active    
7    ParkingLot_7                     active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
8    Native_8                         active  
```
> Конфигурация vlan на S2
```
S2#show vlan 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
3    MGMT_3                           active    
4    Operatinons_4                    active    Fa0/18
7    ParkingLot_7                     active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
8    Native_8                         active  
```
## Configure an 802.1Q Trunk Between the Switches
##### In Part 3, you will manually configure interface F0/1 as a trunk.
#### 1. Manually configure trunk interface F0/1.
- [x] Open configuration window
- [x] Change the switchport mode on interface F0/1 to force trunking. Make sure to do this on both switches.
- [x] As a part of the trunk configuration, set the native VLAN to 8 on both switches. You may see error
messages temporarily while the two interfaces are configured for different native VLANs.
- [x] As another part of trunk configuration, specify that VLANs 3, 4, and 8 are only allowed to cross the trunk.
- [x] Issue the show interfaces trunk command to verify trunking ports, the Native VLAN and allowed VLANs
across the trunk.
> Конфигурация vlan на S1 и S2
```
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 8
 switchport trunk allowed vlan 3-4,8
```
#### 2. Manually configure S1’s trunk interface F0/5
- [x] Configure the F0/5 on S1 with the same trunk parameters as F0/1. This is the trunk to the router.
```
interface FastEthernet0/5
 switchport access vlan 7
 switchport trunk native vlan 8
 switchport trunk allowed vlan 3-4,7-8
 switchport mode trunk
```
- [x] Save the running configuration to the startup configuration file on S1 and S2.
Сохраняем конфиги на S1 и S2
```
copy running-config startup-config
```
- [x] Issue the show interfaces trunk command to verify trunking.
```
S1#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      8
Fa0/5       on           802.1q         trunking      8

Port        Vlans allowed on trunk
Fa0/1       3-4,8
Fa0/5       3-4,7-8

Port        Vlans allowed and active in management domain
Fa0/1       3,4,8
Fa0/5       3,4,7,8

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       3,4,8
Fa0/5       3,4,7,8
```
#### Question:
- Why does F0/5 not appear in the list of trunks?
> Интерфей F0/5 порт не будет отоборажаться в выводе команды, если на R1 интерфейс находится в  состоянии "shutdown"
> В моей конфигурации к этому момент все необходимы настройки были произведены, поэтому вы выводе можно увидеть данный интерфейс

## Configure Inter-VLAN Routing on the Router
- [x] Open configuration window
- [x] Activate interface G0/0/1 on the router.
- [x] Configure sub-interfaces for each VLAN as specified in the IP addressing table. All sub-interfaces use
802.1Q encapsulation. Ensure the sub-interface for the native VLAN does not have an IP address
assigned. Include a description for each sub-interface.
```
interface GigabitEthernet0/0/1.3
 description MGMT_3
 encapsulation dot1Q 3
 ip address 192.168.3.1 255.255.255.0
!
interface GigabitEthernet0/0/1.4
 description Operations_4
 encapsulation dot1Q 4
 ip address 192.168.4.1 255.255.255.0
!
interface GigabitEthernet0/0/1.7
 description ParkinLot_7
 encapsulation dot1Q 7
 no ip address
!
interface GigabitEthernet0/0/1.8
 description Native_8
 encapsulation dot1Q 8
 no ip address
```
- [x] Use the show ip interface brief command to verify the sub-interfaces are operational.
```
R1#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES NVRAM  administratively down down 
GigabitEthernet0/0/1   unassigned      YES NVRAM  up                    up 
GigabitEthernet0/0/1.3 192.168.3.1     YES manual up                    up 
GigabitEthernet0/0/1.4 192.168.4.1     YES manual up                    up 
GigabitEthernet0/0/1.7 unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.8 unassigned      YES unset  up                    up 
GigabitEthernet0/0/2   unassigned      YES NVRAM  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
```
- [x] Close configuration window
## Verify Inter-VLAN Routing is Working
#### 1. Complete the following tests from PC-A. All should be successful.
###### Note: You may have to disable the PC firewall for pings to be successful.
- [x] Ping from PC-A to its default gateway.
```
C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
- [x] Ping from PC-A to PC-B
```
C:\>ping 192.168.4.3

Pinging 192.168.4.3 with 32 bytes of data:

Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time=4ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.4.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 4ms, Average = 1ms
```
- [x] Ping from PC-A to S2
```
C:\>ping 192.168.3.12

Pinging 192.168.3.12 with 32 bytes of data:

Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.12:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
#### 2. Complete the following test from PC-B.
- [x]From the command prompt on PC-B, issue the tracert command to the address of PC-A.
```
C:\>tracert 192.168.3.3

Tracing route to 192.168.3.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.4.1
  2   0 ms      0 ms      0 ms      192.168.3.3

Trace complete.
```
