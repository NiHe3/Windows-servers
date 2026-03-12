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

## Tasks:

    Choose a suitable hypervisor for your own machine and install it (if you do not have one already)
    Configure the networking in the hypervisor so that the virtual machines which you will create can communicate via network
    Create and install two Windows Server 2022 virtual machines
        Datacenter (Desktop Experience)
        Datacenter ie. Core version
    Create and install one Windows 11 (or 10) virtual machine
    After installation, verify that all virtual machines start


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

## Tasks:

    Perform post installation configuration for the Windows Servers installed in the previous lab, at least:
        Computer name
        Network configuration (IP address, DNS settings)
        Time Zone
    Install a web server role to the Windows Server Core installation using PowerShell remotingReturn the Windows Server configurations (for example screenshot from the Server Manager showing name, IP configuration or a sconfig tool output showing similar information).Also document the PowerShell command you used to install the web server and verify that the web server has been installed correctly (web page opened from the server).

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
## Tasks:
    Create a domain controller in a new AD environment
        You can use either of the Windows Server virtual machines installed in the previous lab (recommended: Desktop Experiance)
        Name the domain as you wish
    Create OU structure to the new domain which contains OUs:
        Helsinki
            Users
            Desktops
        Tampere
            Users
            Desktops
    Create example user in both Users OUs (so one user in Helsinki and one in Tampere).Return a screenshot which shows your Domain OUs and their contents from one of the Active Directory management tools.


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
## Tasks:
    1. GPO creation and linking
        Create two GPOs
            Both contain same setting with different values (for example one with taskbar locked and another one with taskbar not locked).
        Link the GPOs      Link them in a way that both affect the same user (or computer), but one is linked higher in the OU hierarchy than the other.
        Verify the processing order of the GPOs (GPO linked closer to the target “wins”) using tools in the Group Policy Management Console.
    2. Create a Central Store
        Create a Central Store for the Administrative Templates
        Add the ADMX files from one machine to the Central Store
        Add the ADMX files for Google Chrome to the Central Store
    Return screenshots and/or documentation that verifies your work. Example screenshots:
        GPMC with the GPOs and OU hierarchy visible
        GP Editor with the modified setting(s) visible
        Central Store folder open (location visible)
        Google Chrome settings showing in GP Editor

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
## DHCP 
Heres the scope
![DHCP-scale](Images/Mod4/DHCP-Scope.png)

Heres the DHCP scope options
![DHCP-options](Images/Mod4/DHCP-Scope-Options.png)

## DNS
DNS static host
![Dns-static](Images/Mod4/DNS-Static-Host.png)

DNS cname
Cname.png
![Cname](Images/Mod4/Cname.png)
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
Windows Client OS IP config
Ipconfig
![Client OS IP config](Images/Mod4.1/Client01-DHCP-IPconfig.png)

IPconfig all
![Client OS IP config](Images/Mod4.1/Client01-DHCP-IPconfig-all.png)

DHCP Server address leases that show the lease for Windows client virtual machine (from DHCP manager)
 ![DHCP Server address](Images/Mod4.1/Server01-DHCP-Lease.png)

Windows client OS and Windows Server OS computer accounts in domain (from any AD DS management tool)
 Server
 ![Domain-Server](Images/Mod4.1/Server01-in-Domain.png)
 Client
  ![Domain-Client](Images/Mod4.1/Client01-Domain-Setting-About.png)
Group Policy effect visible (from the OS or from tool like gpresult /r)
  ![Group Policy](Images/Mod4.1/Client01-Policy-effect.png)
