# Практическая работа №3
## "Implement DHCPv4"
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
| S1 | MGMT_200 | 192.168.200.2 | 255.255.255.224 | 192.168.200.1 |
- [x] One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
###### Record the first IP address in the Addressing Table for R2 G0/0/1.
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :-:| :----------| :--------------| :---------------| :--:|
| R2 | G0/0/1.100 | 192.168.100.65 | 255.255.255.240 | N/A |
  </details>
  <details>
    <summary> Step 2: Cable the network as shown in the topology. </summary>
    
- [x] Attach the devices as shown in the topology diagram, and cable as necessary.
  </details>      
  <details>
    <summary> Step 3: Configure basic settings for each router. </summary>
    
#### Подзаголовок
- [x] Assign a device name to the router.
- [x] Open configuration window
- [x] Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
- [x] Assign class as the privileged EXEC encrypted password.
- [x] Assign cisco as the console password and enable login.
- [x] Assign cisco as the VTY password and enable login.
- [x] Encrypt the plaintext passwords.
- [x] Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
- [x] Save the running configuration to the startup configuration file.
- [x] Set the clock on the router to today’s time and date.
###### Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this command.
  </details>      
  <details>
    <summary> Step 4: Configure Inter-VLAN Routing on R1 </summary>

#### Подзаголовок  
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

#### Подзаголовок    
- [x] Configure G0/0/1 on R2 with the first IP address of Subnet C you calculated earlier.
```
interface GigabitEthernet0/0/1.100
 description Clients_100
 encapsulation dot1Q 100
 ip address 192.168.100.65 255.255.255.240
```
- [x] Configure interface G0/0/0 for each router based on the IP Addressing table above.
```
interface GigabitEthernet0/0/0
 ip address 10.0.0.2 255.255.255.252
 duplex auto
 speed auto
```
- [x] Configure a default route on each router pointed to the IP address of G0/0/0 on the other router.
```
ip route 0.0.0.0 0.0.0.0 10.0.0.1 
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

> Настройка аналогично 3 шагу.
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
- [x] Close configuration window
- [x] Open configuration window
- [x] Close configuration window
  </details>      
  <details>
    <summary> Step 8: Assign VLANs to the correct switch interfaces. </summary>
    
- [x] Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for static access mode.
- [x] Open configuration window
- [x] Verify that the VLANs are assigned to the correct interfaces.
#### Question:
- Why is interface F0/5 listed under VLAN 1?
>
  </details>      
  <details>
    <summary> Step 9: Manually configure S1’s interface F0/5 as an 802.1Q trunk. </summary>
    
- [x] Change the switchport mode on the interface to force trunking.
- [x] As a part of the trunk configuration, set the native VLAN to 1000.
- [x] As another part of trunk configuration, specify that VLANs 100, 200, and 1000 are allowed to cross the trunk.
- [x] Save the running configuration to the startup configuration file.
- [x] Verify trunking status.
#### Question:
- At this point, what IP address would the PC’s have if they were connected to the network using DHCP?
> Close configuration window
  </details>
</details>

<details>
  <summary> Part 2: Configure and verify two DHCPv4 Servers on R1 </summary>
  
### In Part 2, you will configure and verify a DHCPv4 Server on R1. The DHCPv4 server will service two subnets, Subnet A and Subnet C.   
  <details>
    <summary> Step 1: Configure R1 with DHCPv4 pools for the two supported subnets. Only the DHCP Pool for subnet A is given below </summary>
    
- [x] Exclude the first five useable addresses from each address pool.
- [x] Open configuration window
- [x] Create the DHCP pool (Use a unique name for each pool).
- [x] Specify the network that this DHCP server is supporting.
- [x] Configure the domain name as ccna-lab.com
- [x] Configure the appropriate default gateway for each DHCP pool.
- [x] Configure the lease time for 2 days 12 hours and 30 minutes.
- [x] Next, configure the second DHCPv4 Pool using the pool name R2_Client_LAN and the calculated network, default-router and use the same domain name and lease time from the previous DHCP pool.
  </details>      
  <details>
    <summary> Step 2: Save your configuration </summary>
  
 #### Подзаголовок   
- [x] Save the running configuration to the startup configuration file.
- [x] Close configuration window
  </details>      
  <details>
    <summary> Step 3: Verify the DHCPv4 Server configuration </summary>

#### Подзаголовок
- [x] Issue the command show ip dhcp pool to examine the pool details.
- [x] Issue the command show ip dhcp bindings to examine established DHCP address assignments.
- [x] Issue the command show ip dhcp server statistics to examine DHCP messages.
  </details>      
  <details>
    <summary> Step 4: Attempt to acquire an IP address from DHCP on PC-A </summary>
    
#### Подзаголовок    
- [x] Open a command prompt on PC-A and issue the command ipconfig /renew.
- [x] Once the renewal process is complete, issue the command ipconfig to view the new IP information.
- [x] Test connectivity by pinging R1’s G0/0/1 interface IP address.
  </details>
</details>

<details>
  <summary> Part 3: Configure and verify a DHCP Relay on R2 </summary>
  
### In Part 3, you will configure R2 to relay DHCP requests from the local area network on interface G0/0/1 to the DHCP server (R1).      
  <details>
    <summary> Step 1: Configure R2 as a DHCP relay agent for the LAN on G0/0/1 </summary>
    
#### Подзаголовок    
- [x] Configure the ip helper-address command on G0/0/1 specifying R1’s G0/0/0 IP address.
- [x] Open configuration window
- [x] Save your configuration.
- [x] Close configuration window
  </details>      
  <details>
    <summary> Step 2: Attempt to acquire an IP address from DHCP on PC-B </summary>

#### Подзаголовок    
- [x] Open a command prompt on PC-B and issue the command ipconfig /renew.
- [x] Once the renewal process is complete, issue the command ipconfig to view the new IP information.
- [x] Test connectivity by pinging R1’s G0/0/1 interface IP address.
- [x] Issue the show ip dhcp binding on R1 to verify DHCP bindings.
- [x] Issue the show ip dhcp server statistics on R1 and R2 to verify DHCP messages.
</details>
