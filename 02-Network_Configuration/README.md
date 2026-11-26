# Lab 02 - Network Configuration
## Objective
Establish a private virtual network and configure DC01 with a static IP address in preparation for Active Directory.

## Environment
- **Hypervisor:** Oracle VirtualBox
- **OS and Edition:** Microsoft Windows Server 2025 Standard (Desktop Experience)
- **Allocated RAM:** 4 GB
- **CPU:** 2 vCPUs
- **Storage:** 80 GB
- **Hostname:** DC01
- **Network Adapter:** Internal Network

## Implementation
1. Ran Windows updates.
2. Examined the network configuration using `ipconfig /all` and identified that VirtualBox automatically assigned network settings through DHCP.
3. Switched DC01 from NAT to an Internal Network named `IT-Lab` to isolate the lab environment.
4. Configured DC01 with a static IPv4 address of `192.168.50.10/24` to ensure the domain controller has a consistent IP address.
5. Left the IPv4 default gateway and DNS settings unconfigured because those services have not been established yet.
6. Ran `ipconfig /all` to verify the new IPv4 configuration.

## Troubleshooting
No issues were encountered during network configuration.

## Verification
**Confirmed Internal Network**\
<img width="542" height="261" alt="image" src="https://github.com/user-attachments/assets/96f88b2c-44cd-457c-a559-3b0de291198d" />


**Confirmed IPv4 Configuration**\
<img width="770" height="346" alt="image" src="https://github.com/user-attachments/assets/1b6ce193-b1b5-4928-acfe-546b0d5d2541" />

