
<h1>Microsoft Active Directory Home Lab</h1>

 ### [YouTube Demonstration](https://youtu.be/7eJexJVCqJo)

<h2>Description</h2>
A project that sets up an active directory domain controller in a virtual environment that is connected to a local private network with local windows machine connected to it.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell ISE</b> 
- <b>Powershell Random User Generator Script</b>

<h2>Environments Used </h2>

- <b>Oracle Virtual Box</b> 
- <b>Windows 10 ISO</b> 
- <b>Windows Server 2019 ISO</b>


<h2>Program walk-through:</h2>
<h3>High Level Project Overview</h3>
<br/>
It also configures Content Hub solutions
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="1" border="0" /></a>
<br/>
We will install two different Virtual Machines. One is Windows 10 ISO and the other is Server 2019 ISO
<br/>
1st VM is our Domain Controller (Server 2019)
This VM has 2 NICs
1st NIC is connected to the Internet. Will get addressing from Home Router via DHCP
2nd NIC is internal and will be connected to a local Private Network
We will assign IP addresses for the Internal Network 
We will install active directory on the server and create our domain 
We will configure NAT and Routing (RAS)
*Network Address Translation = modify network address information in packet headers. Allows multiple devices
within a local network to share a single IP address when connecting to an Internet*
*Remote Access Service = enables users to connect to a network from a remote location. Users have access to 
resources and services of a private network over a secure connection.
We will set up a DHCP on the Domain Controller so that when we create our Windows 10 machine it will automatically
get an IP address
<br/>
<br/>
Before we create a client VM, we will run a PowerShell script which will automatically create 1000 users in Active 
Directory
<br/>
<br/>
2nd VM is our Windows 10 Client machine
This VM has 1 NIC
The NIC is connected internally and will get addressing from DC via DHCP
This VM will be connected to the Private Virtual Box Network
We will name this VM Client1 and join it to the domain
We will login using one of our domain accounts
<br/>
<br/>
Okay that’s a high level overview. Lets get into the details.





<h3>Step 1 - Setting up our Environment</h3>
Download Virtual Box
Download Windows 10
Download Windows server 2019 - fill out some details
<br/>
<br/>
Create the VM. First the Domain Controller (Server 2019)
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="2" border="0" /></a>
<br/>
Currently I skipped the Unattended Install
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="3" border="0" /></a>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="4" border="0" /></a>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="5" border="0" /></a>

We need to add two Network Adapters. 1st is NAT.
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="6" border="0" /></a>
<br/>
Second Adapter is Internal.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="7" border="0" /></a>
<br/>
Installing the VM.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="8" border="0" /></a>
<br/>
<br/>
Choose Custom Install.
Click Next
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="9" border="0" /></a>
<br/>
After installing 
add password.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="10" border="0" /></a>
<br/>
<br/>
Open with Ctrl + Alt + Delete
got to Input > Insert Ctrl + Alt + Delete
or else you wont be able to unlock.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="11" border="0" /></a>
<br/>
<br/>
Click > Yes
We want other PCs to be discoverable on this network.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="12" border="0" /></a>
<br/>
<br/>
To get rid of the lag and to allow the VM to resize properly 
we need to add guest box additions images
Go to this PC > click on Drive D
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="13" border="0" /></a>
<br/>
<br/>
Click on  VBoxWindowsAdditions-amd64
Click > Install
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="14" border="0" /></a>
<br/>
<br/>
Reboot later. However Shut the VM down.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="15" border="0" /></a>
<br/>
<br/>
<br/>
Login to Windows again.
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="16" border="0" /></a>
<br/>
<br/>
<br/>
SETUP our IP Addresses for the DC
Remember we have 2 NICs
I NICs IP address will be automatically set via DHCP
The 2nd NIC will have to assign the IP Address, Default Gateway, 
Go to Settings > Ethernet > Change adapter options
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="17" border="0" /></a>
<br/>
<br/>
Rename the network to the correct NIC.
Right click > Status > Details 
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="18" border="0" /></a>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="19" border="0" /></a>
<br/>
<br/>
This looks like our NIC 1 IP address
Usually start with a 10
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="20" border="0" /></a>
<br/>
<br/>
<br/>
Right click the second adapter. We know this is NIC2 the internal adapter
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="21" border="0" /></a>
<br/>
<br/>
Lets appropriately rename each NIC
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="22" border="0" /></a>
<br/>
<br/>
<br/>
Lets Assign our IP address to NIC2 adapter
right click properties > 
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="23" border="0" /></a>
<br/>
<br/>
<br/>
Double click on IPv4
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="24" border="0" /></a>
<br/>
<br/>
<br/>
We are not getting into IP addressing in this video, 
Default gateway is left blank because this NIC doesn’t use a Default gateway

When you install Active Directory it automatically installs DNS Server
You can either use the same IP address in the Preferred DNS Server or you can add a loop back address 
which is 127.0.0.1
A loopback address is something that refers to yourself.
So if you ping the above loop back address it will ping yourself. Interesting….

<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="25" border="0" /></a>
<br/>
<br/>
Finished Installing the 1st VM
Lets also rename our PC
Settings > About > Rename this PC
Restart Computer
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="26" border="0" /></a>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="27" border="0" /></a>
<br/>






<br>
</br>
<h3>Step 2 - Installing Active Directory Domain Services and Creating a Domain</h3>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="29" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="30" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="31" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="32" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="33" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="34" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="35" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="36" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="37" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="38" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="39" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="40" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="41" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="42" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="43" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="44" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="45" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="46" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="47" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="48" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="49" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="50" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="51" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="52" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="53" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="54" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="55" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="56" border="0" /></a>
<br/>
<br/>





<br/>
<br/>
<h3>Step 3 - Installing Remote Access Service and Network Address Translation</h3>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="57" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>







</br>
<h3>Step 4 - Creating 1000 Random Fake Active Directory Users with Powershell</h3>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="99" border="0" /></a>
<br/>
<br/>






<br>
</br>
<h3>Step 5 - Enabling User Entity Behavior Analytics and Playbooks  </h3>













<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
