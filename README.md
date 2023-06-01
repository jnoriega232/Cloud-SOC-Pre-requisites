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

```We will name the VNet Lab-VNet-Attacker```

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
- Generate a user account in Active Directory with the username "globalreaderjohn" and utilize the auto-generated password feature, as we will reset the password upon logging in.
- Assign Tenant-Level Global Reader

 <p align="center">
<img src="https://i.imgur.com/oFG6ih0.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

 <p align="center">
<img src="https://i.imgur.com/CaxKf3q.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

 <p align="center">
<img src="https://i.imgur.com/fo0VIsx.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Be sure to make note of the "User Principal Name" for your user, which in my case is ```globalreaderjohn@jnoriega232gmail.onmicrosoft.com```.
- In a new browser/incognito, log in as globalreaderjohn and observe the result of being a Tenant Level “Global Reader”

 [Log into Azure](http://portal.azure.com/) 

 <p align="center">
<img src="https://i.imgur.com/NW1mLSh.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Once in Azure, you will be prompted to change the automatically generated password. Please feel free to set a password of your choice, but make sure to remember it.
- Upon logging into Azure, you may observe that the subscription page appears empty, but please note that you will be able to view all the available users. 
- As a Global Reader, your role grants you access to view users' overviews but does not allow you to make changes or reset passwords.
- This is because we have implemented Role-Based Access Control (RBAC) and enforced the principle of Least Privilege. As a result, John's role as Global Reader only grants him the necessary privileges to perform his job function, which is limited to reading information and not making any changes or resetting passwords.
- Close browser/incognito when satisfied

 <p align="center">
<img src="https://i.imgur.com/obQ8CHq.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Back in the main browser, create another user within AAD  (username: subreaderjane)
- Assign Subscription-Level Reader 
- Configure and Observe Subscription Reader

 <p align="center">
<img src="https://i.imgur.com/Z6J0Lou.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 
- Next, navigate to your specific subscription, which in this case is labeled as ```Azure subscription 1```. You can locate this option under the ```Subscriptions``` section.
- Now, enter the Access Control (IAM) and give Jane the appropriate role. 

 <p align="center">
<img src="https://i.imgur.com/yTUKa1J.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 <p align="center">
<img src="https://i.imgur.com/kZlOMaZ.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- In a new browser/incognito, log in as subreaderjane and observe the result of being a Subscription Level “Global Reader”

 <p align="center">
<img src="https://i.imgur.com/B9i8Sep.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Again, you will be prompted to change your password and document it. 
- After logging in, proceed to the "Resource Groups" section, where you will be able to view the resources contained within. Additionally, you will also be able to explore the resources categorized under your subscriptions. 
- Now, let's attempt to delete a resource group. However, please note that we should not be able to do so due to the access restrictions in place.
- The deletion attempt was unsuccessful. Jane does not possess the necessary privileges to create or delete resource groups. She is restricted to being a subscription-level reader, which means she cannot make any changes to the resources or settings.
- Close browser/incognito when satisfied

  <p align="center">
<img src="https://i.imgur.com/bD18Mf1.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 <p align="center">
<img src="https://i.imgur.com/d6yMo7X.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Next, let's configure and observe the role of Resource Group Contributor, which functions similarly to an administrative role.
- Back in the main browser, create another user within AAD  (username: rgcontributordave)

 <p align="center">
<img src="https://i.imgur.com/UV0YOIa.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- We will create a new resource group called “Permissions-Tester”
- Assign Resource Group-level Contributor
- In our resource group (Permissions-Tester), enter Access Control (IAM) and assign Contributor permissions to rgcontributordave
- Find "Contributor" role under "Privileged administrator roles"

 <p align="center">
<img src="https://i.imgur.com/SlF9rdC.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 <p align="center">
<img src="https://i.imgur.com/TwX42sW.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 <p align="center">
<img src="https://i.imgur.com/XR8z5QS.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- In a new browser or incognito window, log in as "rgcontributordave" and observe the outcome of having a Subscription Level Reader role. 

 <p align="center">
<img src="https://i.imgur.com/d7ecDoP.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>

- Take a moment to observe the outcome of having the role of Resource Group Level Contributor.
- Now, Dave has the capability to view the resource group and create additional resources within it, such as Storage.
- In "Permissions-Tester", we will create a storage account called "observecapability" for observation.

 <p align="center">
<img src="https://i.imgur.com/o1mRjjq.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p>
 <p align="center">
<img src="https://i.imgur.com/eP0XlWB.png" height="70%" width="70%" alt="Azure Free Account"/> 
</p> 

```Be sure  NOT to delete your resources``` we will continue with the same RG & VMs. 

Congratulations on completing the three-part lab series, "Welcome to Cybersecurity"! This marks the beginning of your exciting journey in the field of cybersecurity. 

On our next set of [labs](https://github.com/jnoriega232/Logging-and-Monitoring), we will go over Logging and Monitoring. 
