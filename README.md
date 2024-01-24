
<h1>Microsoft Active Directory Home Lab</h1>


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
Click > Add roles and features
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="28" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="29" border="0" /></a>
<br/>
<br/>
Select > Role based or feature based installation
Click > Next 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="30" border="0" /></a>
<br/>
<br/>
Select the correct server on which to install the roles and features
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="31" border="0" /></a>
<br/>
<br/>
Select Active Directory Domain Services
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="32" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="33" border="0" /></a>
<br/>
<br/>
Click > Next 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="34" border="0" /></a>
<br/>
<br/>
Select Install
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="35" border="0" /></a>
<br/>
<br/>

After installing we need to do Post deployment Configuration
We have installed the software for active directory but we havent created a domain.


Click > Promote this server to a domain controller

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="36" border="0" /></a>
<br/>
<br/>
Select > Add a new forest

Specifiy domain name. You can name it whatever you want. 
Root domain name: mydomain.com
Click > Next

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="37" border="0" /></a>
<br/>
<br/>
We are not going to use this in the future 
but input 
Password1 as the password for Directory Services Restore Mode password

Click > Next

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="38" border="0" /></a>
<br/>
<br/>
Ignore the warning
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="39" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="40" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="41" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="42" border="0" /></a>
<br/>
<br/>
Click > Install
Computer will restart
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="43" border="0" /></a>
<br/>
<br/>
Login Again
Notice the Domain name in front of the Adminstrator
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="44" border="0" /></a>
<br/>
<br/>
Now we are going to create our own dedicated Domain Admin Account

Start > Adminstrative Tools > Active Directory Users and Computers
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="45" border="0" /></a>
<br/>
<br/>
On left sidebar
right click on mydomain.com > New > Organizational Unit
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="46" border="0" /></a>
<br/>
<br/>
Name the folder _Admins
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="47" border="0" /></a>
<br/>
<br/>
right click _Admins folder > New > User
Add name fields
Robert Lee
user logon name
a-rlee (a signifies admin)
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
Click > Next
Password > [Add password]
Uncheck > User must change password at next logon
Check > Password never expires
Click > Next
Click > Finish
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="50" border="0" /></a>
<br/>
<br/>
In the _Admin folder you can see our new user Robert Lee
but now we have to make him the admin
right click on Rober Lee > Properties 

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="51" border="0" /></a>
<br/>
<br/>
Go to Member Of Tab
Click > Add
Enter the Object names to select > type 'domain admins' > Check Names > Ok > Apply > Ok

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="52" border="0" /></a>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="53" border="0" /></a>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="54" border="0" /></a>
<br/>
<br/>
Sign out to test that it works
Click bottom left > Other user
Enter your username and password credentials
a-rlee + password
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="55" border="0" /></a>
<br/>
<br/>
Okay so now you can log into your domain admin account 
Howeveri am getting a network issue. It is saying I have no internet connection. (see bottom right)
Currently I am on a NAT. But maybe I need to set it to bridged adapter. 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="56" border="0" /></a>
<br/>
<br/>







<br/>
<br/>
<h3>Step 3 - Installing Remote Access Service and Network Address Translation</h3>
<br/>
purpose of this is so that our NIC2 Win10 Client can be on the private virtual network and still be able to access
the internet throught the Domain Controller. It will have a private IP address that can access the public internet.

RAS = remote access service
Basically it allows users to connect to a network or computer from a remote location.
It enables remote access to resources like files and applications
We need the Win10 VM machine to access the internet from its private network.


NAT = Network address Translation. 
It is used in routers.
It translates a set of IP addresses into another set of IP addresses
It helps preserve the limited amount of IPv4 public IP addresses
As there around 4 billion public IP addresses, engineers developed private IP addresses and network address translation

There are two different types of IPv4 addresses.
Public and Private
Public IP addresses are publicly registered on the internet
We need a public IP address to go on the internet. 

Private IP addresses are not publicly registered
So we cant directly access the internet with a private address.
Only used internally.

You will usually have multiple devices on a private network. So chances are you will have to have a public IP 
address to access the internet.

So in NAT our router will assign each device in our private network a private IP address. 
So when one of our device needs to connect to the internet, it will send the private IP address to the router.
The router will then use NAT to translate the private IP address into the public IP address.
That’s it!

So NAT:
translates a set of IP addresses into another set of IP addresses - aka Private to Public
but it also translate Public to private

However we wont need NAT or private IP addresses.
This is due to IPv6
Every device will have its own public IP address
Why? IPv6 can produce 340 undecillion IP addresses --> 340 with 36 digits after it


Now lets start installing RAS and NAT on our Domain Controller
We are already logged in as our Admin User Robert Lee

Server Manager
Click > Roles and Features

