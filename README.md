# Static-Route-Troubleshooting-Lab
This lab focused on troubleshooting connectivity issues in a multi-router network environment using static routing. 

The objective was to restore connectivity between PC1 and PC2, which were located on different networks separated by three routers.

Initial testing showed that the hosts were unable to communicate via ICMP.

# 🛜 Network Topology 
<img width="716" height="390" alt="Topology-Troubleshooting-StaticRoutes" src="https://github.com/user-attachments/assets/693db6ab-926c-4978-8e0c-013c83f39c70" />

# Networks Used:<br/>
|Network | Subnet|
| --- | --- |
|PC1 LAN| 192.168.1.0/24|
|R1-R2 Link | 192.168.12.0/24|
|R2-R3 Link | 192.168.13.0/24|
|PC2 LAN    | 192.168.3.0/24|

# 🔧Troubleshooting Methodology
Step 1 - Connectivity Testing
Initial ping tests were performed to verify the problem. 
```
PC1 -> ping PC2
PC2 -> ping PC1
```
Both attempts failed, confirming a routing issue within the network. 

Step 2 - Router Configuration Verification 
Each router was inspected using the following commands:
```
show ip route
show ip interface brief
```
These commands were used to confirm: 
- Active interfaces
- Correct IP addressing
- Static route Configuration

Step 3 - Issue Identified on R1 
The static route was configured with an incorrect next-hop address.
Incorrect configuration: 
```
192.168.12.2
```
Correct configuration:
```
192.168.12.2
```
After correcting the route, traffic could properly forward to Router R2.

Step 4 - Issue Identified on R2 
R2 had a route to the 192.168.3.0 network configured using the outgoing interface.
The incorrect route was removed:
```
no ip route 192.168.3.0 255.255.255.0 g0/0
```
Then replaced with a next-hop route pointing to R3:
```
ip route 192.168.3.0 255.255.255.0 192.168.13.3
```

Step 5 - Interface Misconfiguration on R3 
Using: 
```
show ip interface brief
```
I discovered the GigabitEthernet0/0 interface had an incorrect IP address. 
The interface was corrected:
```
interface g0/0
ip address 192.168.13.3 255.255.255.0
```
A static route was then configured for the 192.168.1.0 

# 📊Verification 
connectivity testing after the fixes showed:
- Initial ping failures due to ARP resolution
- Successful communication between PC1 and PC2

# 💡Key Concepts Practiced
- Static route troubleshooting
- Next-hop vs exit interface configuration
- Router interface verification

# 📝Skills Demonstrated
- Network troubleshooting
- Static route configuration
- Cisco IOS CLI diagnostics
- Routing table analysis 
