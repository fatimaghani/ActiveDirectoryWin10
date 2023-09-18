# Basic Home Lab Running Active Directory

This repository contains steps on how I set up a basic home lab running Active Directory following a tutorial by [Josh Madakor](https://www.youtube.com/@JoshMadakor)

## Diagram
<p align="left"><img src="https://i.imgur.com/TFseqmm.jpg" height="80%" width="80%" alt="Create Virtual Machine"/></p>

## Download and install Oracle VirtualBox from the official website.
[Oracle Virtual Box](https://www.virtualbox.org/)

## Download the Windows 10 and Server 2019 ISO files.
[Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10ISO)
[Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)

## Create and deploy new virtual machine which will act as the Domain Controller.

Create a new Virtual Machine by clicking on "New" in VirtualBox, naming it "Domain Controller" or "DC".
I set the Hardware Base Memory to 2048 MB, Virtual Disk Size to 20 GB and Processors to 4 CPU (adjust based on your capabilites).
<p align="left"><img src="https://i.imgur.com/ru1x7Cz.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

For the Virtual Machine that will be hosting my Domain Controller, I need two network adapters. I need the NAT that will use my host IP address from my home router and an Internal Network Adapter so that my DC can communicate with other Virtual Machines. We will leave Adapter 1 as is (Default: NAT), but we will configure Adapter 2 as the Internal Network. 
<p align="left"><img src="https://i.imgur.com/YIIhqmg.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
Upon starting "DC", selecting the "Windows Server 2019" ISO file as the boot media. Install Server 2019 on the Virtual Machine.
<p align="left"><img src="https://i.imgur.com/YlQw8G5.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

##  Assign IP addressing for the Internal Network in the VM.
After downloading Windows Server 2019 on the Virtual Machine, I will configure and rename the two network adapters I have (external and internal NIC) so it is easier to tell them apart for future configurations.
<p align="left"><img src="https://i.imgur.com/5T85FDv.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
I will leave the _INTERNET_ as is, but I will apply the following settings to the _INTERNAL_X network:
<p align="left"><img src="https://i.imgur.com/5Ba8r07.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

##  Name the server and install Active Directory to create the domain.
Go to the Server Manager > Add Roles and Features.
Leave all settings to default, except check 'Active Directory Domain Services' in the Server Roles tab and install.
<p align="left"><img src="https://i.imgur.com/z9CwOgt.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
Configure the Active Directory Domain Services set up.
<p align="left"><img src="https://i.imgur.com/fxfhw4T.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

##  Configure routing so that clients on the private network can access the internet through the domain controller.
Go to the Server Manager > Add Roles and Features.
Leave all settings to default, except:
check 'Remote Access' in the Server Roles tab
check 'Direct Access and VPN (RAS)' and 'Routing' under the  Role Services tab
<p align="left"><img src="https://i.imgur.com/KFYOz3B.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
Configure the Remote Access set up.
<p align="left"><img src="https://i.imgur.com/mClVd75.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
<p align="left"><img src="https://i.imgur.com/rzasCSI.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

![](attachments/Pasted%20image%2020230402153904.png)

![](attachments/Pasted%20image%2020230402154123.png)

##  Set up DHCP on the domain controller.
Go to the Server Manager > Add Roles and Features.
Leave all settings to default, except:
check 'DHCP' in the Server Roles tab and install.
<p align="left"><img src="https://i.imgur.com/ChWflpy.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
Configure the DHCP set up using:
<p align="left"><img src="https://i.imgur.com/pp3LqZc.png" height="30%" width="30%" alt="Create Virtual Machine"/></p>
<p align="left"><img src="https://i.imgur.com/fGgy3kZ.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

##  Run the PowerShell script to create 1000 users in Active Directory.

[Power Shell script for creating users](https://github.com/joshmadakor1/AD_PS)
<p align="left"><img src="https://i.imgur.com/9Rb1qK1.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
We can observe the output of the newly created users here in the Active Directory.
<p align="left"><img src="https://i.imgur.com/MLimmaz.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

##  Create a new 'Client' virtual machine

Now we will create another virtual machine in VirtualBox named "Client1" and install Windows 10 on it that will act as a user.
<p align="left"><img src="https://i.imgur.com/b5GUjfF.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>


##  Connect the Client machine to the private network and join it to the domain.

Click Start > System > Rename this PC (Advanced)
We will rename the Computer to CLIENT1 and join it to the domain 'mydomain.com'
<p align="left"><img src="https://i.imgur.com/wl3SXbe.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
<p align="left"><img src="https://i.imgur.com/ubr4gXt.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>

##  Log into the Client machine with a domain account.

We will log into MYDOMAIN with a user account created from the Powershell script to test if everything is configured correctly.
<p align="left"><img src="https://i.imgur.com/PUtgGHu.png" height="50%" width="50%" alt="Create Virtual Machine"/></p>
We can ping the domain to see if we recieve a response.
<p align="left"><img src="https://i.imgur.com/jj1Y5h5.png" height="40%" width="40%" alt="Create Virtual Machine"/></p>

'whoami'
<p align="left"><img src="https://i.imgur.com/5MGBXAw.png" height="40%" width="40%" alt="Create Virtual Machine"/></p>

We can also head back into our server VM 'DC' to check how many computers or devices are currently connected to the domain. We can see that CLIENT1 computer is being properly recognized in Active Directory. 
<p align="left"><img src="https://i.imgur.com/9IQmkwg.png" height="60%" width="60%" alt="Create Virtual Machine"/></p>
