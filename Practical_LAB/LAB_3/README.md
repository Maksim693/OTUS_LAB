# Практическая работа №3
## "Implement DHCPv4"
- Техническое задание можно посмотреть в файле - ["7.4.2 Lab - Implement DHCPv4"](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_3/7.4.2%20Lab%20-%20Implement%20DHCPv4.pdf)
<details>
  <summary> Part 1: Build the Network and Configure Basic Device Settings </summary>
  
### In Part 1, you will set up the network topology and configure basic settings on the PC hosts and switches.
  <details>
    <summary> Step 1: Establish an addressing scheme </summary>
    
##### Subnet the network 192.168.1.0/24 to meet the following requirements:
- [x] One subnet, “Subnet A”, supporting 58 hosts (the client VLAN at R1).
###### Record the first IP address in the Addressing Table for R1 G0/0/1.100.
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :-:| :----------| :-------------| :---------------| :--:|
| R1 | G0/0/1.100 | 192.168.100.1 | 255.255.255.192 | N/A |
- [x] One subnet, “Subnet B”, supporting 28 hosts (the management VLAN at R1). 
###### Record the first IP address in the Addressing Table for R1 G0/0/1.200. Record the second IP address in the Address Table for S1 VLAN 200 and enter the associated default gateway.
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :-:| :----------| :-------------| :---------------| :----------:|
| R1 | G0/0/1.200 | 192.168.200.1 | 255.255.255.224 | N/A |
| S1 | MGMT_200   | 192.168.200.2 | 255.255.255.224 | 192.168.200.1 |
- [x] One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
###### Record the first IP address in the Addressing Table for R2 G0/0/1.
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :-:| :------| :--------------| :---------------| :--:|
| R2 | G0/0/1 | 192.168.100.65 | 255.255.255.240 | N/A |
  </details>
  <details>
    <summary> Step 2: Cable the network as shown in the topology. </summary>

  ##### На данном шаге проводи подключение оборудования согласно приложенной схеме
- [x] Attach the devices as shown in the topology diagram, and cable as necessary.
  </details>      
  <details>
    <summary> Step 3: Configure basic settings for each router. </summary>
    
##### Проводим базовую настройку оборудования
- [x] Assign a device name to the router.
```
hostname R1
```
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
```
no ip domain-lookup
```
- [x] Assign class as the privileged EXEC encrypted password.
```
enable secret class
```
- [x] Assign cisco as the console password and enable login.
```
line console 0
password cisco
login
```
- [x] Assign cisco as the VTY password and enable login.
```
line vty 0 4
password cisco
login
```
- [x] Encrypt the plaintext passwords.
```
service password-encryption 
```
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
```
banner motd $ Authorized Users Only! $
```
- [x] Save the running configuration to the startup configuration file.
```
copy running-config startup-config 
```
- [x] Set the clock on the router.
>Непонятно как настроить время в CPT, кроме timezone и настройки с помощью NTP вариантов нет. 
>Первый вариант приложил команды для "ручной" настройки времени на Сisco 
```
clock timezone msk 3
clock set 10:30:00 18 june 2025
```
>Второй вариант это настроить NTP.
```
clock timezone msk 3
ntp server 10.10.10.10
```
###### Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this command.
  </details>      
  <details>
    <summary> Step 4: Configure Inter-VLAN Routing on R1 </summary>

##### Проводим настройка оборудования согласно 4 шагу  
- [x] Activate interface G0/0/1 on the router.
- [x] Configure sub-interfaces for each VLAN as required by the IP addressing table. All sub-interfaces use 802.1Q encapsulation and are assigned the first usable address from the IP address pool you have calculated. Ensure the sub-interface for the native VLAN does not have an IP address assigned. Include a description for each sub-interface.
- [x] Verify the sub-interfaces are operational.
```
interface GigabitEthernet0/0/1.100
 description Clients_100
 encapsulation dot1Q 100
 ip address 192.168.100.1 255.255.255.192
!
interface GigabitEthernet0/0/1.200
 description MGMT_200
 encapsulation dot1Q 200
 ip address 192.168.200.1 255.255.255.224
!
interface GigabitEthernet0/0/1.999
 description Parking_Lot_999
 encapsulation dot1Q 999
!
interface GigabitEthernet0/0/1.1000
 description Native_1000
 encapsulation dot1Q 1000
```
  </details>      
  <details>
    <summary> Step 5: Configure G0/0/1 on R2, then G0/0/0 and static routing for both routers </summary>

