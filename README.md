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

- Deploy Resource Groups and Virtual Network
- Create Virtual Server and Windows Client
- Configure Virtual Server Static IP and Firewall Settings
- Configure Client-1 DNS and Ping Virtual Server

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/3G99k27.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/g5OnpRx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Firsty, we are going to create a resoure group as well as a virtual network names Active Directory Vnet. This is where the VM's will be created and where the network used to communicate between the two is. Next we will create two Virtual machines: the first is our Domain controller. It will run windows server database instead or regular windows 10 and the network it will use is the AD Vnet network we just created. Then we will create our second Virtual machine named "client-1" this will just run regular windows 10 on the AD Vnet server. Once these both are deployed, we will now began configuring them.
</p>
<br />

<p>
<img src="https://i.imgur.com/0Hea2pA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bpSDY3g.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Starting with DC-1, we need to set the private IP address to be static so Client-1 can access it. We'll do so by going into dc-1, going to network settings, and configure the NIC to have a set static IP. Great, now that that is done we will remote connect to DC-1 and turn off the firewall. You shouldnt do this in a regular setting but for the sake of this project and connecting client-1 to the active directory we will. 
</p>
<br />

<p>
<img src="https://i.imgur.com/2wBoqGc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/ix9OIib.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After we finished configuring DC-1 settings we will set Client-1's DNS to DC-1's private IP address. We will change that by going to client-1's Virtual NIC card on Microsoft Azure and instead of the IPv4 tab we will go into the DNS tab and select custom option and paste DC-1's private IP address in. Once in we can restart the VM for Client-1 and then log in and ping DC-1's private address in Microsoft powershell. And it worked! Also we can do "ipconfig /all" to make sure the dns for client-1 is the private IP address for Dc-1. So now we have the basic foundation for this lab complete!
</p> 
<br />
<br />
<h2>Part 2: Connecting VM's and Configuring Domain Users </h2>
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

- Deploy Resource Groups and Virtual Network
- Create Virtual Server and Windows Client
- Configure Virtual Server Static IP and Firewall Settings
- Configure Client-1 DNS and Ping Virtual Server

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/qqXmYdU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/4JUbCtE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To begin, I launched the VM for DC-1, which serves as the Windows Server designated to act as the domain controller. I installed Active Directory Domain Services to provide the necessary tools for deployment. After installation, I promoted DC-1 to a domain controller by configuring a new forest, mydomain.com. This process officially established DC-1 as the domain controller for the environment, enabling centralized management of users and resources.
</p>
<br />

<p>
<img src="https://i.imgur.com/tM4k18u.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/RTNdLsB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After logging in as mydomain.com\labuser on the domain controller, I used Active Directory Users and Computers (ADUC) to create two Organizational Units: _EMPLOYEES and _ADMINS. These OUs serve as containers for domain user accounts. I then created a user account named Jane_Doe, assigned administrative privileges, and placed her in the _ADMINS OU. This account was used throughout the rest of the project to manage and test domain configurations.
</p>
<br />

<p>
<img src="https://i.imgur.com/SIJzvSH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/ZDugcvG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, I joined Client-1 to the domain so that future users could access it. From the computer settings, I navigated to the About tab, selected Rename PC, and added the system as a member of mydomain.com. The system prompted for admin credentials, and I authenticated using the Jane_Doe account. Once Client-1 was successfully joined to the domain, I configured the domain controller to allow non-admin users to log in. To do this, I signed in to Client-1 as Jane, opened System Properties, navigated to Remote Desktop, and granted domain users access. This ensured that standard domain users, not just administrators, could log into the domain environment.
</p> 

<p>
<img src="https://i.imgur.com/mseCG2v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/1Yqwjk9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/fSeqaBm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To simulate an organization with a larger user base, I created multiple accounts using Microsoft PowerShell. From DC-1, I opened PowerShell ISE as an administrator and ran a script to automatically generate user accounts. Each account was provisioned with a default password, and all were able to log in with their own individual profiles. After the script executed successfully, I verified the setup by signing in with a randomly selected account, confirming that the domain was properly handling multiple user logins.
</p> 
<br />