<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="57" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="58" border="0" /></a>
<br/>
<br/>
Installation Type > Role Based > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="59" border="0" /></a>
<br/>
<br/>
Server Selection > DomainController.mydomain.com > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="60" border="0" /></a>
<br/>
<br/>
Server Roles > Remote Access > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="61" border="0" /></a>
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="62" border="0" /></a>
<br/>
<br/>
Role Services > Select Routing & DirectAccess and VPN (RAS) > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="63" border="0" /></a>
<br/>
<br/>
Web Server Role > Next 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="64" border="0" /></a>
<br/>
<br/>
Confirmation > Install
Close
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="65" border="0" /></a>
<br/>
<br/>
Now that the role installed we need to configure and enable routing and remote access


Server Manager Dashboard > Tools > Routing and Remote Access
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="66" border="0" /></a>
<br/>
<br/>
Select DC  (local)
Right click > Configure and Enable Routing and Remote Access > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="67" border="0" /></a>
<br/>
<br/>
Select Network Address Translation (NAT) > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="68" border="0" /></a>
<br/>
<br/>
Select our Internet facing Network Interface > Next > Finish
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="69" border="0" /></a>
<br/>
<br/>
Now our Domain Controller is green 
We have configures our Domain Controller
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="70" border="0" /></a>
<br/>
<br/>
Next step is to set up a DHCP Server on our Domain Controller.
Why is this necessary?
It provides automatic IP address assignment
It simplies network administration
It dynamically allocates IP address to devices that join the network 
It reduces config errors
It enhances security


Every computer or device has to have an IP address
There 2 ways that a computer can be assigned an IP address
A static IP or dynamic IP

Static IP = user assigns IP address manually
This was originally done all manually. You couldn’t also assign two same IP addresses or it would cause IP conflict

You also had to add:
Subnet mask
Default gateway
DNS Server 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="71" border="0" /></a>
<br/>
<br/>
DHCP solves this by assigning dynamic IP addresses
A dynamic IP is where a computer gets an IP address from a DHCP server

DHCP server automatically assigns a computer:
IP address
Subnet mask
Default gateway
DNS Server
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="72" border="0" /></a>
<br/>
<br/>
You can confirm all the DHCP server settings that is given to our computer in windows command prompt
type
Ipconfig /all
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="73" border="0" /></a>
<br/>
<br/>
A DHCP Server assigns IP addresses from its scope
Scope is a range of IP addresses that a DHCP server can hand out.
Start IP address to End IP address

Example below the DHCP Server can hand out around 100 IP addresses
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="74" border="0" /></a>
<br/>
<br/>
The DHCP server assigns the IP address as a lease
The computer actually doesn’t own the IP address
A lease is the amount of time an IP address is assigned to a computer
Why leasing?
Its to make sure the DHCP Server doesn’t run out of IP addresses
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="75" border="0" /></a>
<br/>
<br/>
Also what if one of the computers is removed if it is not leased. 
Then that computer will actually take the IP address with it.

Imagine a new computer is added to the network
It cannot access the network as the DHCP Server has run out of IP addresses

Hence the reason DHCP leases IP addresses not gives it.
The DHCP server will know which ones are available to use and which IP addresses are being used

Also the computers and the devices on the network will renew their leases to the DHCP Server
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="76" border="0" /></a>
<br/>
<br/>
If you want a computer or device to have a specific IP address (IP reservation) all the time
you can create an Address Reservation on the DHCP Server
A reservation ensures that a specific computer or device will always be given the same IP address

Reservations are typically given to special devices or computers such as network printers, servers, routers
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="77" border="0" /></a>
<br/>
<br/>
DHCP is a service that runs on a server such as Microsoft server or a Linux Server
Its also a service that runs on routers


Okay so back to the project






So a DCHP server will allow our windows 10 client to get a IP address so that it can connect to the internet

In Server Manager Dashboard
Click Add roles and features

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="78" border="0" /></a>
<br/>
<br/>
Click Next for Before you Begin
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="79" border="0" /></a>
<br/>
<br/>
Installation Type > Role Based or feature based installation > Next 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="80" border="0" /></a>
<br/>
<br/>
Select the correct server > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="81" border="0" /></a>
<br/>
<br/>
Server Roles > select DHCP Server > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="82" border="0" /></a>
<br/>
<br/>
Features > Next
DHCP > Next
Click > Install
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="83" border="0" /></a>
<br/>
<br/>
In Server Manager Dashboard > Tools > DHCP
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="84" border="0" /></a>
<br/>
<br/>
We need to create a scope for the DHCP 
Right click IPv4 > Select New Scope > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="85" border="0" /></a>
<br/>
<br/>
Usually you can name the Scope after the IP range
Click Next
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="86" border="0" /></a>
<br/>
<br/>
Adding the Start and End IP address
And changing the Length to 24

