# Small Office Network Design & Configuration

![Network Topology](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Network%20Desin.png)
> *Complete network design showing Router, Switches, Firewall, and End Devices.*


## Project Overview
This project demonstrates the design and configuration of a **secure small office network** connecting three departments (**HR**, **IT**, and **Sales**) with shared services including a **Core Server**, **Network Printer**, and **Firewall** for Internet security.  
The goal is to implement a scalable, segmented, and secure LAN with centralized routing, DHCP, and VLANs using Cisco Packet Tracer.


##  Network Devices Used
| Device | Model | Function |
|---------|--------|-----------|
| Router | Cisco 1941 | Inter-VLAN Routing (Router-on-a-Stick) |
| Core Switch | Cisco 2960 | Central Distribution Switch |
| Department Switches | Cisco 2960 | Access Layer for each department |
| Server | Generic | Combined DHCP + Web Server |
| Printer | Network Printer | Shared Office Printer |
| Firewall | Cisco ASA 5506-X | Network Security and NAT |
| PCs | Generic | Department End Devices |


## Network Topology Layers
- **Perimeter Layer:** Firewall → Router  
- **Distribution Layer:** Core Switch (Trunked to Router and Department Switches)  
- **Access Layer:** Department Switches (HR, IT, Sales)  
- **Service Layer:** Core Server + Printer  


##  VLAN & IP Scheme
| VLAN | Department | Subnet | Gateway |
|------|-------------|---------|----------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Sales | 192.168.30.0/24 | 192.168.30.1 |
| 99 | Management (Server/Printer) | 192.168.99.0/24 | 192.168.99.1 |


## Router Configuration (Cisco 1941)
```
enable
configure terminal
interface GigabitEthernet0/0
 no shutdown
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
!
interface GigabitEthernet0/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.1 255.255.255.0
!
ip routing
end
write memory
```

## Core Switch Configuration
```
enable
configure terminal
vlan 10
 name HR
vlan 20
 name IT
vlan 30
 name Sales
vlan 99
 name Management
!
interface GigabitEthernet0/1
 switchport mode trunk
 no shutdown
!
interface range GigabitEthernet0/2 - 4
 switchport mode trunk
 no shutdown
!
interface vlan 99
 ip address 192.168.99.2 255.255.255.0
 no shutdown
end
write memory
```

## Department Switch Configuration (Example: HR Switch)
```
enable
configure terminal
vlan 10
 name HR
!
interface GigabitEthernet0/1
 switchport mode trunk
!
interface range FastEthernet0/2 - 10
 switchport mode access
 switchport access vlan 10
 no shutdown
end
write memory
```
Repeat the same configuration for IT (VLAN 20) and Sales (VLAN 30).

## Server Configuration

Server IP: 192.168.99.10

Services Enabled: DHCP, Web

DHCP Pools:

VLAN 10 → 192.168.10.0/24 → Gateway 192.168.10.1

VLAN 20 → 192.168.20.0/24 → Gateway 192.168.20.1

VLAN 30 → 192.168.30.0/24 → Gateway 192.168.30.1

## Printer Configuration

IP Address: 192.168.99.20

Subnet Mask: 255.255.255.0

Gateway: 192.168.99.1

Connected to Core Switch (VLAN 99) for all users to access.

## Firewall Configuration (Cisco ASA 5506-X)
```
enable
configure terminal
interface GigabitEthernet0/0
 nameif inside
 security-level 100
 ip address 192.168.1.1 255.255.255.0
!
interface GigabitEthernet0/1
 nameif outside
 security-level 0
 ip address 203.0.113.2 255.255.255.0
!
object network LAN
 subnet 192.168.0.0 255.255.0.0
 nat (inside,outside) dynamic interface
end
write memory
```
## Testing Connectivity
```
From HR PC:
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.99.10
```

## Screenshots

Network Topology	
![Network Topology](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Network%20Desin.png)

VLAN Configuration
![VLAN Configuration](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/CoreSwitch_Vlans.png)

Router Subinterfaces	
![Router Subinterfaces](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Router_Configuration1.png)
![Router Subinterfaces](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Router_Configuration2.png)


DHCP & Server Setup	
![DHCP & Server Setup	](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Server_Ip_Configuration.png)
![DHCP & Server Setup	](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Creating_DHCP_Pools.png)

Successful Ping Tests
![Successful Ping Tests](https://github.com/Binali2142/Small-Office-Network-Design-Configuration/blob/main/Screenshots/Test_connectivity.png)


## Step-by-Step Setup Guide

## Step 1 Open the Project
- Launch Cisco Packet Tracer
- Open the file net.pkt

## Step 2️ Configure VLANs
- Create VLANs 10, 20, 30, and 99 on the Core Switch.
- Trunk the connection between the Router and Core Switch.

## Step 3️ Configure Router-on-a-Stick
- Create subinterfaces for each VLAN on the Router.
- Assign correct IP gateways.

## Step 4️ Enable DHCP on the Server
- Set static IP: 192.168.99.10
- Enable DHCP and Web services.
- Configure pools for VLAN 10, 20, and 30.

## Step 5️ Connect and Test
- Verify all PCs receive IPs automatically.
- Ping between VLANs and to the server.
- Verify Internet access through the firewall (if configured).

## Connect with Me
**GitHub:** [https://github.com/Binali2142]  
**LinkedIn:** [www.linkedin.com/in/qasim-ali-mahamed-107069217]  

