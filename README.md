<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<img width="275" alt="azure virtual network" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/52c070a5-5137-434e-85e2-ab86abf3ef79">
<p>From that to this.
<p><img width="275" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/44b59471-8718-482b-a0d8-a5bf3c0eb2f5">



<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure

- Ensure Connectivity between the client and Domain Controller

- Install Active Directory

- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users
- Finish.

<h2>Deployment and Configuration Steps</h2>

<p>
Steps to Set Up Active Directory on Azure
<p>1. Set up a virtual machine in Azure
when you Create a virtual machine it will automatically create a virtual network and subnet to organize your Azure resources.
<p><img width="364" alt="create a virtual machine" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/b00d50f7-645e-426b-a186-c9d9e0db2d16">

<p>2. Create Domain Controller VM
Create a VM (Windows Server 2022) named “DC-1”.
Note the resource group and virtual network (Vnet) created.
<p><img width="362" alt="dc-1 vm creation" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/42cceb6c-70d9-4ff3-acdf-514e00f0516c">

<p>3. Configure Domain Controller's Network
Set DC-1’s NIC private IP address to be static.
<p><img width="362" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/dccbcb6b-38d3-4f87-98aa-07c8df443479">

<p>4. Create Client VM
Create a VM (Windows 10) named “Client-1” in the same resource group and Vnet as DC-1.
Ensure both VMs are in the same Vnet.
<p><img width="362" alt="widows 10 vm" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/87525e38-e1a4-46a4-a88d-b9a01f0fb220">

<p>5. Ensure Connectivity
Ping Test:
Login to Client-1 via Remote Desktop.
Ping DC-1’s private IP address: ping -t <DC-1 private IP>. you will get a request time out since the firewall on the server is blocking ICMP traffic. 
<p><img width="340" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/3bbb84ba-4f87-446a-bd9e-2e2a626f4d7a">
  
<p>Enable ICMP:
Login to DC-1 via Remote Desktop.
Enable ICMPv4 on the local Windows Firewall:
Open "Windows Defender Firewall with Advanced Security".
Click on "Inbound Rules".
Enable rules related to ICMPv4.
<p><img width="668" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/80321994-2593-41d9-8b29-60333f828e25">
<p> when you go back to client-1 you should see the ping attempt being successful.
<p><img width="340" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/2b5f7ae8-8c26-4a54-ad50-8208e1852cad">

<p>6. Install Active Directory
On DC-1:
Install "Active Directory Domain Services":
Open Server Manager and select "Add roles and features".
Choose "Role-based or feature-based installation".
Select "Active Directory Domain Services" and complete the installation.
<p><img width="272" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/9faeecc9-0469-44ba-945a-23e7d904882a">
</p><img width="271" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/96950220-c2c1-4be0-9545-6c2e9e66a0de">
  
Promote DC-1 to a Domain Controller:
Click the flag icon in Server Manager.
Select "Promote this server to a domain controller".
<p><img width="116" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/bd3088aa-cbe4-4a16-b5e7-6bf6e64b312a">
</p>
Set up a new forest with the domain name mydomain.com.
<p><img width="264" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/ec043e32-1658-42fa-bd4e-50713a112fae">
</p> After the setup you will restart DC-1 and log back in as mydomain.com\labuser.
<p></p><img width="233" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/a5a85baa-e2ba-467c-8170-653c500a023f">

<p>7. Create Administrative and User Accounts in AD
In Active Directory Users and Computers (ADUC):
Create Organizational Units (OUs):
_EMPLOYEES
<p>_ADMINS
<p><img width="238" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/12013ea4-dcba-4898-a95f-6ec09c314167">

Create a new user "Jane Doe" with the username jane_admin in the _ADMINS OU.
<p><img width="152" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/bc1a37e4-d1cb-4f60-96be-1598a4c8ddc4">
</p>
Add jane_admin to the "Domain Admins" security group.
<p><img width="143" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/29625cb5-5402-4f51-a479-5960e95de170">
</p>
Log out and log back in as mydomain.com\jane_admin.
<p>8. Join Client-1 to the Domain
On Azure Portal:
Set Client-1’s DNS settings to DC-1’s private IP address.
<p><img width="283" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/d4086b0d-a6fb-49b2-a660-53b261496b75">
</p> 
<p><img width="283" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/e4eff56f-6b64-41d0-a0b4-f30d0305210d">
</p>
Restart Client-1.
<p>On Client-1:
Login as the local admin (labuser) and join it to the domain mydomain.com (this will restart the computer).
<p><img width="416" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/26b57853-1a0c-41b8-af2d-0abaf55bdc1d">
</p>
<p>On DC-1:
Verify Client-1 in ADUC under the "Computers" container.
Optionally, move Client-1 to a new OU named _CLIENTS.
<p>9. Configure Remote Desktop for Domain Users
On Client-1:
Login as mydomain.com\jane_admin.
Open system properties and enable Remote Desktop.
Allow "Domain Users" to access Remote Desktop.
<p><img width="416" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/413f86ea-2124-4cc6-a4c0-f3d69d8cc977">
</p>
<p>10. Create Additional Users and Test Login
Create additional users in ADUC.
<p><img width="259" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/12942965-189c-41eb-88fe-469cdf1884ff">
</p>
Attempt to log into Client-1 with one of these new users.
<p><img width="157" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/ba33cedf-f8ab-46ff-92c7-a7b6636f8213">
</p>
<p><img width="386" alt="image" src="https://github.com/Angelplascencia007/configure-ad/assets/168943120/9e891bee-c94d-4850-acd1-706660377c4f">
</p>
This should be the end unless you would like to play around with the program!
