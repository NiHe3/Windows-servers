# Windows-servers

# Directory
- [Lab 1 – Lab Installing](#lab-1)
- [Lab 2 – Lab Configuration](#lab-2)
- [Lab 3.1 – Active Directory](#lab-31)
- [Lab 3.2 – Group Policy](#lab-32)
- [Lab 4.1 - Network](#lab-41)
- [Lab 4.2 - Domain Join + Group Policy testing](#lab-42)


---

# Lab 1 – Lab Installing

Return documentation with the virtual machine virtual hardware specifications, names, and the operating system installed.  
Also include a screenshot of the hypervisor + virtual machines.

## Hypervisor  
VirtualBox

![VirtualBox with two virtual machines](Images/Mod1/Virtualbox.png)

## NAT Network  
NAT network is used for the virtual machines. Default settings give NAT, but we changed this to **NAT Network** and used Windows Servers config.

![NAT Network Configuration](Images/Mod1/Networks.png)

---

## Server 1
- **Name:** Server1  
- **Operating System:** Windows Server 2022 Datacenter (Desktop Experience)  
- **ISO Used:** `en-us_windows_server_2022_updated_dec_2025_x64_dvd_84450f64`  
- **Network:** NAT Network  

![Server1 working](Images/Mod1/server1-working.png)

---

## Server 2
- **Name:** Server2  
- **Operating System:** Windows Server 2022 Datacenter  
- **ISO Used:** `en-us_windows_server_2022_updated_dec_2025_x64_dvd_84450f64`  
- **Network:** NAT Network  

![Server2 working](Images/Mod1/datacenter-working.png)

---

## Client Machine
- **Name:** Client1  
- **Operating System:** Windows 11 Business (Version 25H2)  
- **ISO Used:** `en-us_windows_11_business_editions_version_25h2_updated_dec_2025_x64_dvd_e9c929fc`  
- **Network:** NAT Network  

![Client VM working](Images/Mod1/Client-working.png)

---

# Lab 2 – Lab Configuration

Perform post‑installation configuration for the Windows Servers installed in the previous lab.

## Server01 – Post Installation

### Computer Name, IP, DNS, Time Zone  
![Server01 IPv4 config](Images/Mod2/Server01-IPv4-config.png)  
![Server01 Time + Name](Images/Mod2/Server01-Time-ipv4-name-changed.png)

### Guest Additions Installed  
![Server01 Guest Additions](Images/Mod2/Server01-Guestadd.png)

---

## Server02 – Post Installation 

### Computer Name  
![Server02 Name](Images/Mod2/Server02-Name.png)

### Time Zone  
![Server02 Time](Images/Mod2/Server02-TIme.png)

### IPv4 Configuration  
![Server02 IPv4](Images/Mod2/Server02-IPv4.png)

### Guest Additions Installed  
![Server02 Guest Additions](Images/Mod2/Server02-Guestadd.png)

---

## NAT Network Verification  
![NAT Network CMD](Images/Mod2/Natnetworkcmd.png)

---

## Installing Web Server Role on Server02 

### PowerShell Remoting  
From Server01:

```
Enter-PSSession -ComputerName Server02
Install-WindowsFeature -Name Web-Server
```

### Screenshots
![IIS](Images/Mod2/Servers-Powershell-Webserver.png)

Webserver working
![Serverup](Images/Mod2/Webserver-working.png)

---

# Lab 3.1 – Active Directory

Create a domain controller, OU structure, and example users.

## Domain Installation  
![AD Install](Images/Mod2/Server01-AD-Install.png)

## OU Structure  
![OU Structure](Images/Mod3/Server01-AD-Small-Org.png)

## Users in OUs  
![Users](Images/Mod3/server01-ad-users.png)

## Groups  
![Groups](Images/Mod3/Server01-AD-Groups.png)  
![Helsinki Groups](Images/Mod3/Server01-AD-Groups-Helsinki.png)

## Login Test  
![AD Login](Images/Mod3/Server01-AD-login.png)

---

# Lab 3.2 – Group Policy

## GPO Creation and Linking  
Two GPOs created with different taskbar settings and linked at different OU levels.
Useful link https://gpsearch.azurewebsites.net/
![GPO Overview](Images/Mod4/Server01-GP-GPO.png)  
![GPO Wizard Helsinki](Images/Mod4/Server01-GP-Wizard-helsinki.png)  
![GPO Wizard Helpdesk](Images/Mod4/Server01-GP-Wizard-Helpdesk.png)

## Taskbar Settings  
![Taskbar Enabled](Images/Mod4/Server01-GP-Taskbar-Enabled.png)  
![Taskbar Disabled](Images/Mod4/Server01-GP-helpdesk-unlock.png)

## Processing Order Verification  
![Processing Order](Images/Mod4/Server01-GP-Shot2.png)

---

## Central Store Creation  
![SYSVOL Central Store](Images/Mod4/Server01-GP-SYSVOL-SHOT3.png)  
![Central Store Folder](Images/Mod4/Server01-GP-CentralStore.png)

## Chrome ADMX Added  
![Chrome ADMX](Images/Mod4/Server01-GP-Google-Shot4.png)

# Lab 4.1 - Network
## Tasks:
    Install DHCP Server role on Windows Server virtual machine installed in first lab
    Configure a DHCP scope for your network on the DHCP Server that has:
        50 available addresses
        Contains gateway, DNS Servers and DNS suffix configuration as options
    Configure the DNS Server:
        Add a static DNS host A record for the second Windows Server (the another virtual machine create in the first lab)
        Add a CNAME record of your choosing that points to the record create in previous step
      Return screenshots that show your configuration. One screenshot could show the DHCP server configuration along with   the scope you configured and another screenshot could show the DNS zone and the modifications you made.

# Lab 4.2 - Domain Join + Group Policy testing
## Tasks:
    Windows 10 or 11 virtual machine required
    After installation change the computer name, verify the IP configuration (from your DHCP server configured in Lab 4,   Part 1), and join the workstation to your domain
    Verify that the Group Policy settings done in Lab 3, Part 2 works in the client machine
    Join the second server to the domain as well
    Return screenshots that show your configuration. Screenshots:
    Windows client OS virtual machine IP configuration (from GUI or CLI)
    DHCP Server address leases that show the lease for Windows client virtual machine (from DHCP manager)
    Windows client OS and Windows Server OS computer accounts in domain (from any AD DS management tool)
    Group Policy effect visible (from the OS or from tool like gpresult /r)
