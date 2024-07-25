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

1. [Setup Resources in Azure](#1.-setup-resources-in-azure)
2. [Ensure Connectivity between the client and Domain Controller](#2.-ensure-connectivity-between-the-client-and-domain-controller)
3. Install Active Directory
4. Create an Admin and Normal User Account in AD
5. Join Client to your domain
6. Setup Remote Desktop for non-administrative users on Client
7. Create additional users for testing

<h2> 

 ### 1. Setup Resources in Azure</h2>

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

<h2>
  
### 2. Ensure Connectivity between the client and Domain Controller</h2>

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

<h2>3. Install Active Directory</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 14 07 AM" src="https://github.com/user-attachments/assets/289d0091-3cd5-486d-88af-2b8eddefa389">
</p>
  <h3>Login to DC-1 and install Active Directory Domain Services</h3> 
<p>
In DC-1, in server manager, go to add roles and features which will open a window, just click next until the window with all the services and select AD domain services and install it.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 17 05 AM" src="https://github.com/user-attachments/assets/855b6395-0143-425c-9062-73b5d5c8fb7f">
</p>
  <h3>Promote to a DC</h3> 
<p>
Click on the flag in the corner adn click "Premote to DC".
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 18 32 AM" src="https://github.com/user-attachments/assets/43d6f807-d3b2-46ee-909c-6b20fdd53b3c">
</p>
  <h3>Make a new domnain</h3> 
<p>
Select "Add new forest" and type out your new domain. I put "myserver.com", set up DSRM password and click next a few times to install AD. It should reset to install.
</p>
<br />

<p>
<img width="711" alt="Screenshot 2024-07-24 at 10 33 19 AM" src="https://github.com/user-attachments/assets/5586fa46-a949-4db8-adba-c7a8f381eb5d">
</p>
  <h3>Restart and then log back into DC-1 as user</h3> 
<p>
After restarting, the username will change to the domain you set up and the user.

```
myserver.com\adminuser
```
</p>
<br />

<h2>4. Create an Admin and Normal User Account in AD</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 37 13 AM" src="https://github.com/user-attachments/assets/efa9e941-5934-4b0e-a4fe-04241dba3581">
<img width="1470" alt="Screenshot 2024-07-24 at 10 37 42 AM" src="https://github.com/user-attachments/assets/79701887-fdd1-43e9-999b-56b2776c0e3d">
</p>
  <h3>Create Organizational Unit “_EMPLOYEES”</h3> 
<p>
Press the windows key, and type "Active Directory Users and Computers". After it opens select the domain, then right-click and slect "New", then "Orgainizational Unit". After4 name the Organizational Unit "_EMPLOYEES"</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 43 08 AM" src="https://github.com/user-attachments/assets/efebe8eb-51a4-46f6-bff0-cc55de54b136">
</p>
  <h3>Create Organizational Unit “_CLIENTS”</h3> 
<p>
In the same domain, create an OU named "_CLIENTS".
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 45 38 AM" src="https://github.com/user-attachments/assets/439c2810-151a-4e46-bfdc-e71a5000bce6">
</p>
  <h3>Create Organizational Unit “_ADMINS”</h3> 
<p>
In the same domain, create an OU named "_ADMINS". See the picture for how the domain root should look like at the end.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 50 13 AM" src="https://github.com/user-attachments/assets/693bfc27-6824-422d-b8aa-3b3cc9c14a06">
</p>
  <h3>Create a new employee</h3> 
<p>
Add a new user named "Jane Doe" in the _ADMINS OU. Make the username "jane.doe_admin"
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 10 54 25 AM" src="https://github.com/user-attachments/assets/3aec6cda-17b6-4e67-a198-ef3450063870">
</p>
  <h3>Add jane_admin to the “Domain Admins” Security Group</h3> 
<p>
Right-click janes user and select "Member of", then click "add", then in the empty text box type "Domain Admins" and click "Check Names"
</p>
<br />

<p>
<img width="711" alt="Screenshot 2024-07-24 at 10 59 34 AM" src="https://github.com/user-attachments/assets/7bdcd1e1-404a-46c8-acb1-420f2ad9867a">
</p>
  <h3>Log out and log back in as jane_admin</h3> 
<p>
Log out of the defult user into janes account with the credentals we created.
</p>
<br />

<h2>5. Join Client to your domain</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 7 11 56 PM" src="https://github.com/user-attachments/assets/3c6d48e7-133c-4a01-a358-9a690f430c86">
</p>
  <h3>Set Client-1’s DNS settings</h3> 
<p>
In azure portal, ago into Client-1's network settings and change the DNS settings under the network card and then DNS settings. Set the DNS server to be the privite IP of the domain controller and click save at the top. In my case its 10.0.0.4 but it can be found in the overview of DC-1 settings.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 7 23 33 PM" src="https://github.com/user-attachments/assets/e526ce28-4794-4395-a774-e488a4ffdf16">
</p>
  <h3>Restart Client-1</h3> 
<p>
From the Azure portal, go to Client-1 and click restart to apply the new DNS settings.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 7 37 42 PM" src="https://github.com/user-attachments/assets/2dcba705-2d1a-407c-b593-0cdaa8098452">
</p>
  <h3>Login to Client-1 as labuser and join it to the domain</h3> 
<p>
In Clien-1, go into the system settings, in the about tab then scroll down to the Advanced system settings. From this menu click the bottem option and in the pop-up select "Member of: Domain" and put the domain of the website we created.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 7 39 35 PM" src="https://github.com/user-attachments/assets/bb5191d7-9623-4a50-b1a3-3d4c5ebd222d">
</p>
  <h3>Login in as jane doe</h3> 
<p>
When prompted to log in, log in as jane doe using the credentals we made earlier. We will have to restart for the change to take effect.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 7 43 37 PM" src="https://github.com/user-attachments/assets/faded2cc-b9c0-4b59-8bff-e0bc8608200f">
</p>
  <h3>Login to the Domain Controller and verify Client-1 shows up in Active Directory</h3> 
<p>
Log in to DC-1 and see if client-1 shows up in the computers tab under our domain. Once varified, right-click on Client-1 and selct move and move it into the _CLIENTS OU.
</p>
<br />


<h2>6. Setup Remote Desktop for non-administrative users on Client</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 8 21 04 PM" src="https://github.com/user-attachments/assets/69401746-2e3c-48e9-9a63-2be24462884e">
</p>
  <h3>Log into Client-1 jane_admin and open system properties</h3> 
<p>
Open the settings and scroll down to "Remote Desktop"
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 8 23 53 PM" src="https://github.com/user-attachments/assets/a266af24-594f-4cab-ac04-8110778602d1">
</p>
  <h3>Click “Users account”</h3> 
<p>
Click "Select users that can remotly access this PC" and a window will pop up.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 8 31 22 PM" src="https://github.com/user-attachments/assets/75efa442-ab80-41ca-b838-652edb28dd3c">
</p>
  <h3>Allow “domain users” access to remote desktop</h3> 
<p>
Click "Add" and then in the empty text box type "domain users" and click "check names" and then click ok.
</p>
<br />

<h2>7. Create additional users for testing</h2>

<p>
<img width="1470" alt="Screenshot 2024-07-24 at 8 46 57 PM" src="https://github.com/user-attachments/assets/6aaf8175-6f34-4a95-8050-ce7f9e81fe8a">
</p>
  <h3>Log into DC-1 and open PowerShell_ise as an administrator</h3> 
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-25 at 12 19 12 PM" src="https://github.com/user-attachments/assets/b969ef70-b631-467b-8e3f-03750ad96449">
</p>
<h3> 
  
Create a new File and paste the contents of [this script](https://github.com/rcruz04/configure-ad/blob/main/Generate-Names-Create-Users.ps1) into it</h3> 
<p>
In the application under the file tab, click new and then a new window should open up. Paste the file listed in the window.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-25 at 12 22 56 PM" src="https://github.com/user-attachments/assets/95055c9e-8e4d-482f-abcf-f0d352770439">
</p>
  <h3>Run the script and observe the accounts being created</h3> 
<p>
Click the play botton at the top to run the applications and in the blue window, users should be created.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-07-25 at 12 30 37 PM" src="https://github.com/user-attachments/assets/6babb1d7-ef89-4fa0-920a-a1fac0bb0bd3">
</p>
  <h3>Observe the accounts being added into OU</h3> 
<p>
Open AD Users and Computers and open the _EMPLOYEES OU and watch as it gets populated with the new users made from the script.
</p>
<br />

<p>
<img width="793" alt="Screenshot 2024-07-25 at 12 35 50 PM" src="https://github.com/user-attachments/assets/f89ce33e-fbee-4084-91a9-88a4a2fe1ae9">
</p>
  <h3>Attempt to log into Client-1 with one of the new accounts</h3> 
<p>
Log in with one of the created users from the script. I used "Babe Kov" with the password "Password1" which is spesified in the script.
</p>
<br />

<h2>
  
This concludes making of On-premise solution for active directory, I continue with network groups in a different [project](https://github.com/rcruz04/azure-network-protocols).</h2>


end







