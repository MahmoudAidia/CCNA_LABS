# OSPF Multi-Router Ring & Dual-LAN Topology

## 📌 Project Overview

This project demonstrates the implementation of **OSPF (Open Shortest Path First)** dynamic routing across a 5-router ring topology in Cisco Packet Tracer. The network interconnects two distinct Local Area Networks (`200.0.0.0/24` and `192.168.1.0/24`) via redundant paths:

* **Primary Upper Path (2 Hops):** `Router0` $\rightarrow$ `Top Router (Router4)` $\rightarrow$ `Router3`

* **Backup Lower Path (3 Hops):** `Router0` $\rightarrow$ `Router1` $\rightarrow$ `Router2` $\rightarrow$ `Router3`

Because OSPF uses cost (based on interface bandwidth) as its metric, traffic between the two LANs will automatically prefer the shortest/highest-bandwidth path and dynamically fail over if a link goes down.

## 🗺️ Network Subnets Summary

| Network Address | Subnet Mask | Wildcard Mask | Description | 
| ----- | ----- | ----- | ----- | 
| `200.0.0.0/24` | `255.255.255.0` | `0.0.0.255` | Left LAN (Switch0 - PC0, PC1, PC2) | 
| `192.168.1.0/24` | `255.255.255.0` | `0.0.0.255` | Right LAN (Switch1 - PC3, PC4, PC5) | 
| `10.0.0.0/8` | `255.0.0.0` | `0.255.255.255` | Point-to-Point: `Router0` $\leftrightarrow$ `Top Router` | 
| `11.0.0.0/8` | `255.0.0.0` | `0.255.255.255` | Point-to-Point: `Top Router` $\leftrightarrow$ `Router3` | 
| `12.0.0.0/8` | `255.0.0.0` | `0.255.255.255` | Point-to-Point: `Router3` $\leftrightarrow$ `Router2` | 
| `13.0.0.0/8` | `255.0.0.0` | `0.255.255.255` | Point-to-Point: `Router2` $\leftrightarrow$ `Router1` | 
| `14.0.0.0/8` | `255.0.0.0` | `0.255.255.255` | Point-to-Point: `Router1` $\leftrightarrow$ `Router0` | 

> **Note:** If you configured your router-to-router links with `/24` (`255.255.255.0`) or `/30` (`255.255.255.252`) masks instead of default Class A (`/8`) masks, adjust the subnet and wildcard masks accordingly.

## 📋 IP Addressing Table

### Routers

| Device | Interface | IP Address | Subnet Mask | Connected To | 
| ----- | ----- | ----- | ----- | ----- | 
| **Router0** (Left) | `FastEthernet0/0` | `200.0.0.1` | `255.255.255.0` | `Switch0` (`Fa0/4`) | 
|  | `FastEthernet0/1` | `10.0.0.2` | `255.0.0.0` | `Top Router` (`Fa0/1`) | 
|  | `Ethernet0/1/0` | `14.0.0.2` | `255.0.0.0` | `Router1` (`Fa0/0`) | 
| **Top Router** (Apex) | `FastEthernet0/1` | `10.0.0.1` | `255.0.0.0` | `Router0` (`Fa0/1`) | 
|  | `FastEthernet0/0` | `11.0.0.2` | `255.0.0.0` | `Router3` (`Fa0/0`) | 
| **Router3** (Right) | `FastEthernet0/1` | `192.168.1.1` | `255.255.255.0` | `Switch1` (`Fa0/4`) | 
|  | `FastEthernet0/0` | `11.0.0.1` | `255.0.0.0` | `Top Router` (`Fa0/0`) | 
|  | `Ethernet0/1/0` | `12.0.0.1` | `255.0.0.0` | `Router2` (`Fa0/1`) | 
| **Router2** (Bottom-Right) | `FastEthernet0/1` | `12.0.0.2` | `255.0.0.0` | `Router3` (`Eth0/1/0`) | 
|  | `FastEthernet0/0` | `13.0.0.1` | `255.0.0.0` | `Router1` (`Fa0/1`) | 
| **Router1** (Bottom-Left) | `FastEthernet0/1` | `13.0.0.2` | `255.0.0.0` | `Router2` (`Fa0/0`) | 
|  | `FastEthernet0/0` | `14.0.0.1` | `255.0.0.0` | `Router0` (`Eth0/1/0`) | 

### End Devices (PCs)

| Device | IP Address | Subnet Mask | Default Gateway | Switch Port | 
| ----- | ----- | ----- | ----- | ----- | 
| **PC0** | `200.0.0.4` | `255.255.255.0` | `200.0.0.1` | `Switch0 - Fa0/1` | 
| **PC1** | `200.0.0.3` | `255.255.255.0` | `200.0.0.1` | `Switch0 - Fa0/2` | 
| **PC2** | `200.0.0.2` | `255.255.255.0` | `200.0.0.1` | `Switch0 - Fa0/3` | 
| **PC5** | `192.168.1.2` | `255.255.255.0` | `192.168.1.1` | `Switch1 - Fa0/1` | 
| **PC4** | `192.168.1.3` | `255.255.255.0` | `192.168.1.1` | `Switch1 - Fa0/2` | 
| **PC3** | `192.168.1.4` | `255.255.255.0` | `192.168.1.1` | `Switch1 - Fa0/3` | 

## 🔍 Verification & Testing Commands

After configuring OSPF on the routers, run the following commands to verify operation:

1. **Verify Active Routing Protocol:**

   ```
   Router# show ip protocols
   ```

   *Confirms `ospf` is active and lists all advertised networks.*

2. **Check OSPF Neighbor Adjacencies:**

   ```
   Router# show ip ospf neighbor
   ```

   *Ensure all neighbor states reach `FULL`.*

3. **Inspect OSPF Learned Routes in the Routing Table:**

   ```
   Router# show ip route ospf
   ```

   *Displays all remote subnets learned via OSPF (marked with code `O`).*

4. **Test End-to-End Connectivity & Failover:**

   * Open the Command Prompt on **PC0** (`200.0.0.4`) and ping **PC3** (`192.168.1.4`):

     ```
     C:\> ping 192.168.1.4
     ```

   * Trace the active path taken by packets:

     ```
     C:\> tracert 192.168.1.4
     ```

     *By default, traffic travels via the upper path (`200.0.0.1` $\rightarrow$ `10.0.0.1` $\rightarrow$ `11.0.0.1` $\rightarrow$ `192.168.1.4`) because it has a lower total OSPF cost than the lower path.*