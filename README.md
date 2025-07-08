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















