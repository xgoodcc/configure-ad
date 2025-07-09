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

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup a Domain Controller in Azure
- Setup a Client computer in Azure
- Join the Domain as the Client
- Create users to login to the Client machine

<h2>Deployment and Configuration Steps</h2>

Start by creating a Resource Group in Microsoft Azure; name it, set the Region to the region you reside, and click "Review + create" and then "Create".
![image](https://github.com/user-attachments/assets/5a14222e-2b6c-470e-afd8-34fe6e2808b4)

Create a Virtual network; set the Resource group to the one that you just created, name it, make sure the region has not changed, and click "Review + create" and then "Create".
![image](https://github.com/user-attachments/assets/9e00157a-3ccd-4402-b876-48225af5f2c8)

Create the Domain Controller virtual machine; select the Resrouce group you created earlier, name it (for this example, "dc-1"), make sure the Region is the same as the Virtual network's region created earlier, set the Image to Windows Server 2022 x64, set the Size as any that has at least 2 vcpus, and set a username and password (for this example, "labuser" and "Cyberlab123!" respectively).
![image](https://github.com/user-attachments/assets/1fc04797-9544-4bfe-8dc0-0917267c56e6)
![image](https://github.com/user-attachments/assets/042cd18a-a9d7-449d-b056-df411a056884)

Make sure to check the Licensing box and the box that follows.

![image](https://github.com/user-attachments/assets/50bfcf91-841e-4666-82b5-de6f10030799)

Click "Next" twice to get to the Networking tab; set the Virtual network to the one that you created earlier if it is not already, leave everything else as is, click "Review + create", and then click "Create".

![image](https://github.com/user-attachments/assets/fbb92b8c-0033-482d-9f27-eede131d341e)

Set the Domain Controller's NIC Private IP Address to be static; click on the name of the Domain Controller, go to the Networking dropdown menu and click Network settings.
![image](https://github.com/user-attachments/assets/121ae5f4-a396-49a2-9cb1-7817e2ac081e)

Click on the Network interface/IP configuration hyperlink, click the name of the IP configuration, check the "Static" box under Private IP address settings, and click "Save".
![image](https://github.com/user-attachments/assets/f7cbfbee-f9df-4f35-beaa-9e3fc81ed317)

Log into the Domain Controller virtual machine and disable Windows Firewall. This is strictly for the lab purpose of test connectivity.
Open your remote desktop application, copy the Public IP Address of the Domain Controller and paste it into the "PC Name" box of remote desktop.
![image](https://github.com/user-attachments/assets/a893597f-f03f-4162-ac0a-5caeae5274a3)

Connect using the credentials established earlier (labuser and Cyberlab123!); click "Connect", select "More choices", and then click "Use a different account".
![image](https://github.com/user-attachments/assets/457170b4-1328-41f9-ab05-f8768793be78)

Click "Ok" and then click "Yes" on the next pop-up.
![image](https://github.com/user-attachments/assets/3e756dbb-7ed2-45cc-b9c9-7ee7c26ef868)

Once you are logged in, right click the Windows icon in the bottom left, select run, type "wf.msc", and click "Ok".
![image](https://github.com/user-attachments/assets/fcdea7e9-a982-4122-b158-b8cf3365192e)

Click "Windows Defender Firewall Properties", set the Firewall state to off in the Domain Profile and Private Profile, change Inbound connections to allow in the Public Profile, click "Apply" and then "Ok".
![image](https://github.com/user-attachments/assets/1457a759-e9d2-4dfc-b243-757d6fd5ec37)
![image](https://github.com/user-attachments/assets/126adc03-b83d-4c9b-86dc-8ccbf0ba54c6)
![image](https://github.com/user-attachments/assets/a5cb9291-f504-4315-94ef-a8d150060df2)

To create the Client machine, repeat the process above, **EXCEPT** change the name (for this example, "client-1) and set the image to Windows 10 Pro.

![image](https://github.com/user-attachments/assets/a816e492-e319-43c7-a440-282b11a24985)
![image](https://github.com/user-attachments/assets/24525874-ed09-42b6-8768-3a8de4147af6)
![image](https://github.com/user-attachments/assets/7ed66062-ae7b-4d8b-9b40-1b1aeea09c4f)

Set the DNS server of the Client machine to the Private IP Address of the Domain Controller; go to the Domain Controller, locate and copy the private IP address.
![image](https://github.com/user-attachments/assets/92230507-8def-4197-b25c-55647b6f6a9b)

Go to the Client machine, go to Networking > Network settings > click the Network interface/IP configuration link
![image](https://github.com/user-attachments/assets/aaaf631c-c834-4bff-a4f1-492306079f48)

In the Settings dropdown menu, go to DNS servers, set DNS servers to Custom, paste the private IP address of the Domain Controller, and click "Save".
![image](https://github.com/user-attachments/assets/ce9f09fc-01f0-49e2-9d99-a8122535366b)

Restart the Client machine; go to Virtual machines, click the box next to the Client machine and click "Restart". This ensures that the DNS settings were applied properly.
![image](https://github.com/user-attachments/assets/43ae600a-eae7-47dc-9f8e-a75026e70c1c)

Login to the Client machine and attempt to ping the Domain Controller's IP address; copy the Public IP Address of the Client machine, paste it into the name box of your remote desktop app, and sign in with the credentials established earlier.
![image](https://github.com/user-attachments/assets/59f1d7c8-8b36-4d49-9c94-06b121b975be)
![image](https://github.com/user-attachments/assets/0af88883-5822-4eb5-a7f5-5b528d3bd667)

When logging in, set "No" to all privacy settings and click "Accept".
![image](https://github.com/user-attachments/assets/9bf02d28-238f-4494-925c-3c03e0f35b96)

Once logged in, open PowerShell.
![image](https://github.com/user-attachments/assets/2c6b14e8-af37-421a-9429-2800f58e3fe4)

Type the command "ping", enter the private IP address of the Domain Controller (locate it again if needed), and monitor the result; a successful ping will look as such:

![image](https://github.com/user-attachments/assets/9613fb2c-57af-413a-b463-0d9fdcf493d8)

Type the command "ipconfig /all" and check to be sure that the DNS settings output is the Domain Controller's private IP address.
![image](https://github.com/user-attachments/assets/d59238fe-0445-4d2d-b52f-f27d3b651a8c)

<h2>Installing Active Directory</h2>

Log into the Domain Controller and install Active Directory Domain Services; if Server Manager is not already open, click Start and open it. In Server Manager, click "Add roles and features".

![image](https://github.com/user-attachments/assets/a4b56599-5de4-411f-916d-6058d143a79e)

Click "Next" on the first two screens; on Server Selection, make sure the name of the system you are working on is selected (for this example, it is dc-1) and click "Next".

![image](https://github.com/user-attachments/assets/fb802c02-e223-43b5-ac72-448e833c9fb2)


On Server Roles, select Active Directory Domain Services, click "Add Features" when the Add Roles and Features Wizard pops up, and click "Next".

![image](https://github.com/user-attachments/assets/6e8cd28d-b42b-4276-9b0b-5f6eed69bc94)

Click "Next" on the next two screens; on confirmation, check "Restart the destination server automatically if required", and click "Install". Once the install is finished, click "Close".
![image](https://github.com/user-attachments/assets/82e57e66-a51e-42a7-8459-af3df0a3e477)

Promote this server to Domain Controller; in Server Manager, check the top right of the window by the flag icon for a caution symbol. Click on it and click "Promote this server to Domain Controller".

![image](https://github.com/user-attachments/assets/8694eed6-75fb-48dc-9e38-52b61a310124)

On Deployment Configuration, select "Add new forest"; you can set it to whatever you want, but for this example, it will be **mydomain.com**. Click "Next" after adding the new forest.
![image](https://github.com/user-attachments/assets/50dedd8e-1525-462e-a6b6-9ecc1dbd14e3)

On Domain Controller Options, set a password and click "Next".
![image](https://github.com/user-attachments/assets/6c080d10-ff67-4b26-88c2-abf51fdabd89)

On DNS Options, make sure "Create DNS delegation" is unchecked and click "Next".
![image](https://github.com/user-attachments/assets/5ff84051-e6a7-4827-8368-95c90decc318)

Click "Next" until you reach Prerequisites Check and click "Install"; the virtual machine will restart and you will not be able to log back in the same as we did before. We will now have to log in with the domain preceding the username (for this example, mydomain.com\labuser). On your Remote Desktop app, use a different account to sign in and use the new credentials; the password will be the same.

![image](https://github.com/user-attachments/assets/42597ed9-14ac-4d0d-80ed-66094810de48)

<h2>Create a Domain Admin user within the domain</h2>

Open Active Directory Users and Computers and create an Organizational Unit (OU); Right-click the domain and go to New > Organizational Unit.

![image](https://github.com/user-attachments/assets/fff1996d-17a7-4d73-8d63-364c3b1f5a50)

Name it "_EMPLOYEES"; this is crucial, so **DO NOT** mistype the name. Click "Ok" when finished.
![image](https://github.com/user-attachments/assets/1b039784-64ef-4bda-9eeb-be0de8dfeeb7)

Create another OU called "_ADMINS".

![image](https://github.com/user-attachments/assets/27d00e4e-6ebc-4c30-abfa-e7f7771a085e)

In _ADMINS, create a new user named "Jane Doe" and set the username as "jane_admin".

![image](https://github.com/user-attachments/assets/fec54bb3-1daf-4025-81f7-d50725dc5c4e)

Set the password to the same one used to log into the virtual machine. For this example, leave all boxes unchecked except "Password never expires".

![image](https://github.com/user-attachments/assets/9fd4777b-b755-424c-92ec-da3aa2052d0a)

Add "jane_admin" to the "Domain Admins" Security Group; in _ADMINS, right-click Jane's profile and go to Properties. Go to Member Of and click "Add".

![image](https://github.com/user-attachments/assets/50288dd4-0301-4795-b622-f0c52dc063da)

Type "domain admins" and click "Check names"; it should now be underlined and autocapitalized. Click Ok > Apply > Ok.
![image](https://github.com/user-attachments/assets/12a3b1ca-ad38-4e5f-b61b-ecdcadc26226)

Log out of the Domain Controller and log back in as this account; the user name will be mydomain.com\jane_admin.

<h2>Joining the Client to the domain</h2>

Log into the client VM as "labuser", which is the original local admin. Right-click the Start menu, go to System > Rename this PC (advanced).
![image](https://github.com/user-attachments/assets/08b8c998-3cde-42db-8e8a-26a538abb432)

In Computer Name, click "Change"; under Member of, click "Domain", type your domain, and click "Ok".
![image](https://github.com/user-attachments/assets/c9a08d3d-3ee8-4c07-beed-b488ad082269)

When Windows Secuirty pops up, enter the credentials for an admin of the domain (for this example, mydomain.com\jane_admin).
![image](https://github.com/user-attachments/assets/0ffd0a63-e0b6-4614-9ad0-de105f1e754a)

A pop-up acknowledging the domain joining will be behind the System settings window; the computer will force restart after you close the System Properties window.
![image](https://github.com/user-attachments/assets/ccaf397b-d354-435c-99f9-165fcbe66b0e)

Back in the Domain Controller, verify that the Client machine now shows up in Active Directory Users and Computers; go to your domain and go to Computers.

![image](https://github.com/user-attachments/assets/940b9718-6617-4aee-9659-1dd4dd474310)

Create a new OU called "_CLIENTS" and drag the Client into it; click "Yes" on the warning pop-up and then right-click your domain and Refresh.
![image](https://github.com/user-attachments/assets/e08ee4df-fa5d-4feb-b598-063f222c3133)![image](https://github.com/user-attachments/assets/2bbcb801-fe84-424a-ac90-074eece51583)

<h2>Setup Remote Desktop for non-administrative users on the Client</h2>

Log into the Client machine as your admin account, open System settings, and click "Remote Desktop".
![image](https://github.com/user-attachments/assets/937d8881-9bff-44e3-9729-ac877d088d4c)

In Remote Desktop, click "Select users that can remotely access this PC".
![image](https://github.com/user-attachments/assets/04e8b9cf-4e79-4c07-b88d-64051932e9c0)

Click "Add", type "domain users", click "Check names", and then click "Ok".
![image](https://github.com/user-attachments/assets/24a9057b-df78-4d04-b7c4-d4eccd4a1962)


<h2>Create users in PowerShell and log into the Client machine with one</h2>

Log into the Domain Controller machine as your admin account and open Powershell_ise **as an administrator**.
![image](https://github.com/user-attachments/assets/6684aadc-aa0e-41c9-bbc3-75f59de1f76c)

In PowerShell ISE, click "New Script" in the top left.
![image](https://github.com/user-attachments/assets/de36741d-93d6-4307-b226-8e8bb2f378c1)

Save this file as "create-users" on the Desktop.
![image](https://github.com/user-attachments/assets/ab913f6c-a480-42f0-83c3-779711c1b639)

Copy and paste the contents of this [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into PowerShell and click "Run script"; click "In the future, do not show this message" on the pop-up that comes next.
![image](https://github.com/user-attachments/assets/95f870c4-5f5c-4405-b280-f06952af072b)

The script will run until it creates 10,000 accounts; for this example, you can stop it short.

In Active Directory Users and Computers, right-click _EMPLOYEES and click refresh and notice all of the new accounts.
![image](https://github.com/user-attachments/assets/af1b9e78-29b4-4f7e-a01f-34319d2e4cc1)

Try to sign into the Client machine as one of these accounts; in your Remote Desktop app, use a different account and type "mydomain.com\(username). The password set by the script for these accounts was Password1.
![image](https://github.com/user-attachments/assets/1f569fd6-8c52-49a4-b626-99be18c0720e)
![image](https://github.com/user-attachments/assets/77246676-179b-426d-b945-8fc00d97f8c4)
![image](https://github.com/user-attachments/assets/4601315d-1f35-445f-8890-86c91b411b91)

Success!










