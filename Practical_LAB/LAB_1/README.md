# Практическая работа №1 
## "VLAN и маршрутизация между VLAN"
![](https://github.com/Maksim693/OTUS_LAB/blob/main/Practical_LAB/LAB_1/Pictures_LAB_1/Pict_LAB1)


**Table of Contents**

[ TOCM ]

[ TOC ]

### Build the Network and Configure Basic Device Settings
#### In Part 1, you will set up the network topology and configure basic settings on the PC hosts and switches.
1. Cable the network as shown in the topology.
Attach the devices as shown in the topology diagram, and cable as necessary.
#### Ответ:

2. Configure basic settings for the router.
  -  Console into the router and enable privileged EXEC mode.
b. Enter configuration mode.
c. Assign a device name to the router.
d. Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
e. Assign class as the privileged EXEC encrypted password.
f. Assign cisco as the console password and enable login.
g. Assign cisco as the VTY password and enable login.
h. Encrypt the plaintext passwords.
i. Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
j. Save the running configuration to the startup configuration file.
k. Set the clock on the router.
#### Ответ:

3.Configure basic settings for each switch.
Open configuration window
a. Console into the switch and enable privileged EXEC mode.
b. Enter configuration mode.
c. Assign a device name to the switch.
d. Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as
though they were host names.
e. Assign class as the privileged EXEC encrypted password.
f. Assign cisco as the console password and enable login.
g. Assign cisco as the vty password and enable login.
h. Encrypt the plaintext passwords.
i. Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
j. Set the clock on the switch.
Note: Use the question mark (?) to help with the correct sequence of parameters needed to execute this
command.
k. Copy the running configuration to the startup configuration.
Close configuration window
4.Configure PC hosts.
Refer to the Addressing Table for PC host address information.
2.Create VLANs and Assign Switch Ports
In Part 2, you will create VLANs, as specified in the table above, on both switches. You will then assign the
VLANs to the appropriate interface. The show vlan command is used to verify your configuration settings.
Complete the following tasks on each switch.
1.Create VLANs on both switches.
Open configuration window
a. Create and name the required VLANs on each switch from the table above.
b. Configure the management interface and default gateway on each switch using the IP address
information in the Addressing Table.
c. Assign all unused ports on both switches to the ParkingLot VLAN, configure them for static access mode,
and administratively deactivate them.
© 2013 - 2019 Cisco and/or its affiliates. All rights reserved. Cisco Public Page 3 of 5 www.netacad.com
Lab - Configure Router-on-a-Stick Inter-VLAN Routing
Note: The interface range command is helpful to accomplish this task with as few commands as
necessary.
2.Assign VLANs to the correct switch interfaces.
a. Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for
static access mode. Be sure to do this on both switches
b. Issue the show vlan brief command and verify that the VLANs are assigned to the correct interfaces.
Close configuration window
3.Configure an 802.1Q Trunk Between the Switches
In Part 3, you will manually configure interface F0/1 as a trunk.
1.Manually configure trunk interface F0/1.
Open configuration window
a. Change the switchport mode on interface F0/1 to force trunking. Make sure to do this on both switches.
b. As a part of the trunk configuration, set the native VLAN to 8 on both switches. You may see error
messages temporarily while the two interfaces are configured for different native VLANs.
c. As another part of trunk configuration, specify that VLANs 3, 4, and 8 are only allowed to cross the trunk.
d. Issue the show interfaces trunk command to verify trunking ports, the Native VLAN and allowed VLANs
across the trunk.
2.Manually configure S1’s trunk interface F0/5
a. Configure the F0/5 on S1 with the same trunk parameters as F0/1. This is the trunk to the router.
b. Save the running configuration to the startup configuration file on S1 and S2.
c. Issue the show interfaces trunk command to verify trunking.
Question:
Why does F0/5 not appear in the list of trunks?
Type your answers here.
Close configuration window
4.Configure Inter-VLAN Routing on the Router
Open configuration window
a. Activate interface G0/0/1 on the router.
b. Configure sub-interfaces for each VLAN as specified in the IP addressing table. All sub-interfaces use
802.1Q encapsulation. Ensure the sub-interface for the native VLAN does not have an IP address
assigned. Include a description for each sub-interface.
c. Use the show ip interface brief command to verify the sub-interfaces are operational.
Close configuration window
5.Verify Inter-VLAN Routing is Working
1.Complete the following tests from PC-A. All should be successful.
Note: You may have to disable the PC firewall for pings to be successful.
a. Ping from PC-A to its default gateway.
b. Ping from PC-A to PC-B
c. Ping from PC-A to S2
2. Complete the following test from PC-B.
From the command prompt on PC-B, issue the tracert command to the address of PC-A. 
# END