##### Проводим настройка оборуджования согласно 5 шагу
- [x] Configure G0/0/1 on R2 with the first IP address of Subnet C you calculated earlier.
```
interface GigabitEthernet0/0/1
 ip address 192.168.100.65 255.255.255.240
```
- [x] Configure interface G0/0/0 for each router based on the IP Addressing table above.
```
R1(config)#interface GigabitEthernet0/0/0
R1(config-if)#ip address 10.0.0.1 255.255.255.252
```
```
R2(config)#interface GigabitEthernet0/0/0
R2(config-if)#ip address 10.0.0.2 255.255.255.252
```
- [x] Configure a default route on each router pointed to the IP address of G0/0/0 on the other router.
```
R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1
```
- [x] Verify static routing is working by pinging R2’s G0/0/1 address from R1.
```
R2#ping 192.168.100.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.100.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
- [x] Save the running configuration to the startup configuration file.
```
R2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
  </details>      
  <details>
    <summary> Step 6: Configure basic settings for each switch. </summary>

##### Проводим настройку оборудования аналогично [Шагу №3](#проводим-базовую-настройку-оборудования)
- [x] Assign a device name to the switch.
- [x] Open configuration window
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
- [x] Assign class as the privileged EXEC encrypted password.
- [x] Assign cisco as the console password and enable login.
- [x] Assign cisco as the VTY password and enable login.
- [x] Encrypt the plaintext passwords.
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
- [x] Save the running configuration to the startup configuration file.
- [x] Set the clock on the switch to today’s time and date.
###### Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this command.
- [x] Copy the running configuration to the startup configuration.
  </details>      
  <details>
    <summary> Step 7: Create VLANs on S1. </summary>
   
##### Проводим настройку оборудования согласно 7 шагу
###### Note: S2 is only configured with basic settings.
- [x] Create and name the required VLANs on switch 1 from the table above.
```
S1#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/5       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/5       100,200,999-1000
```
```
S1show vlan 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
100  Clients_100                      active    Fa0/6
200  MGMT_200                         active    
999  Parking_Lot_999                  active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
```
- [x] Configure and activate the management interface on S1 (VLAN 200) using the second IP address from the subnet calculated earlier. Additionally, set the default gateway on S1.
```
interface Vlan200
 ip address 192.168.200.2 255.255.255.224
```
- [x] Configure and activate the management interface on S2 (VLAN 1) using the second IP address from the subnet calculated earlier. Additionally, set the default gateway on S2
```
S1(config)#interface range Fa0/1-4,Fa0/7-24,Gi0/1-2
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown
```
- [x] Assign all unused ports on S1 to the Parking_Lot VLAN, configure them for static access mode, and administratively deactivate them. On S2, administratively deactivate all the unused ports.
###### Note: The interface range command is helpful to accomplish this task with as few commands as necessary.
```
S1(config)#interface range Fa0/1-4,Fa0/7-24,Gi0/1-2
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown
```
  </details>      
  <details>
    <summary> Step 8: Assign VLANs to the correct switch interfaces. </summary>
    
#### Настройка клиентского влана    
- [x] Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for static access mode.
```
interface FastEthernet0/6
 switchport access vlan 100
 switchport mode access
```
- [x] Verify that the VLANs are assigned to the correct interfaces.
```
S1#sh vlan brief 
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
100  Clients_100                      active    Fa0/6
200  MGMT_200                         active    
999  Parking_Lot_999                  active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
```
#### Question:
- Why is interface F0/5 listed under VLAN 1?
> В конфигурации выше не видно, чтобы 5 порт находился в 1 влане, т.к. я уже настроил влан-транк на данном порту и изменил Native vlan. 5 порт находится в 1 влане до перевода в режим 'mode trunk'
  </details>      
  <details>
    <summary> Step 9: Manually configure S1’s interface F0/5 as an 802.1Q trunk. </summary>

##### Проводим настройку оборудования согласно 9 шагу
- [x] Change the switchport mode on the interface to force trunking.
- [x] As a part of the trunk configuration, set the native VLAN to 1000.
- [x] As another part of trunk configuration, specify that VLANs 100, 200, and 1000 are allowed to cross the trunk.
- [x] Save the running configuration to the startup configuration file.
- [x] Verify trunking status.
```
interface FastEthernet0/5
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 100,200,1000
 switchport mode trunk
```
```
S1#show interfaces trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/5       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/5       100,200,1000

Port        Vlans allowed and active in management domain
Fa0/5       100,200,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/5       100,200,1000
```
#### Question:
- At this point, what IP address would the PC’s have if they were connected to the network using DHCP?
> Оба PC получают ip из пула 169.254.0.0 255.255.0.0
  </details>
</details>

<details>
  <summary> Part 2: Configure and verify two DHCPv4 Servers on R1 </summary>
  
### In Part 2, you will configure and verify a DHCPv4 Server on R1. The DHCPv4 server will service two subnets, Subnet A and Subnet C.   
  <details>
    <summary> Step 1: Configure R1 with DHCPv4 pools for the two supported subnets. Only the DHCP Pool for subnet A is given below </summary>

#### Приступаем к настройке DHCP сервера
- [x] Exclude the first five useable addresses from each address pool.
```
R1(config)#ip dhcp excluded-address 192.168.100.1 192.168.100.5
```
- [x] Create the DHCP pool (Use a unique name for each pool).
```
R1(config)#ip dhcp pool Client_DHCP_R1
```
- [x] Specify the network that this DHCP server is supporting.
```
R1(dhcp-config)#network 192.168.100.0 255.255.255.192
```
- [x] Configure the domain name as ccna-lab.com
```
R1(dhcp-config)#domain-name ccna-lab.comip d  
```
- [x] Configure the appropriate default gateway for each DHCP pool.
```
R1(dhcp-config)#default-router 192.168.100.1
```
- [x] Configure the lease time for 2 days 12 hours and 30 minutes.
```
lease 2 12 30
```
- [x] Next, configure the second DHCPv4 Pool using the pool name R2_Client_LAN and the calculated network, default-router and use the same domain name and lease time from the previous DHCP pool.
```
R1(config)# ip dhcp excluded-address 192.168.100.65 192.168.100.69
R1(config)# ip dhcp pool R2_Client_LAN
R1(dhcp-config)# network 192.168.100.64 255.255.255.240
R1(dhcp-config)# default-router 192.168.100.65
R1(dhcp-config)# domain-name ccna-lab.com
R1(dhcp-config)# lease 2 12 30
```
  </details>      
  <details>
    <summary> Step 2: Save your configuration </summary>
  
##### Сохраняем настройки
- [x] Save the running configuration to the startup configuration file.
```
copy running-config startup-config 
```
  </details>      
  <details>
    <summary> Step 3: Verify the DHCPv4 Server configuration </summary>

#### Приступаем к настройке DHCP сервера
- [x] Issue the command show ip dhcp pool to examine the pool details.
```
R1#show ip dhcp pool 

Pool Client_DHCP_R1 :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 62
 Leased addresses               : 1
 Excluded addresses             : 2
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.100.1        192.168.100.1    - 192.168.100.62    1    / 2     / 62

Pool R2_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 14
 Leased addresses               : 0
 Excluded addresses             : 2
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.100.65       192.168.100.65   - 192.168.100.78    0    / 2     / 14
```
- [x] Issue the command show ip dhcp bindings to examine established DHCP address assignments.
```
R1# show ip dhcp binding 
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.100.6    0001.9703.86A8           --                     Automatic
```
- [x] Issue the command show ip dhcp server statistics to examine DHCP messages.
> Данная команда не работает на маршрутизаторе 4331 в CPT
  </details>      
  <details>
    <summary> Step 4: Attempt to acquire an IP address from DHCP on PC-A </summary>
    
##### Проводим проверки согласно 4 шагу    
- [x] Open a command prompt on PC-A and issue the command ipconfig /renew.
![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_3/Pictures_LAB_3/Pict_LAB3_PC-A.png)
- [x] Once the renewal process is complete, issue the command ipconfig to view the new IP information.
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::201:97FF:FE03:86A8
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.100.6
   Subnet Mask.....................: 255.255.255.192
   Default Gateway.................: ::
                                     0.0.0.0
