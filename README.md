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
<img width="1470" alt="Screenshot 2024-07-23 at 7 12 50 PM" src="https://github.com/user-attachments/assets/81d795d5-1b3f-4b70-9ca0-2bc1960e44ff">
</p>
  <h3>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</h3> 
<p>
In the Azure Portal, set up Windows Server 2022 in a resource group named "AD-Lab" or another name for the project. Name the VM "DC-1" for domain conteroller one and put the VM in the resource group.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 6 59 15 PM" src="https://github.com/user-attachments/assets/0bfc8ba9-f446-4510-9bd3-f05f06f30285">
</p>
  <h3>Set Domain Controller’s NIC Private IP address to be static</h3> 
<p>
In the Azure Portal, in DC-1's network settings and select installed NIC and under ip configureations, configure the ip configureation to be static.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 7 20 44 PM" src="https://github.com/user-attachments/assets/2cb18fda-8e9b-45b2-b082-1b01a981eaa0">
</p>
  <h3>Create the Client VM (Windows 10) named “Client-1”</h3> 
<p>
In the Azure Portal, Make a VM named "Client-1" in the AD resource group.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 7 24 38 PM" src="https://github.com/user-attachments/assets/2a83767e-76dd-4c95-bbae-d6f314fcb2ec">
</p>
  <h3>Ensure that both VMs are in the same vnet</h3> 
<p>
Under the network section when creating the VM, under virtual network, select DC-1 network as opposed to makeing a new virtual network with the VM.
</p>
<br />

<!-- --><h2>2. Ensure Connectivity between the client and Domain Controller</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 7 33 36 PM" src="https://github.com/user-attachments/assets/b5704e14-e297-4f71-9ccd-63e3198da61b">
</p>
  <h3>Login to Client-1 with Remote Desktop and ping DC-1</h3> 
<p>
Log in to Client-1 using Remote Connections in windows or using Microsoft's Remote Desktop appliction. To do this, use the public IP address found in the overview of the VM.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 7 48 38 PM" src="https://github.com/user-attachments/assets/82f19bfc-6a6d-4be1-ad1c-02527baeb415">
In Client-1 open CMD and ping the domain controller at the privite ip found in the overview. 
  
  ``` 
  ping -t 10.0.0.4
  ```

  On my lab the domain controller was at 10.0.0.4 but use the ip address found in the overview. It should time out if done correctly.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 7 57 28 PM" src="https://github.com/user-attachments/assets/0b669d9f-4399-41fd-bb30-6a9e79c0e9b6">
</p>
  <h3>Login to the Domain Controller and enable ICMPv4</h3> 
<p>
Now using the same process to log into Client-1, log into DC-1. Once logged in, go into the windows defender firewall with advanced security and enable ICMPv4 echo requests and replies inside the inbound rules.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 7 58 53 PM" src="https://github.com/user-attachments/assets/ab5ac52a-8d43-4166-87da-beaff7d87884">
</p>
  <h3>Check back at Client-1 to see the ping succeed</h3> 
<p>
Go back into Client-1 and ensure that the ping requests are now going through. To stop the ping requests, press CTRL+SHIFT+C.
</p>
<br />

<!-- --><h2>3. Install Active Directory</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-23 at 8 04 36 PM" src="https://github.com/user-attachments/assets/1b51dd45-e098-4af9-82df-bacc31c99e24">
</p>
  <h3>Login to DC-1 and install Active Directory Domain Services</h3> 
<p>
In DC-1, in server manager, go to add roles and features which will open a window, just click next until the window with all the services and select AD lightweight directory services and install it.
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







