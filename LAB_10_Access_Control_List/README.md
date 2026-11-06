# Extended ACL Implementation for ICMP Restriction

## Project Overview
This project demonstrates the configuration and application of an Extended Access Control List (ACL) within a Cisco Packet Tracer simulated network. The primary objective is to restrict specific ICMP (ping) traffic while permitting all other network communications.

Specifically, the ACL is designed to prevent a single device (IP: `172.16.0.2`) from pinging any device within the `10.0.0.0` network.

## Network Topology Reference
Please refer to the network diagram for the exact layout and interface assignments:
**Screenshot 2026-11-06 170737.png**

Based on the provided topology:
*   **Target Device (Source):** PC2 (`172.16.0.2`)
*   **Target Network (Destination):** `10.0.0.0` network (containing PC6, PC7, PC8)
*   **Target Router:** Router0 (Leftmost router acting as the gateway for the `172.16.0.0` network)

## Objective
1. Identify the router closest to the source device to apply the Extended ACL (Best practice for Extended ACLs). In this case, it is Router0.
2. Create an Extended ACL to explicitly deny ICMP echo requests (pings) from host `172.16.0.2` to the `10.0.0.0` network.
3. Permit all other IP traffic to ensure regular network functions are not interrupted.
4. Apply the ACL to the correct router interface in the correct direction (inbound on Fa0/0).

## Configuration Steps

Execute the following commands on **Router0**:

```text
Router> enable
Router# configure terminal

! Create the Extended ACL (Number 100-199)
Router(config)# access-list 100 deny icmp host 172.16.0.2 10.0.0.0 0.255.255.255 echo
Router(config)# access-list 100 permit ip any any

! Apply the ACL to the interface facing the source device
Router(config)# interface FastEthernet0/0
Router(config-if)# ip access-group 100 in
Router(config-if)# end
Router# write memory
```
*(Note: Adjust the wildcard mask `0.255.255.255` to `0.0.0.255` if the 10.0.0.0 network is specifically subnetted as a /24).*

## Verification & Testing
To verify the configuration is working as intended, perform the following tests:

1. **Test from PC2 (172.16.0.2):**
   * Open the command prompt on PC2.
   * Type `ping 10.0.0.2`
   * **Expected Result:** Destination host unreachable or Request timed out (Blocked by ACL).

2. **Test from PC0 or PC1 (e.g., 172.16.0.4):**
   * Open the command prompt on PC0.
   * Type `ping 10.0.0.2`
   * **Expected Result:** Successful replies (Traffic is permitted).

3. **Verify ACL Matches on Router0:**
   * Run `show access-lists` on Router0's privileged EXEC mode to see the match counters incrementing for the denied traffic.