The Subnet Mask is automatically calculated.
I really don’t know how the subnet mask is calculated. I need to watch a video on this. 

You must do research!
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="87" border="0" /></a>
<br/>
<br/>
Okay so in ChatGPT
I asked the following question.

You
I am configuring a DHCP server with a scope of start address of 172.16.0.100 and end address of 172.16.0.200. I changed the length to 24. What is the Subnet mask? How do i calculate the subnet mask. Why is it even important to calculate the subnet mask? What does the length mean?

Here is Chats reply.

In the context of networking and DHCP configuration, the subnet mask is a critical component. Let's break down your questions:

Subnet Mask Calculation:

The subnet mask determines the network portion and host portion of an IP address. In your case, you mentioned a length of 24, which is commonly denoted as "/24" in CIDR (Classless Inter-Domain Routing) notation.

For a /24 subnet:

The first three octets (24 bits) are dedicated to the network.
The last octet (8 bits) is for host addresses.

So, if you're using the IP range 172.16.0.100 to 172.16.0.200, the subnet mask would be:

255.255.255.0 (or written as /24 in CIDR notation)

Importance of Subnet Mask:

Network Identification:

The subnet mask helps devices identify which portion of the IP address is the network and which is the host. This is crucial for routing traffic within and between networks.
Address Assignment:

DHCP uses the subnet mask to determine the range of IP addresses it can allocate to devices on the network.
Broadcast Domain:

Subnet masks define the boundaries of broadcast domains. Devices within the same subnet can directly communicate with each other without the need for routing.
Efficient Address Utilization:

Proper subnetting allows for efficient utilization of IP addresses, avoiding wastage and optimizing the allocation of addresses to different subnetworks.

Understanding "Length" or CIDR Notation:

The "length" you mentioned, such as "/24," represents the number of bits set to 1 in the subnet mask.
In CIDR notation, it specifies how many bits of the IP address belong to the network. For example, a /24 means the first 24 bits are the network, and the remaining 8 bits are for hosts.

In summary, calculating the subnet mask is important for defining the network structure, aiding in efficient IP address allocation, and facilitating proper routing and communication within a network. The "length" or CIDR notation is a shorthand way of expressing the subnet mask and indicating how many bits are allocated for the network.




Moving back to the Project
Add Exclusions and Delay > Next
We don’t really need to add any exclusions

<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="88" border="0" /></a>
<br/>
<br/>
Lease Duration
We know from our previous research on DHCP that lease duration will mean an IP address will be leased to a 
specific device or computer for a certain amount of time before it is renewed again. 
If the lease duration is not renewed then the DHCP server will cancel the lease.
That means the IP address assigned to that device or computer will return to the scope of the DHCP server.

So if you running a small café, you would naturally give devices attached to your Wifi network maybe around a 
2hr lease as an example. 

Because this is a home lab project we will just leave it as 8 days. 

Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="89" border="0" /></a>
<br/>
<br/>
Configure DHCP Options > Yes

This tells the client which servers to use for DNS, which servers to use for Default Gateway.. Etc
this will help the client get on the internet.
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="90" border="0" /></a>
<br/>
<br/>
In our project configuration the DC using a RAS/NAT to guide the client to the Internet.
It uses a NIC2 as a default gateway that receives the clients request before passing it through the DC and to the internet

So the default gateway will the Domain Controllers IP address 172.16.0.1 which has NAT configured on it.
Click > Add
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="91" border="0" /></a>
<br/>
<br/>
Click > Next
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="92" border="0" /></a>
<br/>
<br/>
For Domain Name and DNS Servers
Active Directory automatically installs it in the DC
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="93" border="0" /></a>
<br/>
<br/>
WINS Servers > Next
WINS Servers are old 
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="94" border="0" /></a>
<br/>
<br/>
Click > Next > Finish
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="95" border="0" /></a>
<br/>
<br/>
Right click DHCP Server > Authorize
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="96" border="0" /></a>
<br/>
<br/>
Right click DHCP Server > Refresh
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="97" border="0" /></a>
<br/>
<br/>
Now we have our DHCP Server setup!
Awesome!
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="98" border="0" /></a>
<br/>
<br/>






</br>
</br>
<h3>Step 4 - Creating 1000 Random Fake Active Directory Users with Powershell</h3>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="99" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="100" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="101" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="102" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="103" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="104" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="105" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="106" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="107" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="108" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="109" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="110" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="111" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="112" border="0" /></a>
<br/>
<br/>





<br>
</br>
<h3>Step 5 - Enabling User Entity Behavior Analytics and Playbooks  </h3>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="113" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="114" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="115" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="116" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="117" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="118" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="119" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="120" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="121" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="122" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="123" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="124" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="125" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="126" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="127" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="128" border="0" /></a>
<br/>
<br/>
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="129" border="0" /></a>
<br/>
<br/>












<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
