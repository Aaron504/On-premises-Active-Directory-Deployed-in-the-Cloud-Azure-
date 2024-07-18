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

- Setup Resources in Azure
- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
- Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
![Screenshot 2024-07-17 111801](https://github.com/user-attachments/assets/c0271a94-2b2b-42d7-8b26-3144f3181985)
![Screenshot 2024-07-17 111821](https://github.com/user-attachments/assets/597def86-7c04-4214-b084-55648ac7f10d)
![Screenshot 2024-07-17 112641](https://github.com/user-attachments/assets/7e3892f7-f85e-4b03-94dc-cba95a2988a4)

- Ensure Connectivity between the client and Domain Controller
- Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
- Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
- Check back at Client-1 to see the ping succeed
![Screenshot 2024-07-17 120458](https://github.com/user-attachments/assets/fc6ad180-efdd-4dda-b8b5-f62c01d82a95)
![Screenshot 2024-07-17 122212](https://github.com/user-attachments/assets/fffbac76-ad11-4fe5-b6cb-1964c09f3381)

- Install Active Directory
- Login to DC-1 and install Active Directory Domain Services
- Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
- Restart and then log back into DC-1 as user: mydomain.com\labuser
![Screenshot 2024-07-17 145212](https://github.com/user-attachments/assets/2ea0e8d5-4e85-4e0d-a10a-3d504e27e0f0)
![Screenshot 2024-07-17 150118](https://github.com/user-attachments/assets/3fd50136-8fd8-4854-bdff-2fee83570569)

- Create an Admin and Normal User Account in AD
- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
- Create a new OU named “_ADMINS”
- Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
- Add jane_admin to the “Domain Admins” Security Group
- Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
- User jane_admin as your admin account from now on
![Screenshot 2024-07-17 152029](https://github.com/user-attachments/assets/64f9424c-d024-4c20-b8d5-b36502d09354)
![Screenshot 2024-07-17 152529](https://github.com/user-attachments/assets/4a242ed8-576c-450d-b0dd-1eb7e3be5913)
![Screenshot 2024-07-17 153022](https://github.com/user-attachments/assets/d9b76654-ef78-4e33-b6d0-f0c79f07c787)
![Screenshot 2024-07-17 153510](https://github.com/user-attachments/assets/1d36aca0-67f5-4946-90eb-8e5464870d4a)
![Screenshot 2024-07-17 154116](https://github.com/user-attachments/assets/65d2e442-bc55-4cae-a41e-9f6a07621b26)

- Join Client-1 to your domain (mydomain.com)
- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
- Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)
![image](https://github.com/user-attachments/assets/a3acb21c-656b-4888-b304-2fe8fd6746fd)
![Screenshot 2024-07-17 155945](https://github.com/user-attachments/assets/a1561b23-e3ac-4442-882e-21fd4183bacd)
![image](https://github.com/user-attachments/assets/5538a231-5b08-4363-a922-f40f525ba92a)
![Screenshot 2024-07-17 174058](https://github.com/user-attachments/assets/75f78c9a-f80b-46fe-9d01-316d815957c6)
![Screenshot 2024-07-18 134817](https://github.com/user-attachments/assets/b6e6c2a7-0961-4489-b34b-fb386b4fdf6c)

- Setup Remote Desktop for non-administrative users on Client-1
- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop
- You can now log into Client-1 as a normal, non-administrative user now
- Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)
![Screenshot 2024-07-18 135548](https://github.com/user-attachments/assets/0c4d696c-1f6a-4cff-9cce-249271afac41)
![Screenshot 2024-07-18 135708](https://github.com/user-attachments/assets/bf1d9e7a-069c-4a67-a7f8-98e5e2424fb2)
![Screenshot 2024-07-18 135854](https://github.com/user-attachments/assets/be08f3b5-d9a2-41ff-9f98-9eac3f486a7f)
![Screenshot 2024-07-18 140138](https://github.com/user-attachments/assets/07deceda-542d-4b01-a792-cff3e8b70046)
![Screenshot 2024-07-18 140556](https://github.com/user-attachments/assets/b887120a-e835-4cfa-814e-4c5a379e1075)

- Create a bunch of additional users and attempt to log into client-1 with one of the users
- Login to DC-1 as jane_admin
- Open PowerShell_ise as an administrator
- Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
- Attempt to log into Client-1 with one of the accounts (take note of the password in the script)

![Screenshot 2024-07-18 141013](https://github.com/user-attachments/assets/68b5a88d-9848-4d3b-bfe3-3b3c525c4d37)
![Screenshot 2024-07-18 141610](https://github.com/user-attachments/assets/2b101a4d-4e3b-4b7c-b656-4166ba32414a)
![Screenshot 2024-07-18 141853](https://github.com/user-attachments/assets/01d51699-27f2-46b1-8391-9be541b29b91)
![Screenshot 2024-07-18 142232](https://github.com/user-attachments/assets/742840ff-9ced-4aab-b6fa-e703190b9462)
From the script, all of the new users are being placed into the _EMPLOYEES folder in Active Directory.
![Screenshot 2024-07-18 142452](https://github.com/user-attachments/assets/5edc909b-22b3-4eb4-bb97-c01b0bdb00a4)
Logging in with one of the random users from the list.
![Screenshot 2024-07-18 142916](https://github.com/user-attachments/assets/52da5faa-4765-4ddf-aae1-1d8b39084768)



































