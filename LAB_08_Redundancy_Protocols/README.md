# First Hop Redundancy Protocol (FHRP) Topology

This repository contains the configuration and documentation for a basic network topology demonstrating First Hop Redundancy Protocols (such as HSRP, VRRP, or GLBP) using Cisco networking equipment.

## Topology Overview

The network is designed to provide high availability for end-user devices. If the primary default gateway fails, the secondary gateway will seamlessly take over traffic routing without requiring changes on the client side.

![Topology Diagram](Topology.png)

### Devices and Connections

The topology consists of the following components:

*   **Upstream Core Router:** 
    *   `Router2` (ISR4321): Acts as the external network or ISP routing traffic from the internal gateways.
*   **Gateway Routers (Redundancy Pair):**
    *   `Router0` (ISR4321): Connected to the LAN via `Gig0/0/0` and to the upstream router via `Gig0/0/1`.
    *   `Router1` (ISR4321): Connected to the LAN via `Gig0/0/0` and to the upstream router via `Gig0/0/1`.
*   **Access Switch:** 
    *   `Switch0` (2960 24TT): Connects the client PCs to both gateway routers.
*   **End Devices:** 
    *   `PC0`, `PC1`, and `PC2` connected to the local area network.

## IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask (Assumed) | Connected To |
| :--- | :--- | :--- | :--- | :--- |
| **Router0** | Gig0/0/0 | 192.168.1.2 | 255.255.255.0 | LAN (Switch0 Fa0/4) |
| | Gig0/0/1 | 10.0.1.1 | 255.255.255.0 | Router2 (Gig0/0/0) |
| **Router1** | Gig0/0/0 | 192.168.1.1 | 255.255.255.0 | LAN (Switch0 Fa0/5) |
| | Gig0/0/1 | 10.0.0.1 | 255.255.255.0 | Router2 (Gig0/0/1) |
| **Router2** | Gig0/0/0 | 10.0.1.2 | 255.255.255.0 | Router0 (Gig0/0/1) |
| | Gig0/0/1 | 10.0.0.2 | 255.255.255.0 | Router1 (Gig0/0/1) |
| **PC0** | Fa0 | 192.168.1.7 | 255.255.255.0 | Switch0 (Fa0/1) |
| **PC1** | Fa0 | 192.168.1.6 | 255.255.255.0 | Switch0 (Fa0/2) |
| **PC2** | Fa0 | 192.168.1.5 | 255.255.255.0 | Switch0 (Fa0/3) |

*Note: In a redundancy setup, a Virtual IP (VIP) address (e.g., `192.168.1.254` or `192.168.1.100`) is typically configured on both Router0 and Router1. This VIP serves as the configured default gateway for PC0, PC1, and PC2.*

## Redundancy Protocol Details

This topology is structured to implement an FHRP. Based on standard Cisco topologies, the implementation details are generally as follows:

1.  **Virtual IP Address:** Both `Router0` and `Router1` share a virtual IP and virtual MAC address on their `Gig0/0/0` interfaces.
2.  **Active/Standby Roles:** One router (e.g., `Router1` with IP `192.168.1.1`) is configured with a higher priority to be the **Active** forwarder. `Router0` acts as the **Standby** forwarder.
3.  **Failover Mechanism:** If the Active router or its LAN connection fails, the Standby router detects the loss of hello packets and assumes the Active role, taking over the Virtual IP address to ensure uninterrupted internet/WAN access for the PCs.

### Testing the Setup

To verify the redundancy protocols:
1. Ping the virtual gateway IP from PC0, PC1, or PC2.
2. Run a continuous ping or traceroute to an external loopback address on `Router2`.
3. Shut down the `Gig0/0/0` interface on the Active Router (Router0 or Router1).
4. Verify that the continuous ping resumes after a brief interruption and that traffic is successfully re-routed through the Standby Router.