```
- [x] Test connectivity by pinging R1’s G0/0/1 interface IP address.
```
C:\>ping 192.168.100.1

Pinging 192.168.100.1 with 32 bytes of data:

Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.100.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
  </details>
</details>

<details>
  <summary> Part 3: Configure and verify a DHCP Relay on R2 </summary>
  
### In Part 3, you will configure R2 to relay DHCP requests from the local area network on interface G0/0/1 to the DHCP server (R1).      
  <details>
    <summary> Step 1: Configure R2 as a DHCP relay agent for the LAN on G0/0/1 </summary>
    
#### Переходим к настройки DHCP Relay    
- [x] Configure the ip helper-address command on G0/0/1 specifying R1’s G0/0/0 IP address.
```
R2(config)# interface g0/0/1
R2(config-if)# ip helper-address 10.0.0.1
```
- [x] Save your configuration.
  </details>      
  <details>
    <summary> Step 2: Attempt to acquire an IP address from DHCP on PC-B </summary>

##### Проводим проверки согласно 2 шагу    
- [x] Open a command prompt on PC-B and issue the command ipconfig /renew.
![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_3/Pictures_LAB_3/Pict_LAB3_PC-B.png)
- [x] Once the renewal process is complete, issue the command ipconfig to view the new IP information.
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: ccna-lab.com
   Link-local IPv6 Address.........: FE80::290:21FF:FE4E:9A4C
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.100.70
   Subnet Mask.....................: 255.255.255.240
   Default Gateway.................: ::
                                     192.168.100.65
```
- [x] Test connectivity by pinging R1’s G0/0/1 interface IP address.
```
C:\>ping 192.168.100.1

Pinging 192.168.100.1 with 32 bytes of data:

Reply from 192.168.100.1: bytes=32 time<1ms TTL=254
Reply from 192.168.100.1: bytes=32 time<1ms TTL=254
Reply from 192.168.100.1: bytes=32 time<1ms TTL=254
Reply from 192.168.100.1: bytes=32 time=4ms TTL=254

Ping statistics for 192.168.100.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 4ms, Average = 1ms
```
- [x] Issue the show ip dhcp binding on R1 to verify DHCP bindings.
```
R1#sh ip dhcp binding 
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.100.6    0001.9703.86A8           --                     Automatic
192.168.100.70   0090.214E.9A4C           --                     Automatic
```
- [x] Issue the show ip dhcp server statistics on R1 and R2 to verify DHCP messages.
> Данная команда не работает на маршрутизаторе 4331 в CPT
  </details>
</details>

## "Configure DHCPv6"
- Техническое задание можно посмотреть в файле - ["8.5.1 Lab - Configure DHCPv6"](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_3/8.5.1%20Lab%20-%20Configure%20DHCPv6.pdf)
<details>
  <summary> Part 1: Build the Network and Configure Basic Device Settings </summary>
  
### In Part 1, you will set up the network topology and configure basic settings on the PC hosts and switches.
  <details>
    <summary> Step 1: Cable the network as shown in the topology. </summary>

##### На данном шаге проводи подключение оборудования согласно приложенной схеме
- [x] Attach the devices as shown in the topology diagram, and cable as necessary.
  </details>      
  <details>
    <summary> Step 2: Configure basic settings for each switch. (Optional) </summary>

##### Проводим настройку оборудования согласно 2 шагу 
- [x] Assign a device name to the router.
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
- [x] Assign class as the privileged EXEC encrypted password.
- [x] Assign cisco as the console password and enable login.
- [x] Assign cisco as the VTY password and enable login.
- [x] Encrypt the plaintext passwords.
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
- [x] Shutdown all unused ports
- [x] Save the running configuration to the startup configuration file
  </details>
  <details>
    <summary> Step 3: Configure basic settings for each router </summary>
    
##### Проводим настройку оборудования согласно 3 шагу
- [x] Assign a device name to the router.
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
- [x] Assign class as the privileged EXEC encrypted password.
- [x] Assign cisco as the console password and enable login.
- [x] Assign cisco as the VTY password and enable login.
- [x] Encrypt the plaintext passwords.
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
- [x] Enable IPv6 Routing
> Из важных моментов отмечу только данный пункт. Все остальные настраиваются по стандрату.
```

