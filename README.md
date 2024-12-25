# Vlan-Trunk

**VLAN and Trunk Configuration Guide**

This guide provides a step-by-step explanation of how to configure VLANs and trunking on switches. Proper configuration ensures devices in the same VLAN can communicate across different switches.

**_1. Hostname and Banner Configuration_**

Example Configuration on Switch1

Switch>enable
Switch#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#banner motd #Cyber Quince#

This sets the hostname to S1 and configures a Message of the Day (MOTD) banner as "Cyber Quince."

**_2. Access Ports and VLAN Configuration_**

Switch1 Configuration

_Interface FastEthernet 0/1:_

Set to access mode.

Assign to VLAN 5.

S1(config)#interface FastEthernet 0/1
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 5
% Access VLAN does not exist. Creating vlan 5
S1(config-if)#exit

_Interface FastEthernet 0/2:_

Set to access mode.

Assign to VLAN 7.

S1(config)#interface FastEthernet 0/2
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 7
% Access VLAN does not exist. Creating vlan 7
S1(config-if)#exit

_Interface GigabitEthernet 0/1:_

Set to trunk mode.

S1(config)#interface GigabitEthernet 0/1
S1(config-if)#switchport mode trunk
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

Switch2 Configuration

_Interface FastEthernet 0/1:_

Set to access mode.

Assign to VLAN 5.

S2(config)#interface FastEthernet 0/1
S2(config-if)#switchport mode access
S2(config-if)#switchport access vlan 5
% Access VLAN does not exist. Creating vlan 5
S2(config-if)#exit

_Interface Range GigabitEthernet 0/1-2:_

Set to trunk mode.

S2(config)#interface range GigabitEthernet 0/1-2
S2(config-if-range)#switchport mode trunk
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

**Switch3 Configuration**

Interface FastEthernet 0/1:

Set to access mode.

Assign to VLAN 7.

S3(config)#interface FastEthernet 0/1
S3(config-if)#switchport mode access
S3(config-if)#switchport access vlan 7
% Access VLAN does not exist. Creating vlan 7
S3(config-if)#exit

Interface FastEthernet 0/2:

Set to access mode.

Assign to VLAN 5.

S3(config)#interface FastEthernet 0/2
S3(config-if)#switchport mode access
S3(config-if)#switchport access vlan 5
% Access VLAN does not exist. Creating vlan 5
S3(config-if)#exit

Interface GigabitEthernet 0/2:

Set to trunk mode.

S3(config)#interface GigabitEthernet 0/2
S3(config-if)#switchport mode trunk

**3. Trunk Configuration**

Trunk ports allow VLAN traffic to pass between switches.

**Switch1**

Configure GigabitEthernet 0/1 as a trunk port:

S1(config)#interface GigabitEthernet 0/1
S1(config-if)#switchport mode trunk
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

**Switch2**

Configure a range of ports (GigabitEthernet 0/1-2) as trunk ports:

S2(config)#interface range GigabitEthernet 0/1-2
S2(config-if-range)#switchport mode trunk
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

**Switch3**

Configure GigabitEthernet 0/2 as a trunk port:

S3(config)#interface GigabitEthernet 0/2
S3(config-if)#switchport mode trunk

_**4. Troubleshooting: Why Can’t PC2 Ping PC4?**_

Problem: PC2 cannot ping PC4, even though both belong to VLAN 7.

Reason: Switch2 does not have VLAN 7 created, so traffic for VLAN 7 is not being forwarded.

Solution: Create VLAN 7 on Switch2.

Steps to Create VLAN 7 on Switch2

S2(config)#vlan 7

This ensures Switch2 is aware of VLAN 7 and can properly forward traffic between devices in this VLAN.

**5. Key Takeaways**

Access Ports: Assign devices to specific VLANs.

Trunk Ports: Allow VLAN traffic between switches.

VLAN Creation: Ensure VLANs exist on all relevant switches.

Troubleshooting: If devices in the same VLAN cannot communicate, check VLAN presence and trunk configurations.
