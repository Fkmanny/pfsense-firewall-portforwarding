# Secure Network Administration Projects

This repository documents three hands-on projects implementing enterprise-grade network security using **pfSense firewalls, port forwarding**. Each project includes detailed configurations, annotated screenshots, and key learnings.

---

## **Project 1: Enterprise Firewall Deployment with pfSense**
### **Overview**
Deployed a pfSense firewall to segment WAN (external) and LAN (internal) traffic in a virtualized environment, enabling secure internet access for internal devices.

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

## **Project 2: Port Forwarding for Remote Desktop**
### **Overview**
Configured pfSense to forward external RDP traffic to two internal Windows machines:
- **desk01**: `10.174.237.25:3399` → `192.168.237.60:3389`  
- **desk02**: `10.174.237.25:3400` → `192.168.237.70:3389`  

### **Key Steps**
1. Assigned static IPs to Windows VMs.  
2. Created NAT rules in pfSense.  
3. Disabled Windows Defender Firewall for testing.

### **Screenshots**
- **NAT Rules**:  
  ![Port Forwarding Rules](screenshots/nat-rules.png)  
- **Successful RDP Connection**:  
  ![Remote Desktop](screenshots/rdp-success.png)  

### **Lessons Learned**
- RFC 1918 blocking can interfere with VPN connections; disabled it for testing.  

---

## **How to Reproduce**
1. Download [pfSense ISO](https://www.pfsense.org/download/).  
2. Set up VMs with WAN/LAN interfaces as described.  
3. Refer to screenshots for troubleshooting.  

---