```
- [x] Save the running configuration to the startup configuration file
  </details>
  <details>
    <summary> Step 4: Configure interfaces and routing for both routers </summary>

##### Проводим настройку оборудования согласно 4 шагу
- [x] Configure the G0/0/0 and G0/0/1 interfaces on R1 and R2 with the IPv6 addresses specified in the table above.
```

```
- [x] Configure a default route on each router pointed to the IP address of G0/0/0 on the other router.
```

```
- [x] Verify routing is working by pinging R2’s G0/0/1 address from R1
```

```
- [x] Save the running configuration to the startup configuration file.
  </details>
</details>

<details>
  <summary>Part 2: Verify SLAAC Address Assignment from R1</summary>
  
### In Part 2, you will verify that Host PC-A receives an IPv6 address using the SLAAC method.
##### Power PC-A up and ensure that the NIC is configured for IPv6 automatic configuration.
##### After a few moments, the results of the command ipconfig should show that PC-A has assigned itself an address from the 2001:db8:1::/64 network.
#### Question:
- Where did the host-id portion of the address come from?
>
</details>
<details>
  <summary>Part 3: Configure and Verify a DHCPv6 server on R1</summary>

### In Part 3, you will configure and verify a stateless DHCP server on R1. The objective is to provide PC-A with DNS server and Domain information.

  <details>
    <summary>Step 1: Examine the configuration of PC-A in more detail</summary>

##### Проводим настройку оборудования согласно 1 шагу
- [x] Issue the command ipconfig /all on PC-A and take a look at the output.
- [x] Notice that there is no Primary DNS suffix. Also note that the DNS server addresses provided are “site local anycast” addresses, and not unicast addresses, as would be expected.
  </details>
  <details>
    <summary>Step 2: Configure R1 to provide stateless DHCPv6 for PC-A</summary>

 ##### Проводим настройку оборудования согласно 2 шагу
- [x] Create an IPv6 DHCP pool on R1 named R1-STATELESS. As a part of that pool, assign the DNS server address as 2001:db8:acad::1 and the domain name as stateless.com.
- [x] Configure the G0/0/1 interface on R1 to provide the OTHER config flag to the R1 LAN, and specify the DHCP pool you just created as the DHCP resource for this interface.
- [x] Save the running configuration to the startup configuration file.
- [x] Restart PC-A.
- [x] Examine the output of ipconfig /all and notice the changes.
- [x] Test connectivity by pinging R2’s G0/0/1 interface IP address.
  </details>
</details>
<details>
  <summary>Part 4: Configure a stateful DHCPv6 server on R1</summary>

### In Part 4, you will configure R1 to respond to DHCPv6 requests from the LAN on R2.

- [x] Create a DHCPv6 pool on R1 for the 2001:db8:acad:3:aaaa::/80 network. This will provide addresses to the LAN connected to interface G0/0/1 on R2. As a part of the pool, set the DNS server to 2001:db8:acad::254, and set the domain name to STATEFUL.com.
- [x] Assign the DHCPv6 pool you just created to interface g0/0/0 on R1.
</details>
<details>
  <summary>Part 5: Configure and verify DHCPv6 relay on R2</summary>

### In Part 5, you will configure and verify DHCPv6 relay on R2, allowing PC-B to receive an IPv6 Address.

  <details>
    <summary>Step 1: Power on PC-B and examine the SLAAC address that it generates</summary>

##### Проводим настройку оборудования согласно 1 шагу
  </details>    
  <details>
    <summary>Step 2: Configure R2 as a DHCP relay agent for the LAN on G0/0/1</summary>

##### Проводим настройку оборудования согласно 2 шагу    
- [x] Configure the ipv6 dhcp relay command on R2 interface G0/0/1, specifying the destination address of the G0/0/0 interface on R1. Also configure the managed-config-flag command.
- [x] Save your configuration.
  </details>
  <details>
    <summary>Step 3: Attempt to acquire an IPv6 address from DHCPv6 on PC-B</summary>

##### Проводим настройку оборудования согласно 3 шагу
- [x] Restart PC-B.
- [x] Open a command prompt on PC-B and issue the command ipconfig /all and examine the output to see the results of the DHCPv6 relay operation.
- [x] Test connectivity by pinging R1’s G0/0/1 interface IP address.
  </details>
</details>
