**Cisco Packet Tracer IP Phone Registration Guide**
-------------------------------------------------------------

This repository contains a guide for setting up and troubleshooting IP Phone Registration with Telephony Services on a Cisco Router using Cisco Packet Tracer. The guide includes steps for configuring VLANs, DHCP, telephony services, and troubleshooting common registration issues.

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

Unregistering IP Phones

Additional Notes

Conclusion


**Introduction**
-----------------------

In this guide, we will walk through the process of configuring IP phone registration on a Cisco router using SCCP protocol in Cisco Packet Tracer. The setup involves configuring VLANs, DHCP, telephony services, and IP phones. It also includes troubleshooting common issues such as phones being stuck at "Configuring CM List" or "Configuring IP".

Prerequisites
Before starting, ensure that you have the following:

Cisco Packet Tracer (version 7.x or higher) make sure to use firewall to block unnecessary sign in and interruptions(Free packet tracer)
Basic understanding of networking (VLANs, IP addressing, DHCP, etc.)
A Cisco router and IP Phones added to the Packet Tracer workspace
A DHCP server (or router configured as DHCP) to assign IP addresses
Step-by-Step Configuration
Step 1: Setting Up VLANs
Create Voice VLAN (VLAN 100):

Open the router and configure the Voice VLAN (VLAN 100).
Make sure that the VLAN is configured on both the router and the switch (if applicable).
Example on the router:

bash
Copy code
Router(config)# vlan 100
Router(config-vlan)# name Voice
Assign VLANs to Switch Ports:

Configure the switch ports where the phones will be connected to ensure they are in the correct access VLAN and voice VLAN.
Example on the switch:

bash
Copy code
Switch(config)# interface range fa0/1 - 2   # Adjust according to your ports
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 100  # Voice VLAN
Switch(config-if-range)# switchport voice vlan 100
Step 2: Configure DHCP for Voice VLAN
Configure DHCP Server:

Set up a DHCP pool for the Voice VLAN with a TFTP server address pointing to the router's IP.
Example on the router:

bash
Copy code
Router(config)# ip dhcp pool Voice_Pool
Router(config-dhcp)# network 192.168.100.0 255.255.255.0
Router(config-dhcp)# default-router 192.168.100.1
Router(config-dhcp)# option 150 ip 192.168.100.1   # TFTP server IP
Configure Lease Time:

Adjust the DHCP lease time to ensure it is long enough (e.g., 24 hours).
bash
Copy code
Router(config-dhcp)# lease 0 24 0  # 24 hours lease
Step 3: Configure Telephony Service on the Router
Enable Telephony Service:

On the router, configure the telephony service and specify the max ephones and max directory numbers.
Example on the router:

bash
Copy code
Router(config)# telephony-service
Router(config-telephony)# max-ephones 5
Router(config-telephony)# max-dn 5
Router(config-telephony)# ip source-address 192.168.100.1  # Router's IP in the Voice VLAN
Verify Telephony Configuration:

Verify that the router is configured to handle telephony services and the IP phones are ready to register.
bash
Copy code
Router# show telephony-service
Step 4: Configure IP Phones
Assign Extensions:
On the IP phones, configure Directory Numbers (DNs) to define their extensions.
Example on Packet Tracer:
Click on the phone, go to the Settings tab, and assign a DN (e.g., 1000, 1001, etc.).
Verify Phone Registration:
Once connected to the network, the phone should attempt to register with the router. Check the router logs or telephony-service status to verify successful registration.
Step 5: Troubleshooting IP Phone Registration
Phone Stuck on "Configuring IP" or "Configuring CM List":

Ensure the TFTP server is properly configured in the DHCP pool (Option 150).
Verify the VLAN configuration on both the switch and the router.
Ensure the DHCP lease time is sufficient, and check if IP conflicts are causing issues.
Registration Issues (e.g., "Phone-Reg-Rej"):

Verify the phone's extension is unique.
Ensure the telephony service on the router is correctly configured (max-ephones, max-dn).
Check the phone’s MAC address is correctly mapped and no conflicts exist in the DHCP binding table.
DHCP Issues:

Verify the DHCP server is providing the correct IP addresses to the phones.
Check the router's IP address in the Voice VLAN and confirm connectivity.
Unregistering IP Phones
To unregister an IP phone, use the following steps:

Remove the Phone from the Network:

In Packet Tracer, simply remove the IP phone from the workspace and reconnect it.
This will reset the phone and cause it to request a new IP address.
Unregister Using CLI (Real Cisco Devices):

On real devices, you can use the following command to unregister a phone from the router:
bash
Copy code
Router(config)# no ephone 1   # Replace '1' with the phone's ephone number
Additional Notes
Packet Tracer may have limitations on telephony services and might not support all the commands and features available in a real Cisco environment.
Make sure VLANs are correctly configured across all devices in the network to ensure proper communication between the router and IP phones.
