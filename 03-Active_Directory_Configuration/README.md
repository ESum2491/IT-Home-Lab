# Lab 03 - Active Directory Configuration
## Objective
Install Active Directory Domain Services and promote DC01 to a domain controller in order to have a centralized management system for users and computers on my lab.

## Research and Planning
- Researched Active Directory Domain Services purpose as well as installation methods.
- Investigated Active Directory Forests and the difference between a Forest & Domain.
- Looked into Server Pools and their use.
- Looked into Service Alert for `Inventory and Compatibility Appraisal Service`.
- Researched DNS purpose on AD DS local network.
- Researched Reverse Lookup Zones.

## Environment
- **Hypervisor:** Oracle VirtualBox
- **OS and Edition:** Microsoft Windows Server 2025 Standard (Desktop Experience)
- **Allocated RAM:** 4 GB
- **CPU:** 2 vCPUs
- **Storage:** 80 GB
- **Hostname:** DC01
- **Network Adapter:** Internal Network
- **Domain Name:** homelab.test
- **Static IPv4 Address:** 192.168.50.10/24

## Implementation
1. Created a snapshot to fall back on if necessary.
2. Went to Server Manager > Manage > Add Roles and Features.
3. Used Role-based installation.
4. Selected DC01 from the server pool and gave it AD DS role.
5. Kept default features for basic AD DS install.
6. Promoted DC01 to domain controller.
7. Created a new forest and set the Root domain name to `homelab.test`.
8. Enabled the DNS Server role on DC01 and set DSRM password.
9. NetBIOS domain name set to `HOMELAB`.
10. Left paths as default.
11. Restarted system and tested that DNS and AD DS were configured properly.
12. Addressed errors by checking events and adding Reverse Lookup Zone.
13. Tested and confirmed configurations.
14. Created another baseline snapshot to fall back on.

## Troubleshooting
- `Inventory and Compatibility Appraisal service` had a service alert flag.
    - Looking into it, the Start Type was Delayed Start.
    - Waiting and refreshing the dashboard did not resolve the alert.
    - Upon further research, this is normal behavior as the service is only active under certain conditions and shuts down 5 minutes after the final RPC call.
 - When running `dcdiag /q`, `SystemLog` and `DFSREvent` return failed.
    - `Secure Boot certificates have been updated but are not yet applied to the device firmware.`.
    - Attempted system restart to fix.
      - Didn't fix the problem.
      - New error appeared for the SystemLog error. `The dynamic registration of the DNS record 'homelab.test. 600 IN A 192.168.50.10' failed on the following DNS server:`
    - Running `nslookup dc01.homelab.test` returned the proper IP.
    - Running `nslookup homelab.test` returned the proper IP.
    - Both nslookup executions caused a timeout.
    - Ran `nslookup homelab.test 192.168.50.10` to test if problem is IPv6 specific.
      - Returned with timeout.
    - Ran `nslookup 192.168.50.10 192.168.50.10` to see if reverse DNS resolution is the problem.
      - Fully timed out so reverse DNS resolution is the problem.
    - Created a Reverse Lookup Zone and ran `ipconfig /registerdns` to try and get DC01 to register properly.
      - New PTR appeared in the RLZ for `192.168.50.10 - DC01.homelab.test`.
    - Ran `nslookup 192.168.50.10 192.168.50.10` to see if timeout persists.
      - IP properly resolved.
 - I ran `dcdiag /q` again and the same error was output for `SystemLog` but `DFSREvent` error changed.
    - `There are warning or error events within the last 24 hours after the SYSVOL has been shared. Failing SYSVOL replication problems may cause Group policy problems.`
    - I looked in Event Viewer at DFS Replication.
      - One warning and one error were shown but their times were during AD installation and configuration.
    - Ran `dcdiag /test:SysVolCheck`.
      - Required and Primary tests passed.
    - Ran `dcdiag /test:NetLogons`.
      - Required and Primary tests passed.
    - Appears that the issue resolved itself and the diagnostic failure should clear after 24 hours if no further DFS Replication errors occur.
 - EventID 1801 caused the `SystemLog` test to fail.
    - Investigated the error using msinfo32.
    - Identified the VM uses Legacy BIOS mode and does not support Secure Boot in its current configuration.
    - Decided to not change configuration as it does not affect AD DS functionality. (Identified but Not Resolved)

## Research References
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview \
https://www.serveracademy.com/blog/what-is-an-active-directory-forest \
https://learn.microsoft.com/en-us/answers/questions/2258983/inventory-and-compatibility-appraisal-service-in-m \
https://learn.microsoft.com/en-us/windows-server/networking/dns/dns-overview \
https://learn.microsoft.com/en-us/windows-server/networking/dns/zone-types

## Verification
**Confirmed Domain Controller Promotion**
<img width="1026" height="770" alt="image" src="https://github.com/user-attachments/assets/a31630fb-5556-4a0c-9168-948775f577dc" />

**Confirmed Network and DNS Configuration**
<img width="939" height="621" alt="image" src="https://github.com/user-attachments/assets/5af906e5-5d81-4d83-b91e-a521a80e4ec1" />

**Confirmed Domain Authentication**
<img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/3793fb51-a4a9-4839-a1bc-6d31acd95a31" />

**Confirmed Forward and Reverse DNS Resolution**
<img width="1021" height="770" alt="image" src="https://github.com/user-attachments/assets/aef00d2d-68b8-4365-abc4-5bfd7d64f651" />

**Confirmed DNS Record Registration**
<img width="1024" height="769" alt="image" src="https://github.com/user-attachments/assets/bab49dea-e201-4cc0-a8a0-40c878ea717e" />

**Confirmed SYSVOL and NETLOGON Functionality**
<img width="1029" height="771" alt="image" src="https://github.com/user-attachments/assets/8c2526ad-46f8-4464-a444-3bd31707be9c" /> \
<img width="1028" height="770" alt="image" src="https://github.com/user-attachments/assets/5ab5bf60-5135-4a93-bd53-ec13b3440ca4" />


