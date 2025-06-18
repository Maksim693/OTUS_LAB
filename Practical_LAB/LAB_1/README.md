# Практическая работа №1
## "VLAN и маршрутизация между VLAN"
![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_1/Pictures_LAB_1/Pict_LAB1)

## *Table of Contents*
- [Build the Network and Configure Basic Device Settings](#Build-the-Network-and-Configure-Basic-Device-Settings)
- [Create VLANs and Assign Switch Ports](#Create-VLANs-and-Assign-Switch-Ports)
- [Configure an 802.1Q Trunk Between the Switches](#configure-an-8021q-trunk-between-the-switches)
- [Configure Inter-VLAN Routing on the Router](#Configure-Inter-VLAN-Routing-on-the-Router)

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
```
R1(config)#clock timezone msk 3

```
###### Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this command.
### 3. Configure basic settings for each switch.
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
>Ответ:
```
Блок кода
```
### 4. Configure PC hosts.
- [x] Refer to the Addressing Table for PC host address information.
>Ответ:
```
Блок кода
```
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
#### 2. Manually configure S1’s trunk interface F0/5
- [x] Configure the F0/5 on S1 with the same trunk parameters as F0/1. This is the trunk to the router.
- [x] Save the running configuration to the startup configuration file on S1 and S2.
- [x] Issue the show interfaces trunk command to verify trunking.
#### Question:
- Why does F0/5 not appear in the list of trunks?
- Type your answers here.
- Close configuration window
> Ответ:

## Configure Inter-VLAN Routing on the Router
- [x] Open configuration window
- [x] Activate interface G0/0/1 on the router.
- [x] Configure sub-interfaces for each VLAN as specified in the IP addressing table. All sub-interfaces use
802.1Q encapsulation. Ensure the sub-interface for the native VLAN does not have an IP address
assigned. Include a description for each sub-interface.
- [x] Use the show ip interface brief command to verify the sub-interfaces are operational.
- [x] Close configuration window
> Ответ:
>> ```Блок кода```
## Verify Inter-VLAN Routing is Working
##### 1. Complete the following tests from PC-A. All should be successful.
###### Note: You may have to disable the PC firewall for pings to be successful.
- [x] Ping from PC-A to its default gateway.
- [x] Ping from PC-A to PC-B
- [x] Ping from PC-A to S2
> Ответ:
##### 2. Complete the following test from PC-B.
- [x]From the command prompt on PC-B, issue the tracert command to the address of PC-A. 
> Ответ:
