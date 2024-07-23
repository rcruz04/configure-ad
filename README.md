<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
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
- Windows 10

<h2>High-Level Deployment and Configuration Steps</h2>

1. Setup Resources in Azure
2. Ensure Connectivity between the client and Domain Controller
3. Install Active Directory
4. Create an Admin and Normal User Account in AD
5. Join Client to your domain
6. Setup Remote Desktop for non-administrative users on Client
7. Create additional users for testing

<!-- --> <h2>1. Setup Resources in Azure</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" />
</p>
  <h3>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" />
</p>
  <h3>Set Domain Controller’s NIC Private IP address to be static</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" />
</p>
  <h3>Create the Client VM (Windows 10) named “Client-1”</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Ensure that both VMs are in the same vnet</h3> 
<p>
subsitution text
</p>
<br />

<!-- --><h2>2. Ensure Connectivity between the client and Domain Controller</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Login to Client-1 with Remote Desktop and ping DC-1</h3> 
<p>
subsitution text
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Login to the Domain Controller and enable ICMPv4</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Check back at Client-1 to see the ping succeed</h3> 
<p>subsitution text</p>
<br />

<!-- --><h2>3. Install Active Directory</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Login to DC-1 and install Active Directory Domain Services</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Promote to a DC</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Restart and then log back into DC-1 as user</h3> 
<p>
subsitution text
</p>
<br />

<!-- --> <h2>4. Create an Admin and Normal User Account in AD</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Create Organizational Unit “_EMPLOYEES”</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Create Organizational Unit “_CLIENTS”</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Create Organizational Unit “_ADMINS”</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Create a new employee</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Add jane_admin to the “Domain Admins” Security Group</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Log out and log back in as jane_admin</h3> 
<p>
subsitution text
</p>
<br />

<!-- --> <h2>5. Join Client to your domain</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Set Client-1’s DNS settings</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Restart Client-1</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Login to Client-1 as labuser and join it to the domain</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Login to the Domain Controller and verify Client-1 shows up in Active Directory</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Move Client-1 into "_CLIENTS" OU</h3> 
<p>
subsitution text
</p>
<br />

<!-- --> <h2>6. Setup Remote Desktop for non-administrative users on Client</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Log into Client-1 jane_admin and open system properties</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Click “Remote Desktop”</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Allow “domain users” access to remote desktop</h3> 
<p>
subsitution text
</p>
<br />

<!-- --> <h2>7. Create additional users for testing</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Login to DC-1 as jane_admin</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Open PowerShell_ise as an administrator</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Create a new File and paste the contents of the script into it</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Run the script and observe the accounts being created</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Observe the accounts being added into OU</h3> 
<p>
subsitution text
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%"/>
</p>
  <h3>Attempt to log into Client-1 with one of the new accounts</h3> 
<p>
subsitution text
</p>
<br />


<!--  end  -->







