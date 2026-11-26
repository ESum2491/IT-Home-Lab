# Lab 01 - Windows Server Installation
## Objective
Install Windows Server 2025 to set up the base of my IT Home Lab.

## Environment
- **Hypervisor:** Oracle VirtualBox
- **OS and Edition:** Microsoft Windows Server 2025 Standard (Desktop Experience)
- **Allocated RAM:** 4 GB
- **CPU:** 2 vCPUs
- **Storage:** 80 GB
- **Hostname:** DC01
- **Network Adapter:** NAT

## Implementation
1. Obtained Windows Server ISO through Azure Student resources.
2. Disabled unattended installation.
3. Chose Desktop Experience instead of the Server Core installation.
4. Configured built-in Administrator account and completed Windows updates.
5. Captured baseline VM snapshot.

## Troubleshooting
Server Manager Dashboard had initially reported "Local Server > Services" to have 7 service alerts. Looking into services, I found there were multiple delayed start services which were not shown to be running. Refreshing the dashboard cleared the alerts.

## Verification
**Confirmed Server Setup**
<img width="1022" height="770" alt="image" src="https://github.com/user-attachments/assets/b2b1bb76-e26f-4de6-9b58-fff8f2a2dc2e" />

**Confirmed Windows Activation**
<img width="1024" height="767" alt="image" src="https://github.com/user-attachments/assets/7415af95-b31c-4200-9e36-a40298a1671a" />

**Confirmed Windows Up-to-Date**
<img width="797" height="629" alt="image" src="https://github.com/user-attachments/assets/42bb6d39-ad14-492c-8626-34161125d3f9" />
