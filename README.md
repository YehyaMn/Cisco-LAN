**Cisco Packet Tracer IP Phone Registration Guide**
-------------------------------------------------------------

This repository contains a guide for setting up DCHP server that transmit DHCP request based on VLANs and IP Phone Registration with Telephony Services on a Cisco Router using Cisco Packet Tracer. The guide includes steps for configuring Data and voice VLANs, DHCP server, routing, and telephony services.

![Packet Tracer](https://github.com/user-attachments/assets/e34ef378-4c04-4022-a2d8-8dc37b199d8b)


**Table of Contents**
------------------------
Introduction

Prerequisites

Step-by-Step Configuration

Step 1: Setting Up VLANs

Step 2: Configure DHCP for Voice VLAN

Step 3: Configure Telephony Service on the Router

Step 4: Configure IP Phones

Step 5: Troubleshooting IP Phone Registration

Additional Notes

Conclusion


**Introduction**
-----------------------

In this guide, we will walk through the process of configuring IP phone registration on a Cisco router using CLI. The setup involves configuring VLANs, DHCP server and scopes, telephony services, and IP phones, Trunk ports, Access ports, Sub-intefaces. It also includes troubleshooting common issues such as phones being stuck at "Configuring CM List" or "Configuring IP".

**Prerequisites**
------------------------------------
Before starting, ensure that you have the following:

Cisco Packet Tracer (version 7.x or higher) make sure to use firewall to block unnecessary sign in and interruptions(Anonymous study)
Basic understanding of networking (VLANs, IP addressing, DHCP, etc.)
A Cisco router and IP Phones added to the Packet Tracer workspace
A DHCP server (or router configured as DHCP) to assign IP addresses

**Step-by-Step Configuration**
------------------------------------------
**Step 1: Setting Up VLANs on Switch and Routers
**

configure the VLANs as follows:

Router(config)# vlan 100
Router(config-vlan)# name VOIP

Router(config)# vlan 2
Router(config-vlan)# name Servers

Router(config)# vlan 3
Router(config-vlan)# name Sales

Router(config)# vlan 4
Router(config-vlan)# name IT

Router(config)# vlan 100
Router(config-vlan)# name Voice

interface FastEthernet0/1
 switchport mode trunk
!
interface FastEthernet0/2
 switchport access vlan 4
 switchport mode access
 switchport voice vlan 100
!
interface FastEthernet0/3
 switchport access vlan 4
 switchport mode access
!

**Configure the switch ports where the phones will be connected
**
----------------------------------------------------

Switch(config)# interface range fa0/1 - 2   # Adjust according to your ports

Switch(config-if-range)# switchport mode access

Switch(config-if-range)# switchport access vlan 4  # Data VLAN for IT team, adjust according to your network

Switch(config-if-range)# switchport voice vlan 100

You can create a miscellaneous VLAN for security reasons, and assign range of unused ports, then shut them down



Open the router and configure the Voice VLAN (VLAN 100) and subinterfaces:
Make sure that the VLANs are configured on both the router and the switchs

interface GigabitEthernet0/0/0.100
 no encapsulation dot1Q 100
 no ip address 192.168.100.254 255.255.255.0
 no ip helper-address 192.168.2.100

interface GigabitEthernet0/0/0.2
 no encapsulation dot1Q 2
 no ip address 192.168.2.254 255.255.255.0
 no ip helper-address 192.168.2.100

interface GigabitEthernet0/0/0.3
 no encapsulation dot1Q 3
 no ip address 192.168.3.254 255.255.255.0
 no ip helper-address 192.168.2.100

no ip helper-address command infoms the subnet about the DHCP server relay



**Step 2: Configure interface on telephony server **
-------------------------------------------------------
Configure telephony server with an ip of your preference, in my case I used 172.16.0.1/16 and added a static route on the main router.

Set up a DHCP pool for the Voice VLAN with a TFTP server address pointing to the router's IP 172.16.0.1/16 as shown below:

![Screenshot 2024-12-19 144004](https://github.com/user-attachments/assets/dcd77269-c25e-4ec7-b349-5138a10bfc9b)

**Step 3: Configure Telephony Service on the Router**
---------------------------------------------------------------------
Enable Telephony Service:

On the router, configure the telephony service and specify the max ephones and max directory numbers.

Example on the Telephony router:

Router(config)# telephony-service
Router(config-telephony)# max-ephones 2
Router(config-telephony)# max-dn 2
Router(config-telephony)# ip source-address 172.16.0.1  # Router's IP in the Voice VLAN


**Telephony Configuration:**
---------------------------------

Assign Extensions:

To assign extensions to IP phones (called ephones) and map them to a MAC address on a Cisco router, you will use the telephony-service configuration in Packet Tracer. Router 2811 has this feature

Here’s a step-by-step guide for assigning extensions to IP phones and associating them with MAC addresses.

Steps for Assigning Extensions to IP Phones (ephones)

Router(config-telephony)# ephone 1
Router(config-ephone)# mac-address xxxx.xxxx.xxxx  # Replace with the MAC address of the first IP phone
Router(config-ephone)# button 1:1 # Assign extension 1000 to the first phone

Router(config-telephony)# ephone 2
Router(config-ephone)# mac-address xxxx.xxxx.xxxx  # Replace with the MAC address of the second IP phone
Router(config-ephone)# button 1:2


Remove the Phone from the Network:
-------------------------------------

In Packet Tracer, simply remove the IP phone from the workspace and reconnect it.
This will reset the phone and cause it to request a new IP address.
Unregister Using CLI (Real Cisco Devices):

On real devices, you can use the following command to unregister a phone from the router:

Router(config)# no ephone 1   # Replace '1' with the phone's ephone number

