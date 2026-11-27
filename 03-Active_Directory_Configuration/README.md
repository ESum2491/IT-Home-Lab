# Lab 03 - Active Directory Configuration
## Objective
Install Active Directory Domain Services and promote DC01 to a domain controller in order to have a centralized management system for users and computers on my lab.

## Research and Planning
- Researched Active Directory Domain Services purpose as well as installation methods.
- Investigated Active Directory Forests and the difference between a Forest & Domain.
- Looked into Server Pools and their use.
- Looked into Service Alert for `Inventory and Compatibility Appraisal Service`.
- Researched DNS purpose on AD DS local network.

## Environment
- **Hypervisor:** Oracle VirtualBox
- **OS and Edition:** Microsoft Windows Server 2025 Standard (Desktop Experience)
- **Allocated RAM:** 4 GB
- **CPU:** 2 vCPUs
- **Storage:** 80 GB
- **Hostname:** DC01
- **Network Adapter:** Internal Network

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
11. Restarted system and confirmed DNS and AD DS were configured properly.

## Troubleshooting
- `Inventory and Compatibility Appraisal service` had a service alert flag.
    - Looking into it, the Start Type was Delayed Start.
    - Waiting and refreshing the dashboard did not resolve the alert.
    - Upon further research, this is normal behavior as the service is only active under certain conditions and shuts down 5 minutes after the final RPC call.
 - When running `dcdiag /q`, `SystemLog` and `DFSREvent` return failed.
    - Both return string `Secure Boot certificates have been updated but are not yet applied to the device firmware.`.
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

## Research References
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview \
https://www.serveracademy.com/blog/what-is-an-active-directory-forest \
https://learn.microsoft.com/en-us/answers/questions/2258983/inventory-and-compatibility-appraisal-service-in-m \
https://learn.microsoft.com/en-us/windows-server/networking/dns/dns-overview

## Verification
**Confirmed AD DS Installation on DC01**
<img width="1025" height="770" alt="image" src="https://github.com/user-attachments/assets/8ebb95d0-3aef-43f5-bf84-03dc21da85c4" /> \
<img width="1026" height="770" alt="image" src="https://github.com/user-attachments/assets/a31630fb-5556-4a0c-9168-948775f577dc" />

**Confirmed New Forest Setup**
<img width="1022" height="768" alt="image" src="https://github.com/user-attachments/assets/5b3f9010-8e36-42b3-88e6-a2060c0d03d2" />

**Confirmed DNS Configuration**
<img width="1026" height="769" alt="image" src="https://github.com/user-attachments/assets/2258d085-a331-4276-ad65-7561a29bfe41" /> \
<img width="939" height="621" alt="image" src="https://github.com/user-attachments/assets/5af906e5-5d81-4d83-b91e-a521a80e4ec1" />

**Confirmed AD DS Configuration**
<img width="1025" height="766" alt="image" src="https://github.com/user-attachments/assets/9f034b99-e081-44fe-93b3-31d868dcd15a" />
