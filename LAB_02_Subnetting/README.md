# Hospital Network Subnetting (Cisco Packet Tracer)

## Overview

This project demonstrates how to divide a Class C network into multiple subnets using Cisco Packet Tracer. The objective is to isolate different hospital departments into separate broadcast domains while efficiently utilizing IP addresses.

The original network is:

Network Address: 192.168.1.0/24

The network is subnetted using a /26 subnet mask (255.255.255.192), resulting in four equal-sized subnets.

---

## Objectives

- Practice IPv4 subnetting
- Configure multiple LANs
- Assign IP addresses correctly
- Understand network and broadcast addresses
- Prepare a scalable enterprise network design

---

## Network Information

- Original Network: **192.168.1.0/24**
- Subnet Mask: **255.255.255.192**
- Prefix Length: **/26**
- Number of Subnets: **4**
- Hosts per Subnet: **62 usable hosts**

---

## Department Subnets

| Department | Network | Usable Host Range | Broadcast |
|------------|---------|-------------------|-----------|
| HR | 192.168.1.0/26 | 192.168.1.1 - 192.168.1.62 | 192.168.1.63 |
| IT | 192.168.1.64/26 | 192.168.1.65 - 192.168.1.126 | 192.168.1.127 |
| Patient Accounts | 192.168.1.128/26 | 192.168.1.129 - 192.168.1.190 | 192.168.1.191 |
| Finance | 192.168.1.192/26 | 192.168.1.193 - 192.168.1.254 | 192.168.1.255 |

---

## Topology

The network consists of four departments connected through networking devices in Cisco Packet Tracer.

Each department belongs to its own subnet to reduce broadcast traffic and improve network organization.

---

## Skills Practiced

- IPv4 Addressing
- Subnetting
- Cisco Packet Tracer
- Network Planning
- IP Address Assignment
- Enterprise LAN Design

---

## Technologies Used

- Cisco Packet Tracer
- IPv4
- Ethernet Switching
- Static IP Addressing

---

## Learning Outcomes

After completing this project, I learned how to:

- Divide a /24 network into multiple /26 subnets
- Calculate network, broadcast, and usable host addresses
- Assign devices to the correct subnet
- Design a simple enterprise network topology
- Document networking projects for a professional portfolio

