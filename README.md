![image](https://user-images.githubusercontent.com/109401839/230745596-57cee9bd-687c-427d-b0db-d1080df77f7e.png)

# Azure Setup

## Objectives 
We will commence by establishing our Subscription and Resources, followed by examining Failed Authentication and Log Observation, and concluding with an overview of Azure Active Directory encompassing Users, Groups, and Access Management.

## Environments and Technologies Used 
- Microsoft Azure
- SQL Server
- Event Viewer

## Operating Systems Used
- VM Windows 10 PRO (21H2)

## Resources & SQL Server Vulnerabilties

<details close>

<div>

</summary>

## Actions and Observations<b>

- Create Windows 10 Pro Virtual Machine
- Name the Resource Group: RG-Cyber-Lab

<p align="center">
<img src="https://i.imgur.com/erJwUnm.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Name the Virtual Network: Lab-VNet

<p align="center">
<img src="https://i.imgur.com/5aszFsx.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Now, verify the VM configurations and proceed with the creation! 

<p align="center">
<img src="https://i.imgur.com/IFE9TaZ.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Configure the Network Security Group (Layer 4 Firewall) to permit inbound traffic from all sources.

- By establishing a custom firewall configuration for the virtual machine, we are enabling unrestricted inbound traffic. The intention is to create an enticing environment that attracts potential threat actors, including hackers, bots, and attackers, to attempt unauthorized access to the virtual machine.

- Within the resource groups, navigate to access and examine all associated resources linked to the created virtual machine.

- Proceed to modify the network security group either through search functionality or by accessing the resource groups.

- Evaluate the priority of incoming network traffic based on predefined rules and protocols, categorized within the Azure environment.

- Create an inbound security rule with the designation "Dangerallowallinbound," allowing any type of inbound traffic. 

<p align="center">
<img src="https://i.imgur.com/0kuuxxA.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Let us now proceed to initiate a ping command to the IP Address of the virtual machine within the Command Prompt and carefully observe the resulting outcome.

<p align="center">
<img src="https://i.imgur.com/7q1xfgU.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- You will observe that the ping operation is unsuccessful as we need to modify the firewall settings within the virtual machine itself to allow the necessary access. 

- Now remote into the Windows 10 VM by using Remote Desktop Connection 

<p align="center">
<img src="https://i.imgur.com/H4BFSlS.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Turn off Windows Firewall
 
- After logging in, please utilize the start menu search function to locate and execute the program "Windows Defender Firewall Advanced Security" by searching for "wf.msc"
- Click on "Windows Defender Firewall Properties" 
- On each tab, turn off the "Firewall State" 
- Ignore IPsec Settings for now.

<p align="center">
<img src="https://i.imgur.com/wvIk0aH.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Now observe the changes in CMD: 

<p align="center">
<img src="https://i.imgur.com/B3APjv6.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Install SQL Server Evaluation

- [Download here](https://www.microsoft.com/en-us/evalcenter/download-sql-server-2022)

- Install .exe file > Download Media > ISO option > Open Folder > Mount Media

- It will show as a disk file under the "This PC" side panel: 

- Select the application labeled "setup" to open "SQL Server Installation Center"
 
<p align="center">
<img src="https://i.imgur.com/lUi81uN.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Select Installation > New SQL Server standalone installation or add features to an existing installation > Specify a free edition: Evaluation > Accept licensing > select Database Engine Services > Default instance > Next > select Mixed Mode (SQL Server authentication and Windows authentication) > fill out password > Add Current User > Next > Install
 
 <p align="center">
<img src="https://i.imgur.com/IlIY8kt.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
<p align="center">
<img src="https://i.imgur.com/bIsTOxn.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
<p align="center">
<img src="https://i.imgur.com/kLS1yo3.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

 - Choose the "Mixed Mode" option, as it holds significance in enabling both online and local login capabilities to the SQL Server. This is crucial because, with Windows Authentication Mode alone, logging in is restricted to online accounts, while the mixed mode grants the flexibility to access the SQL Server through both online and local authentication methods.

Default Username: ```sa```
Password: ```Cyberlab123!``` (This is what I used, however you can set any password but need to document it.) 

- Enter your password and add your current user. 

- Once you finish installing, we can now connect to our SQL Database.  

- Next, we will download [Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)

 - Once downloaded, open SQL Server Management Studio and connect to server to enable logging

 <p align="center">
<img src="https://i.imgur.com/PwIREQm.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

 <p align="center">
<img src="https://i.imgur.com/35zYIoL.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

[Configure](https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16) the audit object access setting in Windows using audit-pol

- Enable logging for SQL Server to be ported into Windows Event Viewer 

- Open Command Prompt with administrative permissions.

- From the Start menu, navigate to Command Prompt, and then select Run as administrator.

- If the User Account Control dialogue box opens, select Continue.

- Execute the following statement to enable auditing from SQL Server.

- Windows Command Prompt

- Copy

```audit-pol /set /subcategory: "application generated" /success:enable /failure:enable```

- Close the command prompt window.

 <p align="center">
<img src="https://i.imgur.com/GjZ3dug.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Now open Registry Editor and explore:

 ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security```

 - Right-click Security > select Permissions > Add > type "NETWORK SERVICE" > Check Names > OK > select Full Control > Apply > OK

 <p align="center">
<img src="https://i.imgur.com/bBAJbHR.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
<p align="center">
<img src="https://i.imgur.com/tonrn2T.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Restart SQL Management, Disconnect Connection, Reconnect, and Choose SQL Managements Authentication Method. 

- Now, intentionally enter the wrong username and password to perform a failed login attempt. 

<p align="center">
<img src="https://i.imgur.com/8YIVXcn.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Test SQL logging to make sure it’s working properly.

- Open Enter Event Viewer > Windows Logs > select Application > view SQL Management Logs Entries

<p align="center">
<img src="https://i.imgur.com/dJYTuec.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

 - Here we can see the failed login attempt and the reason. With that, we bring the first lab to a conclusion. 

## Precursor to Security Operations (Failed Authentication and Log Observation)


<details close>

---

</summary>

To facilitate our investigation, we will establish a virtual machine (VM) in the cloud as the focal point for conducting the attack. Subsequently, we will closely analyze the logs generated by this VM to gain a comprehensive understanding of their structure and characteristics. The primary objective of this laboratory exercise is to effectively distinguish between false negatives, false positives, true positives, and true negatives, thus achieving our ultimate goal. 
  
<b>Actions and Observations<b>

- Our objective is to simulate a threat by creating an attack VM in a distinct region to mimic an external attack targeting our Windows-based VM. 

<p align="center">
<img src="https://i.imgur.com/TSsZ0bu.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

``` We will name the VNet Lab-VNet-Attacker```

<p align="center">
<img src="https://i.imgur.com/dSehf4i.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Firstly, we will retrieve the public IP address of the attack VM. Next, we will access the remote desktop connection and input the necessary information to establish a connection with the attack VM.

<p align="center">
<img src="https://i.imgur.com/zG4deDa.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- To proceed, obtain the public IP address of the Windows VM. Then, navigate to the Remote Desktop Protocol (RDP) and access the Start menu. From there, search for "Remote Desktop" and enter the acquired IP address to establish a connection. 

- At this stage, we will generate a series of failed Remote Desktop Protocol (RDP) logs against the Windows VM using the attacker VM. 

- From within the attack VM, our next step involves attempting to establish a Remote Desktop Protocol (RDP) connection with the Windows VM using incorrect credentials. We will perform this action five times, employing different combinations of incorrect usernames and passwords, to simulate multiple failed login attempts against the Windows VM.

<p align="center">
<img src="https://i.imgur.com/lC40zVG.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 <p align="center">
<img src="https://i.imgur.com/5ouvxDV.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Following the aforementioned failed login attempts, we will proceed to access the Event Viewer, where we can review and examine all the recorded instances of unsuccessful login attempts.

 <p align="center">
<img src="https://i.imgur.com/ogP9vcU.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- In the subsequent step, we will install SQL Server Management Studio (SSMS) within the attack VM. Using SSMS, we will then generate a series of failed Microsoft SQL Server authentication logs against the Windows VM.
- Enter the wrong password 5 times

 <p align="center">
<img src="https://i.imgur.com/mubEEHJ.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Log out of the attack VM and return to our local computer.
- Establish an RDP connection back into the Windows VM from our computer.
- Inspect the failures and successes recorded in the Security log for RDP and the Application log for SQL.
- Take note of essential details such as EventIDs, messaging, source IP addresses, and other pertinent information to facilitate log analysis and understanding.

 <p align="center">
<img src="https://i.imgur.com/O7bCHdE.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
<div>

<h3>Azure Active Directory Overview (Users, Groups, and Access Management)<h3>

<details close>

---

</summary>

![Untitled](https://user-images.githubusercontent.com/109401839/230747442-f0a1831d-1cf0-4895-b335-372314cd5d51.png)

<b>Actions and Observations<b>

- Configure and Observe Tenant-Level Global Reader
1. Create a user in Active Directory, we will name the user "globalreaderjohn"
Then we will select the auto password to generate the option, Maxo1396" 
```It will be different for you``

![image](https://user-images.githubusercontent.com/109401839/230799438-00d3e9fe-4348-4052-9995-6d6895f6f283.png)

![image](https://user-images.githubusercontent.com/109401839/230799569-fca3562d-15c4-4332-9e30-0e75432e7e96.png)

- Assign Tenant-Level Global Reader

![image](https://user-images.githubusercontent.com/109401839/230799619-da680846-c56a-479a-b215-ab5758f49b50.png)

![msedge_Y3BrhP8v9T](https://user-images.githubusercontent.com/109401839/230799861-29ebd5fd-4b1d-445f-a9da-abfd1056a726.png)

Be sure to copy your user's "User Principal Name",  for us that is ```globalreaderjohn@fnabeelpm.onmicrosoft.com```

- In a new browser/incognito, log in as globalreaderjohn and observe the result of being a Tenant Level “Global Reader”

 [Login to Azure](http://portal.azure.com/) 

![msedge_HqzxFbf0iN](https://user-images.githubusercontent.com/109401839/230799958-e166ab3b-f43a-4ffc-9042-c836cf5c3ec2.png)

 Azure will prompt you to change your auto-generated password, for us we changed it to ```LabTest123456```, feel free to use anything but be sure to remember it. 

Once you are logged into Azure, notice you can not see anything on your subscription page, however, you can view all available users. 

Due to your role, as Global Reader. I can view users' overviews, however, I am not able to make changes or reset passwords. 

![image](https://user-images.githubusercontent.com/109401839/230800090-ab7e025f-079f-41f5-a8f4-10cf7dbd5676.png)

This is because we have given RBAC (Role-Based Access Control) and enforced the Least Privileges so John can only do his job, Read. 

- Close browser/incognito when satisfied

- Back in the main browser, create another user within AAD  (username: subreaderjane)

- Configure and Observer Subscription Reader

- Auto-generated password, ```Qudo6437```  

![msedge_JftbufmM3X](https://user-images.githubusercontent.com/109401839/230800294-58e9a6f0-984e-435a-8db1-41a5cbbd8523.png)

- Assign Subscription-Level Reader 

This may be called something different for you, for me ``` Azure subscription 1``` and you can find this under ```Subscriptions```. 

Now, Enter the Access Control (IAM) and give Jane the deserved role. 

![image](https://user-images.githubusercontent.com/109401839/230800962-a70cbdaf-c686-41ec-9313-f8dc9cb0fd9e.png)

![msedge_UUzHPizmTA](https://user-images.githubusercontent.com/109401839/230800551-c809f8b7-975f-4a3b-8761-ef7e7c6fe5fb.png)

- In a new browser/incognito, log in as subreaderjane and observe the result of being a Subscription Level “Global Reader”

Again, you will be prompted to change your password and document it. 

Go to resource groups and notice you can see the resources in there and even under subscriptions. 

Let us try to delete a resource group now, we should not be able to do so... 

![image](https://user-images.githubusercontent.com/109401839/230801715-c9537447-9f66-4f22-8d33-19eab3074bd2.png)

Did it delete? YIKES.

![image](https://user-images.githubusercontent.com/109401839/230801752-80f0c485-cf6b-40d5-ac6d-de5b9cedf301.png)

It did not delete! Jane does not have the privileges to create or delete. Only a subscription-level reader. We can not change anything. 

- Close browser/incognito when satisfied

- Configure and Observe Resource Group Contributor (like an admin)

- Back in the main browser, create another user within AAD  (username: rgcontributordave)

Auto-generated password ```Fayo1701```

- Create a new resource group called “Permissions-Tester”

- Assign Resource Group-level Contributor

- For our resource group (RG-Cyber-Lab), assign Contributor Permissions

![image](https://user-images.githubusercontent.com/109401839/230802793-5cd508ee-80f4-4ec5-a550-a9bcfee016b6.png)

- In a new browser/incognito, log in as rgcontributordave and observe the result of being a Subscription Level Reader
 
![msedge_0h9H4b5cvQ](https://user-images.githubusercontent.com/109401839/230802957-c2564141-5e21-4f06-8566-70b1464c40ce.png)

- Observe the result of being a Resource Group Level Contributor

![image](https://user-images.githubusercontent.com/109401839/230803072-730eeeb8-3051-4173-9a19-44d513cddb56.png)

Dave is now able to view the resource group and create further resources in the group such as Storage. 
 
 ![image](https://user-images.githubusercontent.com/109401839/230803808-2d32b1ba-4312-4cc9-99a3-26d158587e0f.png)

```Be sure  NOT to delete your resources``` we will continue with the same RG & VM. 

That concludes the three-part labs series, *Welcome to Cybersecurity*, your journey starts here! 

On our next set of [labs](https://github.com/fnabeel/Logging-and-Monitoring), we will go over Logging and Monitoring. 
