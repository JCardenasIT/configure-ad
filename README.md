<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial explains how to set up on-premises Active Directory in Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems </h2>

- Windows Server 2022
- Windows 10


<h2>Configuration and Deployment Steps</h2>

<p>
</p>
<p>
In this lab, we will create two virtual machines (VMs) in the same virtual network (VNET). One VM will act as a Domain Controller (DC), and the other will be a Client. The DC will provide Active Directory services, so we’ll set its IP address to static. The Client will be joined to the domain, and we’ll update its DNS settings to use the DC as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 needs to have a static private IP address. To check the connection, we’ll try to ping DC-1 from Client-1. At first, the ping will fail because the firewall is blocking it. To fix this, we need to enable ICMPv4 (ping) on DC-1’s firewall. After that, the ping will work, confirming the connection.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next, log back into DC-1 and install Active Directory Users and Computers. Then, promote the VM to a Domain Controller by creating a new forest named "mydomain.com". After the setup, restart the VM and log in as:
mydomain.com\labuser

If you followed the steps correctly, you should now be able to open and use Active Directory Users and Computers.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Great! Now we can start creating Organizational Units (OUs).

    First, create an OU named _EMPLOYEES.

    Then, create another one named _ADMINS.

To do this, right-click on your domain name, choose New > Organizational Unit, and enter the name.

Next, go into the _ADMINS OU, right-click, select New > User, and create a new user:

    Name: Jane Doe

    Username: Jane_admin

Since Jane is an admin, add her to the Domain Admins group. 
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From now on, you can use Jane_admin as your administrator account.

Next, we’ll join Client-1 to the domain (mydomain.com).

    In the Azure portal, change Client-1’s DNS settings to use the private IP address of DC-1.

    Then, restart Client-1 from the Azure portal.

Once that’s done, Client-1 should be using DC-1 as its DNS server. 
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
Now we need to join Client-1 to the domain.

    On Client-1, go to System Settings, then click on About.

    On the right side, click Rename this PC (advanced).

    Choose the option to change the domain.

    Enter mydomain.com.

    When prompted, enter the credentials: mydomain.com\labuser.

    After that, the computer will restart, and Client-1 will be joined to the domain.
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Great! Client-1 is now part of the domain.

Next, we’ll set up Remote Desktop so that regular (non-admin) users can log in.

    Log into Client-1 as an admin.

    Open System Properties and go to the Remote Desktop section.

    Allow Domain Users to have Remote Desktop access.

Once that’s done, you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To make sure regular users can RDP into Client-1, we'll run a script to create thousands of users in the domain. After the users are created using PowerShell, we'll pick one user and try RDPing into Client-1.

</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
The PowerShell script created a user with the username "bab.hubo." We were able to log in to Client-1 using his credentials as a regular user.
 
</p>
