![image](https://github.com/chun-eric/active-directory-homelab/assets/102393871/f999a414-04b9-4641-96bd-72041bfa6e1b)
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


<h2>Project walk-through:</h2>
<h3>High Level Overview</h3>
The below diagram represents what the goal of our project is.
<br/>
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="1" border="0" /></a>
<br/>
<br/>
We will install two different Virtual Machines. One is a Windows 10 ISO and the other is Server 2019 ISO.
<br/>
<br/>
The first VM is our Domain Controller (Server 2019). This VM has 2 Network Interface Cards (NICs).
<br/>
<br/>
The first NIC is connected to the Internet. Will get addressing from Home Router via DHCP. The second
NIC is internal and will be connected to a local Private Network.
<br/>
<br/>
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
<br/>
<br/>



<h3>Step 1 - Setting up our Environment</h3>
Download Virtual Box, Windows 10 and Windows server 2019. We need to fill out some details in Virtual Box.
<br/>
<br/>
Create the VM. First the Domain Controller (Server 2019)
<br/>
<a href="https://ibb.co/5xB3sjB"><img src="https://i.ibb.co/x8JbzgJ/2.png" alt="2" border="0"></a>
<br/>
<br/>
 I skipped the Unattended Install intentionally for now.
<br/>
<a href="https://ibb.co/MkyVXTm"><img src="https://i.ibb.co/s9pwBSZ/3a.png" alt="3a" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/qdJR5Tq"><img src="https://i.ibb.co/QfD6dyS/3b.png" alt="3b" border="0"></a>
<br/>
<br/>
We select our base memory and the number of processors.
<br/>
<a href="https://ibb.co/j50xgGc"><img src="https://i.ibb.co/MhwjMpY/4.png" alt="4" border="0"></a>
<br/>
<br/>
Hard drive allocation was set to around 40 gigabytes.
<br/>
<a href="https://ibb.co/fNqxb3v"><img src="https://i.ibb.co/NNtyX0S/5.png" alt="5" border="0"></a>
<br/>
<br/>
We need to add two Network Adapters. The first one is NAT.
<br/>
<a href="https://ibb.co/P1W7d7v"><img src="https://i.ibb.co/wpQ9f9F/6.png" alt="6" border="0"></a>
<br/>
<br/>
The second Adapter is Internal Network.
<br/>
<a href="https://ibb.co/w7kxq89"><img src="https://i.ibb.co/nBG5y4H/7.png" alt="7" border="0"></a>
<br/>
<br/>
Now we install the Windows Server VM.
<br/>
<a href="https://ibb.co/wgwGJ33"><img src="https://i.ibb.co/MSGw8WW/8.png" alt="8" border="0"></a>
<br/>
<br/>
Choose Custom Install.

```
Click > Next
```
<a href="https://ibb.co/ZhZthDq"><img src="https://i.ibb.co/qRZXRQT/9.png" alt="9" border="0"></a>
<br/>
<br/>
After the installation has completed, type in your password.
<br/>
<a href="https://ibb.co/QmTbVXp"><img src="https://i.ibb.co/NYJVq7t/10.png" alt="10" border="0"></a>
<br/>
<br/>
Open Windows by holding:

```
Ctrl + Alt + Delete
```
Go to:

```
Input > Insert Ctrl + Alt + Delete
```
If we don't follow the above instruction, we won't be able to unlock the home screen to enter our credentials.
<br/>
<a href="https://ibb.co/ZhS7W6b"><img src="https://i.ibb.co/19Mj8f4/11.png" alt="11" border="0"></a>
<br/>
<br/>
```
Click > Yes
```
We want other PCs to be discoverable on this network.
<br/>
<a href="https://ibb.co/WtrHJRJ"><img src="https://i.ibb.co/Srk6CZC/12.png" alt="12" border="0"></a>
<br/>
<br/>
To get rid of the lag and to allow the VM to resize properly, we will need to add the Guest Box additions Images.
<br/>
<br/>
Go to:

```
This PC > Click on Drive D
```
<a href="https://ibb.co/1v2hq7t"><img src="https://i.ibb.co/mDz1qbV/13.png" alt="13" border="0"></a>
<br/>
<br/>

```
Click > VBoxWindowsAdditions-amd64
```

```
Click > Install
```
<a href="https://ibb.co/gjkVdLJ"><img src="https://i.ibb.co/Tv9L1xt/14.png" alt="14" border="0"></a>
<br/>
<br/>
Reboot later. However Shut the VM down.
<br/>
<a href="https://ibb.co/418TN2t"><img src="https://i.ibb.co/fDkp09F/15.png" alt="15" border="0"></a>
<br/>
<br/>
Login to Windows again.
<br/>
<a href="https://ibb.co/9GwDY1r"><img src="https://i.ibb.co/WsDhKM2/16.png" alt="16" border="0"></a>
<br/>
<br/>
Now setup our IP Addresses for the Domain Controller. Remember we have 2 NICs.
The first NICs IP address will be automatically set via DHCP.
The 2nd NIC will have to assign the IP Address, Default Gateway.
<br/>
<br/>

```
Go to Settings > Ethernet > Change adapter options.
```
<br/>
<a href="https://ibb.co/GdsF0yn"><img src="https://i.ibb.co/CwJWzgm/17.png" alt="17" border="0"></a>
<br/>
<br/>
Rename the network to the correct NIC.
<br/>
<br/>

```
Right Click > Status > Details
```
<a href="https://ibb.co/TvdcZQH"><img src="https://i.ibb.co/C1DBqjn/18.png" alt="18" border="0"></a>
<a href="https://ibb.co/m06ZGH5"><img src="https://i.ibb.co/zFG9rSV/19.png" alt="19" border="0"></a>
<br/>
<br/>
This looks like our 1st NIC's IP address. 
<br/>
It will usually start with a number 10.
<br/>
<a href="https://ibb.co/m08hyNq"><img src="https://i.ibb.co/cwL28YF/20.png" alt="20" border="0"></a>
<br/>
<br/>
Right click the second adapter. We know this is the second NIC as it has a IPv4 Autoconfiguration.
<br/>
<a href="https://ibb.co/R9MgSWd"><img src="https://i.ibb.co/zrLm5wD/21.png" alt="21" border="0"></a>
<br/>
<br/>
Lets rename each NIC to make things easier for us.
<br/>
<a href="https://ibb.co/kxT2HxC"><img src="https://i.ibb.co/nbyMgbJ/22.png" alt="22" border="0"></a>
<br/>
<br/>
Lets Assign our IP address to NIC2 adapter:
<br/>

```
Right click > Properties
```  
<a href="https://ibb.co/L0Ns7Pm"><img src="https://i.ibb.co/51KJH9m/23.png" alt="23" border="0"></a>
<br/>
<br/>

```
Double click on IPv4.
```
<a href="https://ibb.co/4SkQwPH"><img src="https://i.ibb.co/t2Rdw89/24.png" alt="24" border="0"></a>
<br/>
<br/>
The default gateway is left blank because this NIC doesn’t use a Default gateway.
<br/>
<br/>
When you install Active Directory, it automatically installs a DNS Server.
<br/>
<br/>
You can either use the same IP address in the Preferred DNS Server or you can add a loop back address which is 127.0.0.1.
<br/>
<br/>
A loopback address is something that refers to yourself.
So if you ping the above loop back address it will ping yourself. 
<br/>
<a href="https://ibb.co/0h5VtdS"><img src="https://i.ibb.co/pJqry98/25.png" alt="25" border="0"></a>
<br/>
<br/>
Finished installing the first virtual machine.
Lets also rename our PC:

```
Settings > About > Rename this PC
```
Restart the computer.
<br/>
<a href="https://ibb.co/xX627hy"><img src="https://i.ibb.co/2WdFkqw/26.png" alt="26" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/PTwKMF4"><img src="https://i.ibb.co/dbGFtj6/27.png" alt="27" border="0"></a>
<br/>






<br>
</br>
<h3>Step 2 - Installing Active Directory Domain Services and Creating a Domain</h3>
<br/>

```
Click > Add roles and features
```
<a href="https://ibb.co/qgKdVM8"><img src="https://i.ibb.co/PWKN0Zy/28.png" alt="28" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/n0Y6nx7"><img src="https://i.ibb.co/yRZdYH6/29.png" alt="29" border="0"></a>
<br/>

```
Select > Role based or feature based installation
```

```
Click > Next 
```
<a href="https://ibb.co/S3SLdG4"><img src="https://i.ibb.co/DRdq8TH/30.png" alt="30" border="0"></a>
<br/>
<br/>
Select the correct server on which to install the roles and features.
<br/>
<a href="https://ibb.co/rG2gXb6"><img src="https://i.ibb.co/GvQXrs2/31.png" alt="31" border="0"></a>
<br/>
<br/>
Select Active Directory Domain Services
<br/>
<a href="https://ibb.co/5LyJ968"><img src="https://i.ibb.co/YhMVW3k/32.png" alt="32" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/q15Wq7w"><img src="https://i.ibb.co/kc19b5F/33.png" alt="33" border="0"></a>
<br/>
<br/>

```
Click > Next 
```
<a href="https://ibb.co/2YqnpNf"><img src="https://i.ibb.co/1JZv46w/34.png" alt="34" border="0"></a>
<br/>
<br/>

```
Select > Install
```
<a href="https://ibb.co/PQdykM7"><img src="https://i.ibb.co/GTqDm9K/35.png" alt="35" border="0"></a>
<br/>
<br/>
After installing, we need to do a Post deployment Configuration.
We have installed the software for active directory but we havent created a domain.

```
Click > Promote this server to a domain controller
```
<a href="https://ibb.co/bvxrX2x"><img src="https://i.ibb.co/PWR5g9R/36.png" alt="36" border="0"></a>
<br/>
<br/>

```
Select > Add a new forest
```

Specifiy a domain name. We can name it whatever we want. 
For instance --> Root domain name: mydomain.com.

```
Click > Next
```
<a href="https://ibb.co/MNLKvrs"><img src="https://i.ibb.co/tC5nGg4/37.png" alt="37" border="0"></a>
<br/>
<br/>
We are not going to use this in the future but input. 
Password1 as the password for Directory Services Restore Mode password.

```
Click > Next
```
<a href="https://ibb.co/WsXbRP0"><img src="https://i.ibb.co/GFDKrt9/38.png" alt="38" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/tDv8zSN"><img src="https://i.ibb.co/89f6bJR/39.png" alt="39" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/PZXJTHb"><img src="https://i.ibb.co/jv0qVXx/40.png" alt="40" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/4Yfptzx"><img src="https://i.ibb.co/dLcDMFy/41.png" alt="41" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/pXpXz0G"><img src="https://i.ibb.co/ryJy67T/42.png" alt="42" border="0"></a>
<br/>
<br/>

```
Click > Install
```

The computer will restart.
<br/>
<a href="https://ibb.co/vY0wtLj"><img src="https://i.ibb.co/16FvSRm/43.png" alt="43" border="0"></a>
<br/>
<br/>
Let's login again.
Notice the Domain name in front of the Adminstrator.
<br/>
<a href="https://ibb.co/grLcLpm"><img src="https://i.ibb.co/x2trt0X/44.png" alt="44" border="0"></a>
<br/>
<br/>
Now we are going to create our own dedicated Domain Admin Account.

```
Start > Adminstrative Tools > Active Directory Users and Computers
```
<br/>
<a href="https://ibb.co/ZHX3nkM"><img src="https://i.ibb.co/TK2ZVFg/45.png" alt="45" border="0"></a>
<br/>
<br/>
On left sidebar:

```
Right click on mydomain.com > New > Organizational Unit
```
<a href="https://ibb.co/CKwgw54"><img src="https://i.ibb.co/JjcVcR8/46.png" alt="46" border="0"></a>
<br/>
<br/>
Name the folder _Admins.
<br/>
<a href="https://ibb.co/VxJ14CS"><img src="https://i.ibb.co/ypd9jky/47.png" alt="47" border="0"></a>
<br/>
<br/>

```
Right click _Admins folder > New > User
```

Add the following name fields:

```
Robert Lee
```

User login name:

```
a-rlee (a signifies admin)
```
<a href="https://ibb.co/rMB4djH"><img src="https://i.ibb.co/MpzMBv7/48.png" alt="48" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/FBB9hDP"><img src="https://i.ibb.co/gDDGrRn/49.png" alt="49" border="0"></a>
<br/>
<br/>
```
Click > Next
```
```
Password > [Add password]
```
```
Uncheck > User (must change password at next login)
```
```
Check > Password never expires
```
```
Click > Next
```
```
Click > Finish
```
<a href="https://ibb.co/rkTsyJL"><img src="https://i.ibb.co/CBN20FX/50.png" alt="50" border="0"></a>
<br/>
<br/>
In the _Admin folder you can see our new user Robert Lee but we have to make him the admin.

```
Right click on Robert Lee > Properties 
```
<a href="https://ibb.co/nPyVdj1"><img src="https://i.ibb.co/WVXd8fz/51.png" alt="51" border="0"></a>
<br/>
<br/>
Go to Member Of Tab.

```
Click > Add
```

```
Select + enter the Object name > type 'domain admins' > Check Names > Ok > Apply > Ok
```
<a href="https://ibb.co/qJpL6Tt"><img src="https://i.ibb.co/rdHK9DC/52.png" alt="52" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/Sy96nGh"><img src="https://i.ibb.co/mTMRqgL/53.png" alt="53" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/QYnyNKm"><img src="https://i.ibb.co/Dwk0CRK/54.png" alt="54" border="0"></a>
<br/>
<br/>
Sign out to test that it works.

```
Click bottom left > Other user
```
Finally enter your username and password credentials.
<a href="https://ibb.co/tQxYhTc"><img src="https://i.ibb.co/Fm08WLJ/55.png" alt="55" border="0"></a>
<br/>
<br/>
Okay, now we can log into our domain admin account.
However we were getting a network issue. It was saying that I had no internet connection. (see bottom right)
On virtual box, I was intially on a NAT adapter but changed it to a bridged adapter. That solved our 
connectivity issue. I may have to look into this further.
<br/>
<a href="https://ibb.co/pwLD7xC"><img src="https://i.ibb.co/XLCBQbm/56.png" alt="56" border="0"></a>
<br/>
<br/>







<br/>
<br/>
<h3>Step 3 - Installing Remote Access Service and Network Address Translation</h3>
The purpose of this step is so that our NIC2 Win10 Client can be on the private virtual network and still be able to access
the internet throught the Domain Controller. It will have a private IP address that can access the public internet.

Let's understand some key terminology first.

RAS = remote access service
Basically it allows users to connect to a network or computer from a remote location.
It enables remote access to resources like files and applications.
We need the Win10 VM machine to access the internet from its private network.


NAT = Network address Translation. 
NAT is used in routers.
It translates a set of IP addresses into another set of IP addresses.
It helps preserve the limited amount of IPv4 public IP addresses.
As there are around 4 billion public IP addresses, engineers developed private IP addresses and network address translation.

There are two different types of IPv4 addresses.
Public and Private IP addresses.
Public IP addresses are publicly registered on the internet.
We need a public IP address to go on the internet. 

Private IP addresses are not publicly registered.
So we can't directly access the internet with a private address.
We use private IP addresses for internal networks.

Usually, there are multiple devices on a private network. 
So chances are, you will have to have a public IP address to access the internet.

So in NAT our router will assign each device in our private network a private IP address. 
So when one of our device needs to connect to the internet, it will send the private IP address to the router.
The router will then use NAT to translate the private IP address into the public IP address.
That’s it in a nutshell!

So to recap NAT translates a set of IP addresses into another set of IP addresses - 
aka Private to Public but it can also translate public to private.

However we wont need NAT or private IP addresses.

This is due to IPv6.
Every device will have its own public IP address.
Why? IPv6 can produce 340 undecillion IP addresses. That's 340 with 36 digits after it

Now lets start installing RAS and NAT on our Domain Controller
We are already logged in as our Admin User Robert Lee

In the Server Manager:

```
Click > Roles and Features
```
<a href="https://ibb.co/cbmNSRq"><img src="https://i.ibb.co/wJvKF8t/57.png" alt="57" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/Yp69Fy6"><img src="https://i.ibb.co/LnMwXJM/58.png" alt="58" border="0"></a>
<br/>
<br/>

```
Installation Type > Role Based > Next
```
<a href="https://ibb.co/gm66K0Q"><img src="https://i.ibb.co/sbttYTc/59.png" alt="59" border="0"></a>
<br/>
<br/>

```
Server Selection > DomainController.mydomain.com > Next
```
<a href="https://ibb.co/GxFsnMD"><img src="https://i.ibb.co/HFXNBPy/60.png" alt="60" border="0"></a>
<br/>
<br/>

```
Server Roles > Remote Access > Next
```
<a href="https://ibb.co/1vDDV52"><img src="https://i.ibb.co/Wxbb9wK/61.png" alt="61" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/FH2Xvfz"><img src="https://i.ibb.co/KKdDZQW/62.png" alt="62" border="0"></a>
<br/>
<br/>

```
Role Services > Select Routing & DirectAccess and VPN (RAS) > Next
```
<a href="https://ibb.co/wBgNNgG"><img src="https://i.ibb.co/xXLSSL9/63.png" alt="63" border="0"></a>
<br/>
<br/>

```
Web Server Role > Next
```
<a href="https://ibb.co/tZQf1vw"><img src="https://i.ibb.co/8zN3Qfn/64.png" alt="64" border="0"></a>
<br/>
<br/>

```
Confirmation > Install > Close
```
<a href="https://ibb.co/pK5Mj6j"><img src="https://i.ibb.co/PW0PGsG/65.png" alt="65" border="0"></a>
<br/>
<br/>
Now that the role is installed, we need to configure and enable routing and remote access.

```
Server Manager Dashboard > Tools > Routing and Remote Access
```
<a href="https://ibb.co/xGWg86S"><img src="https://i.ibb.co/L6mgh1z/66.png" alt="66" border="0"></a>
<br/>
<br/>
Select DC  (local).

```
Right click > Configure and Enable Routing and Remote Access > Next
```
<a href="https://ibb.co/ZXY7wss"><img src="https://i.ibb.co/DW8T6XX/67.png" alt="67" border="0"></a>
<br/>
<br/>

```
Select Network Address Translation (NAT) > Next
```
<a href="https://ibb.co/2sZh8cd"><img src="https://i.ibb.co/7RkCgrK/68.png" alt="68" border="0"></a>
<br/>
<br/>

```
Select our Internet facing Network Interface > Next > Finish
```
<a href="https://ibb.co/fD8cLgn"><img src="https://i.ibb.co/FHsPCr0/69.png" alt="69" border="0"></a>
<br/>
<br/>
Now our Domain Controller is green and that means we have configured our Domain Controller.
<br/>
<a href="https://ibb.co/BZ6YDM0"><img src="https://i.ibb.co/zR2tcvq/70.png" alt="70" border="0"></a>
<br/>
<br/>
Next step is to set up a DHCP Server on our Domain Controller.
Why is this necessary?
It provides automatic IP address assignment.
It simplies network administration.
It dynamically allocates IP addresses to devices that join the network. 
It reduces configuration errors.
It enhances security.


Every computer or device has to have an IP address.
There two ways that a computer can be assigned an IP address.
A static IP or dynamic IP.

Static IP = user assigns IP address manually.
This was originally done manually. You also couldn’t assign two same IP addresses or it would cause IP conflict.

We have to add a:
<br/>
- Subnet mask
- Default gateway
- DNS Server 
<br/>
<br/>
<a href="https://ibb.co/9VgJV0H"><img src="https://i.ibb.co/k9Bp9T2/71.png" alt="71" border="0"></a>
<br/>
<br/>
A DHCP solves this by assigning dynamic IP addresses.
A dynamic IP is where a computer gets an IP address from a DHCP server.
<br/>
DHCP server automatically assigns a computer:
<br/>
- IP address
- Subnet mask
- Default gateway
- DNS Server
<br/>
<br/>
<a href="https://ibb.co/CHYcBSB"><img src="https://i.ibb.co/Vqz1VkV/72.png" alt="72" border="0"></a>
<br/>
<br/>
You can confirm all the DHCP server settings that is given to our computer in windows command prompt.
Type:

```
Ipconfig /all
```
<br/>
<a href="https://ibb.co/qx0Z6pX"><img src="https://i.ibb.co/N714RKQ/73.png" alt="73" border="0"></a>
<br/>
<br/>
A DHCP Server assigns IP addresses from its scope.
A scope is a range of IP addresses that a DHCP server can hand outStart -- a Start IP address to End IP address.

Example below the DHCP Server can hand out around 100 IP addresses
<br/>
<br/>
<a href="https://ibb.co/CmGmKww"><img src="https://i.ibb.co/q7Q7YDD/74.png" alt="74" border="0"></a>
<br/>
<br/>
The DHCP server assigns the IP address as a lease.
The computer actually doesn’t own the IP address.
A lease is the amount of time an IP address is assigned to a computer.

Why leasing?

It is to make sure the DHCP Server doesn’t run out of IP addresses.
<br/>
<br/>
<a href="https://ibb.co/KL701Sh"><img src="https://i.ibb.co/s6QmBzR/75.png" alt="75" border="0"></a>
<br/>
<br/>
Also what if one of the computers is removed, if it is not leased. 
Then that computer will actually take the IP address with it.

Imagine a new computer is added to the network.
It cannot access the network as the DHCP Server has run out of IP addresses.

Hence the reason DHCP leases IP addresses.
The DHCP server will know which ones are available to use and which IP addresses are being used.

Also the computers and the devices on the network will renew their leases to the DHCP Server.
<br/>
<br/>
<a href="https://ibb.co/Dp4qKf5"><img src="https://i.ibb.co/py2mXjJ/76.png" alt="76" border="0"></a>
<br/>
<br/>
If you want a computer or device to have a specific IP address (IP reservation) all the time,
you can create an Address Reservation on the DHCP Server.

A reservation ensures that a specific computer or device will always be given the same IP address.

Reservations are typically given to special devices or computers such as network printers, servers, routers.
<br/>
<br/>
<a href="https://ibb.co/NsyjRKH"><img src="https://i.ibb.co/f2xpJHP/77.png" alt="77" border="0"></a>
<br/>
<br/>
DHCP is a service that runs on a server such as Microsoft server or a Linux Server.
Its also a service that runs on routers.


Okay, so back to the project.


So, a DCHP server will allow our windows 10 client to get a IP address so that it can connect to the internet.

In Server Manager Dashboard:

```
Click > Add roles and features
```
<a href="https://ibb.co/T4Vrx6Y"><img src="https://i.ibb.co/jycHPmb/78.png" alt="78" border="0"></a>
<br/>
<br/>

```
Click > Next 
```
<a href="https://ibb.co/0JX99mC"><img src="https://i.ibb.co/QmcHHpP/79.png" alt="79" border="0"></a>
<br/>
<br/>

```
Installation Type > Role Based or feature based installation > Next
```
<a href="https://ibb.co/YBfKwCZ"><img src="https://i.ibb.co/Ld9qHF5/80.png" alt="80" border="0"></a>
<br/>
<br/>

```
Select the correct server > Next
```
<a href="https://ibb.co/QFBdvQ7"><img src="https://i.ibb.co/pvDyjw6/81.png" alt="81" border="0"></a>
<br/>
<br/>

```
Server Roles > select DHCP Server > Next
```
<a href="https://ibb.co/s693x8Q"><img src="https://i.ibb.co/TbMhj9R/82.png" alt="82" border="0"></a>
<br/>
<br/>

```
Features > Next
```

```
DHCP > Next
```

```
Click > Install
```
<a href="https://ibb.co/gjYHQxG"><img src="https://i.ibb.co/vw9gfSb/83.png" alt="83" border="0"></a>
<br/>
<br/>

```
In Server Manager Dashboard > Tools > DHCP
```
<a href="https://ibb.co/GTxG7zh"><img src="https://i.ibb.co/16vBZFS/84.png" alt="84" border="0"></a>
<br/>
<br/>
We need to create a scope for the DHCP.

```
Right click IPv4 > Select New Scope > Next
```
<a href="https://ibb.co/gTS4yhX"><img src="https://i.ibb.co/G2WH7N1/85.png" alt="85" border="0"></a>
<br/>
<br/>
Usually, you can name the Scope after the IP range.

```
Click > Next
```
<a href="https://ibb.co/hsVCVX5"><img src="https://i.ibb.co/NxK7K1h/86.png" alt="86" border="0"></a>
<br/>
<br/>
Adding the Start and End IP address.
And changing the Length to 24. 

The Subnet Mask is automatically calculated.

The subnet mask determines the network portion and host portion of an IP address. 
In our case, for a /24 subnet:

The first three octets (24 bits) are dedicated to the network.
The last octet (8 bits) is for host addresses.

So, if we are using the IP range 172.16.0.100 to 172.16.0.200, the subnet mask would be:

255.255.255.0 (or written as /24 in CIDR notation)
<br/>
<br/>
<a href="https://ibb.co/ssN8cdj"><img src="https://i.ibb.co/wpP58Fy/87.png" alt="87" border="0"></a>
<br/>
<br/>

Back to the Project.

```
Add Exclusions and Delay > Next
```

We don’t really need to add any exclusions.
<br/>
<a href="https://ibb.co/j31M5fF"><img src="https://i.ibb.co/NWHj3Cz/88.png" alt="88" border="0"></a>
<br/>
<br/>
Lease Duration.

We know from our previous research on DHCP that lease duration will mean an IP address will be leased to a 
specific device or computer for a certain amount of time before it is renewed again. 

If the lease duration is not renewed then the DHCP server will cancel the lease.
That means the IP address assigned to that device or computer will return to the scope of the DHCP server.

As an example, if you are running a small café, you would give devices attached to your Wifi network maybe around a 
2hr lease.

Because this is a home lab project we will just leave it as 8 days. 

```
Click > Next
```
<a href="https://ibb.co/5kwpdZb"><img src="https://i.ibb.co/423vckh/89.png" alt="89" border="0"></a>
<br/>
<br/>

```
Configure DHCP Options > Yes
```

This tells the client which servers to use for DNS, which servers to use for Default Gateway and so on.
This will help the client get on the internet.
<br/>
<br/>
<a href="https://ibb.co/9cqm4bJ"><img src="https://i.ibb.co/tQbw2qS/90.png" alt="90" border="0"></a>
<br/>
<br/>
In our project configuration the DC using a RAS/NAT to guide the client to the Internet.
It uses a NIC2 as a default gateway that receives the clients request before passing it through the DC and to the internet.

So the default gateway will the Domain Controllers IP address 172.16.0.1 which has NAT configured on it.

```
Click > Add
```
<a href="https://ibb.co/q7zDPXj"><img src="https://i.ibb.co/QpZJ1zQ/91.png" alt="91" border="0"></a>
<br/>
<br/>

```
Click > Next
```
<a href="https://ibb.co/vP2prYv"><img src="https://i.ibb.co/Z8y5wm1/92.png" alt="92" border="0"></a>
<br/>
<br/>
For Domain Name and DNS Servers.
Active Directory automatically installs it in the DC.
<br/>
<br/>
<a href="https://ibb.co/kDHzCz5"><img src="https://i.ibb.co/LgkB7BY/93.png" alt="93" border="0"></a>
<br/>
<br/>

```
WINS Servers > Next
```

WINS Servers are pretty old.
<br/>
<br/>
<a href="https://ibb.co/3SbG8x7"><img src="https://i.ibb.co/SVgW1kP/94.png" alt="94" border="0"></a>
<br/>
<br/>

```
Click > Next > Finish
```
<a href="https://ibb.co/qCQ63sV"><img src="https://i.ibb.co/KDvnpqS/95.png" alt="95" border="0"></a>
<br/>
<br/>

```
Right click DHCP Server > Authorize
```
<a href="https://ibb.co/S7LYjyX"><img src="https://i.ibb.co/1JSygQf/96.png" alt="96" border="0"></a>
<br/>
<br/>

```
Right click DHCP Server > Refresh
```
<a href="https://ibb.co/mv2RymT"><img src="https://i.ibb.co/9w1p4Jg/97.png" alt="97" border="0"></a>
<br/>
<br/>
Now we have our DHCP Server setup! 
Fantastic!
<br/>
<br/>
<a href="https://ibb.co/PrJFzs7"><img src="https://i.ibb.co/S6H57Dh/98.png" alt="98" border="0"></a>
<br/>
<br/>






</br>
</br>
<h3>Step 4 - Creating 1000 Random Fake Active Directory Users with Powershell</h3>

Before we create our Client VM, we will create a whole bunch of fake random users with a Powershell Script
We now need to access the internet for the Server
As it is a home lab environment we can configure the server to access the internet but usually we wouldn’t do 
that in a production environment


In Server Manager Dashboard: 

```
Click > Configure this local server
```
<a href="https://ibb.co/71Ys9Vp"><img src="https://i.ibb.co/HHF5JYP/99.png" alt="99" border="0"></a>
<br/>
<br/>
<a href="https://ibb.co/VTVHNKL"><img src="https://i.ibb.co/1TZsnS0/100.png" alt="100" border="0"></a>
<br/>
<br/>

```
IE Enhanced Security Configuration > Off
```
<a href="https://ibb.co/c3Jn8PN"><img src="https://i.ibb.co/bm19vxr/101.png" alt="101" border="0"></a>
<br/>
<br/>
We won't be writing this script from scratch but downloading it off the Internet.
However we will be explaining each line of code to fully understand it.

Download the zipped script file.
Save it on desktop and extract it on the desktop.
<br/>
<a href="https://ibb.co/Zc82BS7"><img src="https://i.ibb.co/KVqK2DS/102.png" alt="102" border="0"></a>
<br/>
<br/>
Inside the folder we see the scripts and the names.txt file. 
The names.txt file has around 1000 generated names but in text file format.
<br/>
<br/>
<a href="https://ibb.co/7rBctZm"><img src="https://i.ibb.co/RQtnTsw/103.png" alt="103" border="0"></a>
<br/>
<br/>
Next, open PowerShell ISE (integrated Scripting Environment) and run as adminstrator.
<br/>
<br/>
<a href="https://ibb.co/88ptsN9"><img src="https://i.ibb.co/LpWHRY1/104.png" alt="104" border="0"></a>
<br/>
<br/>

```
Open the PowerShell Script > 1_CREATE_USERS
```
<a href="https://ibb.co/mB7ZGDt"><img src="https://i.ibb.co/QcGSfbn/105.png" alt="105" border="0"></a>
<br/>
<br/>
Now our script has opened.
<br/>
<br/>
<a href="https://ibb.co/ZG71m8x"><img src="https://i.ibb.co/8Nt4c59/106.png" alt="106" border="0"></a>
<br/>
<br/>
Before we run anything, we have to enable the execution of all scripts.
If we try to run the script now, we will get an error.
<br/>
<br/>
<a href="https://ibb.co/Kh9FQKK"><img src="https://i.ibb.co/XLzSdYY/107.png" alt="107" border="0"></a>
<br/>
<br/>
We need to change the execution policy for PowerShell scripts on a system.

It’s a security feature in PowerShell that determines whether scripts are allowed to run and what kind of scripts are allowed.

```
Type > Set-ExecutionPolicy Unrestricted
```

We need to change the user preference for PowerShell script execution policy
and allow PowerShell scripts to run without restrictions.

```
Click > Yes to All
```
<a href="https://ibb.co/gjkPmKD"><img src="https://i.ibb.co/1vFd7W9/108.png" alt="108" border="0"></a>
<br/>
<br/>
Let's explain the code line by line.

Setting Variables

```
$PASSWORD_FOR_USERS   = "Password1"
```
This line initializes a variable named $PASSWORD_FOR_USERS and assigns the string "Password1" to it. 
This will be the default password for all users created later in the script.

```
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
```
This line reads the content of a file named names.txt and stores it in the variable $USER_FIRST_LAST_LIST. 
Each line in the file is expected to contain a first name and a last name separated by a space.

```
$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
```
This line converts the plain text password stored in $PASSWORD_FOR_USERS into a secure string and assigns it to the variable $password. 
This is done to securely handle passwords in PowerShell.

```
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false
```
This line creates a new Active Directory Organizational Unit (OU) named "_USERS" with the parameter -ProtectedFromAccidentalDeletion set to $false. 
This Organizational Unit will be the container for the new user accounts.

```
foreach ($n in $USER_FIRST_LAST_LIST) {
```
This line starts a foreach loop that iterates through each line in the $USER_FIRST_LAST_LIST variable, where each line is expected to contain a first name and a last name separated by a space. That is it will iterate through each user in the list

    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
These lines split the current line from the loop into first and last names using a space as the delimiter. The resulting names are converted to lowercase and stored in the variables $first and $last.

    $username = "$($first.Substring(0,1))$($last)".ToLower()
This line creates a username by taking the first letter of the first name and appending it to the last name. The resulting username is converted to lowercase and stored in the variable $username.

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
This line prints a message to the console indicating the username that is currently being created. The background color is set to black, and the foreground color is set to cyan for visual formatting

    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
This line creates a new Active Directory user using the New-AdUser cmdlet. It sets various properties such as the account password, given name, surname, display name, name, employee ID, and other attributes. The user account is placed in the "_USERS" OU, and the account is enabled (-Enabled $true).


Now that we understand general gist of the code. 
We need to cd into the directory of the names.txt file.
<a href="https://ibb.co/sVgZz4K"><img src="https://i.ibb.co/3CTnQbp/109a.png" alt="109a" border="0"></a>
<br/>
<br/>
Check with the ls command.
<br/>
<a href="https://ibb.co/5GfwbWd"><img src="https://i.ibb.co/xXRdxSy/109b.png" alt="109b" border="0"></a>
<br/>
<br/>
All we need to do is run the script. 
You can see it running the script and creating the random users.
Very cool.
<br/>
<br/>
<a href="https://ibb.co/fkRtGX3"><img src="https://i.ibb.co/8r324my/110.png" alt="110" border="0"></a>
<br/>
<br/>
If we check our Active Directory and our Domain, we can see that our _USERS folders have been created with 1000 users inside.
<br/>
<br/>
<a href="https://ibb.co/VY3mFcM"><img src="https://i.ibb.co/N27S0dW/111.png" alt="111" border="0"></a>
<br/>
<br/>
If we search for a "Robert Lee" we can see that our user has been created.
<br/>
<br/>
<a href="https://ibb.co/LnsVkDk"><img src="https://i.ibb.co/hDT37x7/112.png" alt="112" border="0"></a>
<br/>
<br/>







<br>
</br>
<h3>Step 5 - Enabling User Entity Behavior Analytics and Playbooks  </h3>
Go through the same process of creating a VM but for Windows 10 Pro ISO.
<br/>
<a href="https://ibb.co/jGS27Xg"><img src="https://i.ibb.co/J294XJ5/113.png" alt="113" border="0"></a>
<br/>
<br/>

```
We will set our Network > Internal Network
```
<a href="https://ibb.co/tq7m1mM"><img src="https://i.ibb.co/zXyntnh/114.png" alt="114" border="0"></a>
<br/>
<br/>

```
Choose Windows 10 Pro
```

```
Custom Install
```
<a href="https://ibb.co/tChX0dv"><img src="https://i.ibb.co/VBNmFKz/115.png" alt="115" border="0"></a>
<br/>
<br/>

```
Input user to this PC
```
<a href="https://ibb.co/DYG5FXj"><img src="https://i.ibb.co/nB3zy2K/116.png" alt="116" border="0"></a>
<br/>
<br/>
No password.
<br/>
<br/>
<a href="https://ibb.co/NsGz4Fx"><img src="https://i.ibb.co/7X0F9Sv/117.png" alt="117" border="0"></a>
<br/>
<br/>
Now we are logged into our Window 10 Client.
<br/>
<br/>
<a href="https://ibb.co/HXg1NZM"><img src="https://i.ibb.co/hC8hX0n/118.png" alt="118" border="0"></a>
<br/>
<br/>
Now to see if we are connected to the Internet.
Go to cmd:

```
type > ipconfig
```

If you can see default gateway address the same as the DC IP address of 172.16.0.1, then we are all connected!
<a href="https://ibb.co/PTwXrwJ"><img src="https://i.ibb.co/z8G9mGC/119.png" alt="119" border="0"></a>
<br/>
<br/>
Lets ping our domain mydomain.com.

Everything seems to be working! Woohoo!
<br/>
<br/>
<a href="https://ibb.co/rkyTNtn"><img src="https://i.ibb.co/NxYQb3w/120.png" alt="120" border="0"></a>
<br/>
<br/>
Lets also change the name of the computer.

```
System > Rename this PC (advanced).
```
<a href="https://ibb.co/SrQqCkz"><img src="https://i.ibb.co/dgbnYNR/121.png" alt="121" border="0"></a>
<br/>
<br/>

```
Click > Change
```

```
Change Computer Name > CLIENT1
```

Member of Domain: 
<br/>
<br/>
<a href="https://ibb.co/n0vVNX6"><img src="https://i.ibb.co/hMjnJGL/122.png" alt="122" border="0"></a>
<br/>
<br/>
Our admin username and password robert lee.
<br/>
<br/>
<a href="https://ibb.co/5rPK8w0"><img src="https://i.ibb.co/0CxsMv7/123.png" alt="123" border="0"></a>
<br/>
<br/>
If you go back to our:

```
Server Manager Dashboard > DHCP
```
We can see under:

```
IPv4 > Scope > Address Leases> CLIENT1.mydomain
```

There is our CLIENT1 machine being leased.
<br/>
<a href="https://ibb.co/hL0S3pM"><img src="https://i.ibb.co/Bck908P/124.png" alt="124" border="0"></a>
<br/>
<br/>
Our new computer is also showing in our Active Directory Users and Computers within the DC.
<br/>
<br/>
<a href="https://ibb.co/chgnXKP"><img src="https://i.ibb.co/23n4Y0w/125.png" alt="125" border="0"></a>
<br/>
<br/>
Lets logout and login as one of our other users in our CLIENT1 machine.

Click Other user.
<br/>
<a href="https://ibb.co/v40YtmP"><img src="https://i.ibb.co/d48gh62/126.png" alt="126" border="0"></a>
<br/>
<br/>
Lets sign in as one of the random users we created.

```
Username > acoke
```

```
Password > [enter password]
```
<a href="https://ibb.co/tbpnyr3"><img src="https://i.ibb.co/LCdB47v/127.png" alt="127" border="0"></a>
<br/>
<br/>
We are logged into as the new user!
<br/>
<a href="https://ibb.co/2q10pn3"><img src="https://i.ibb.co/rkRKX72/128.png" alt="128" border="0"></a>
<br/>
<br/>
We can also connect to the internet.
<br/>
<a href="https://ibb.co/G7S2H04"><img src="https://i.ibb.co/ZdD82Vp/129.png" alt="129" border="0"></a>
<br/>
<br/>




<br>
</br>
<h3>Step 6 - Project Conclusion and Summary</h3>
This Active Directory Home Lab project would replicate a mini corporate network for a small to medium sized business.

<br/>
<br/>
Imagine a new staff being hired to a company. We can create a new user via a script, assign a new account through a Domain Controller.

<br/>
<br/>
The new hire would login with their corporate credentials onto a laptop in the domain we created. 
All internet traffic would then be directed through a domain controller. 
<br/>
<br/>
This concludes the project.
<br/>
<br/>
Thank you for reading the project until the end.
<br/>
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
