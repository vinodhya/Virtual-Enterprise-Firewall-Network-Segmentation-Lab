# Virtual Enterprise Firewall & Network Segmentation Lab

## Overview
This project is a virtual networking and security lab built with **pfSense** and **Oracle VirtualBox**. It demonstrates firewall interfaces, WAN/LAN/DMZ segmentation, traffic-control rules, connectivity testing, and network isolation validation using an integrated **Kali Linux** testing VM.

## Objectives
- Separate WAN, LAN, and DMZ network segments using virtual network adapters.
- Implement a least-privilege security model using pfSense firewall rules.
- Isolate internal LAN assets (`192.168.10.0/24`) from perimeter DMZ hosts.
- Permit outbound internet access for DMZ resources while restricting lateral movement.
- Validate network security policies using hands-on ICMP testing and firewall logging.

## Tools
- **Hypervisor:** Oracle VirtualBox
- **Firewall:** pfSense Community Edition
- **Testing Client:** Kali Linux VM (placed in DMZ)


## IP Addressing Plan

| Zone / Device | Interface | Subnet / IP Address | Description |
|---|---|---|---|
| **WAN** | `em0` | VirtualBox NAT / DHCP | Gateway interface to the internet |
| **pfSense LAN** | `em1` | `192.168.10.1/24` | Trusted internal gateway |
| **pfSense DMZ** |`em2` (OPT1) | `192.168.20.1/24` | Semi-trusted perimeter gateway |
| **Kali Linux VM** | `eth0` | `192.168.20.100/24` | Testing client in DMZ Internal Network |

## Implemented Firewall Policy (DMZ Interface)

| Rule Priority | Source | Port | Destination | Port | Action | Purpose / Description |
|---|---|---|---|---|---|---|
| **1** | DMZ subnets (`192.168.20.0/24`) | `*` | LAN subnets (`192.168.10.0/24`) | `*` | **Block** | Prevents lateral movement from DMZ to Internal LAN |
| **2** | DMZ subnets (`192.168.20.0/24`) | `*` | Any (`*`) | `*` | **Allow** | Permits outbound internet connectivity for updates/services |

## Verified Testing Results

Testing was conducted directly from the Kali Linux VM (`192.168.20.100`) in the DMZ segment:

| Test Case | Command | Expected Result | Actual Result | Status |
|---|---|---|---|---|
| **DMZ to LAN Isolation** | `ping 192.168.10.1` | Packets blocked / Request timed out | Connection timed out (0 packets received) | **PASSED** |
| **Outbound Internet Access** | `ping 8.8.8.8` | Reply from `8.8.8.8` | Successful reply (`64 bytes from 8.8.8.8...`) | **PASSED** |
| **DNS Resolution & Web Access** | `ping google.com` | Resolved IP & Reply | Successful resolution & reply | **PASSED** |

## Verification & Screenshots
1. **Firewall Rules Verification:** Confirmed rule ordering on the pfSense DMZ interface tab (Block rule placed above Allow rule).
2. **Network Isolation Validation:** Verified via Kali Linux terminal output showing packet loss on `192.168.10.1`.
3. **Outbound Traffic Verification:** Verified ICMP ping success to `8.8.8.8`.

## Learning Outcomes
This lab is intended to develop practical understanding of virtual network interfaces, IP addressing, network segmentation, firewall policy, connectivity troubleshooting, and log-based validation.


