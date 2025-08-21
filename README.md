# pfSense Firewall Configuration and Portforwarding Project

This repository documents three hands-on projects implementing enterprise-grade network security using **pfSense firewalls, port forwarding**. Each project includes detailed configurations, annotated screenshots, and key learnings.

---

## Organizational Application

### Importance to Companies
This implementation is critical for securing network perimeters and enabling secure remote access, which are foundational to modern business operations. Proper firewall configuration protects sensitive data from unauthorized access, while port forwarding facilitates essential remote services without exposing internal networks.

### Use Case Scenario
A financial services firm uses this setup to allow employees secure remote desktop access to internal accounting systems from home, ensuring business continuity while maintaining a hardened security posture against external threats.

---

## **Project 1: Enterprise Firewall Deployment with pfSense**
### **Overview**
Deployed a pfSense firewall to segment WAN (external) and LAN (internal) traffic in a virtualized environment, enabling secure internet access for internal devices.

---

### **Implementation Steps & Screenshots**
1. **pfSense VM Configuration**  
   - Assigned WAN: `10.174.237.25/24` (Adapter: `network01`)  
   - Assigned LAN: `192.168.237.1/24` (Adapter: `network02`) with DHCP range `192.168.237.35-50`  
   ![pfSense CLI Setup](screenshots/pfsense-cli.png)  
   *Static IP assignment via console.*

2. **Windows 10 Client Deployment**  
   - **Fix**: Enabled DHCP on pfSense LAN interface and repaired Windows network settings:  
   ![Successful DHCP Lease](screenshots/dhcp-fix.png)  
   *Assigned IP: `192.168.237.35`.*

3. **Validation**  
   - Ping tests from Windows client to WAN (`10.174.237.25`) and Google (`8.8.8.8`):  
   ![Ping Validation](screenshots/ping-success.png)  
   *4/4 packets received confirms proper routing.*

### **Key Challenges**
- **DHCP Misconfiguration**: Required manual intervention on Windows client.  
- **Interface Assignment**: Critical to bind WAN/LAN to correct adapters (`network01`/`network02`).


---


## **Project 2: Secure Remote Access via Port Forwarding**
### **Overview**
Enabled external RDP access to two internal Windows VMs (`desk01`/`desk02`) through pfSense NAT rules.

### **Step-by-Step with Screenshots**
1. **Static IP Assignment**  
   - `desk01`: `192.168.237.60`  
   ![Static IP - desk01](screenshots/desk01-staticip.png)  
   - `desk02`: `192.168.237.70`  
   ![Static IP - desk02](screenshots/desk02-staticip.png)  

2. **pfSense NAT Rules**  
   - Forwarded external ports to internal RDP (3389):  
     - `3399` → `192.168.237.60:3389`  
     - `3400` → `192.168.237.70:3389`  
   ![NAT Rules](screenshots/nat-rules.png)  

3. **Connection Test**  
   - Successful RDP from home PC to both VMs:  
   ![RDP to desk01](screenshots/nat-rules.png)  
   ![RDP to desk02](screenshots/rdp-success.png)  

### **Lessons Learned**
- **Windows Firewall**: Must be disabled for external RDP during testing.  
- **RFC 1918 Blocking**: Disabled on pfSense WAN to allow private IP traffic (e.g., VPN).

---

## **How to Reproduce**
1. **Requirements**:  
   - VMware/vSphere environment.  
   - [pfSense ISO](https://www.pfsense.org/download/).  
   - Windows 10 VMs for client testing.  
2. **Steps**:  
   - Follow interface/IP assignments per project.  
   - Refer to screenshots for rule configurations.  

---
