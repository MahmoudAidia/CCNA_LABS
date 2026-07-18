# Basic Router Configurations (DHCP) Lab

A Cisco Packet Tracer lab that demonstrates how to configure a Cisco router to provide **Dynamic Host Configuration Protocol (DHCP)** services for clients on a local network. This lab covers the complete router setup, interface configuration, DHCP pool creation, and client connectivity verification.

---

## 📌 Objectives

- Configure a Cisco router from scratch.
- Assign IPv4 addresses to router interfaces.
- Configure the router as a DHCP server.
- Create a DHCP pool for client devices.
- Exclude reserved IP addresses.
- Verify DHCP lease assignments.
- Test network connectivity.

---

## 🛠️ Technologies Used

- Cisco Packet Tracer
- Cisco IOS CLI
- DHCP
- IPv4 Networking

---

## 📚 Concepts Covered

- Initial Router Configuration
- Interface Configuration
- IPv4 Addressing
- DHCP configuration
- DHCP Address Exclusion
- DHCP Pool Configuration
- Default Gateway Assignment
- DNS Server Configuration (Optional)
- Configuration Verification
- Connectivity Testing

---

## 📂 Project Structure

```text
Basic-Router-Configurations-DHCP/
│
├── Basic Router Configurations (DHCP).pkt
└── README.md
└── Screenshots
```

---

## 🚀 Lab Tasks

### 1. Initial Router Configuration

- Configure the hostname.

---

### 2. Configure Router Interfaces

- Assign IPv4 addresses.
- Configure subnet masks.
- Enable interfaces using:

---

### 3. Configure DHCP

Create excluded addresses:

```bash
ip dhcp excluded-address <start-ip> <end-ip>
```

Create a DHCP pool:

```bash
ip dhcp pool LAN
 network <network> <subnet-mask>
 default-router <gateway>
 dns-server <dns-ip>
```

---

### 4. Configure Client PCs

- Set IP configuration to **DHCP**.
- Obtain an IP address automatically.
- Verify assigned IP information.

---

### 5. Verify Configuration

Check DHCP bindings:

```bash
show ip dhcp binding
```

View DHCP pool statistics:

```bash
show ip dhcp pool
```

Verify interface status:

```bash
show ip interface brief
```

View the running configuration:

```bash
show running-config
```

---

## 🔍 Verification Checklist

- Router interfaces are up.
- DHCP pool is configured correctly.
- Reserved IP addresses are excluded.
- PCs receive IP addresses automatically.
- Clients receive:
  - IP Address
  - Subnet Mask
  - Default Gateway
  - DNS Server (if configured)
- Clients can successfully ping the router.

---

## 🧪 Skills Practiced

- Cisco IOS CLI
- Router Configuration
- DHCP Configuration
- Interface Configuration
- IPv4 Addressing
- Network Verification
- Basic Troubleshooting

---

## 🎯 Learning Outcomes

After completing this lab, you will be able to:

- Configure a Cisco router as a DHCP server.
- Create and manage DHCP pools.
- Reserve IP addresses using exclusions.
- Verify DHCP operation.
- Troubleshoot IP address assignment issues.
- Validate network connectivity.
