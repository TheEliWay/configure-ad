
![image](https://github.com/user-attachments/assets/f8dcc892-d59f-4620-92fe-e462e44ad19f)

</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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


<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/36fcab47-4f39-427c-b205-591aac71a864)

</p>
<p>
<h2>Setup Resources in Azure</h2>

- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
   - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

</p>
<br />

![image](https://github.com/user-attachments/assets/2d368c0c-acaa-4301-9709-40d7f86767b0)

</p>
<p>
<h2>Ensure Connectivity between the client and Domain Controller<h/2>

- Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
- Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
- Check back at Client-1 to see the ping succeed

</p>
<br />

<p>

   ![image](https://github.com/user-attachments/assets/5539f238-1683-4d0e-b061-f115c6fb0f61)

<p>
<h3>Install Active Directory</h3>

- Login to DC-1 and install Active Directory Domain Services
- Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
- Restart and then log back into DC-1 as user: mydomain.com\labuser

</p>
<br />

<p>
<img src="https://i.imgur.com/FsjIGf0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h3>Create an Admin and Normal User Account in AD</h3>

- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
- Create a new OU named “_ADMINS”
- Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
- Add jane_admin to the “Domain Admins” Security Group
- Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
- User jane_admin as your admin account from now on


</p>
<br />

![image](https://github.com/user-attachments/assets/fcc33c80-2749-402b-af0c-821784b23ed0)

</p>
<p>
<h3>Join Client-1 to your domain (mydomain.com)</h3>

- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
- Create a new OU named “_CLIENTS” and drag Client-1 into there


</p>
<br />

<p>
<img src="https://i.imgur.com/HmkkR8l.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h3>Setup Remote Desktop for non-administrative users on Client-1</h3>

- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop
- You can now log into Client-1 as a normal, non-administrative user now
- Normally you’d want to do this with Group Policy that allows you to change MANY systems at once.


</p>
<br />

<p>
