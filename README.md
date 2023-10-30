<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Creating Resources in Microsoft Azure
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Creating Resources in Microsoft Azure</h2>

- Our first step is log into Microsoft Azure and create a <b>Windows 10 virtual machine</b>
  -  First create a <b>resource group</b> to house the vm within.  This can be done by either first clicking the <b>resource group icon</b> found in Azure's main menu or by opting to use the <b>Create a virtual machine's</b> menu and clicking the <b>Create new</b> option beneath the second dropdown menu.  The example in the image below uses the latter method.
  -  Use the below information as a guide for your vm settings.  Key factors:
  -  -  Name: vm1
  -  -  Security type: Standard
  -  -  Image: Windows 10 (22H2)
  -  -  Username: vmuser (or whatever you choose)
  -  -  Password: Labuser12345 (or whatever you choose)
  -  -  Virtual network: Default (allow the system automatically create the vnet for you and be sure to use it in the next vm we create.  Odds are it will be named <b>vm1-vnet</b>)
- Once the <b>Windows vm</b> settings have been established, select <b>Review + create</b> at the bottom, await validation, and continue finalizing it's creation

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/6dea9e80-b49c-4f48-bd43-ba87de94fff1)

<br />


- Next we will create a <b>Linux (Ubuntu) vm</b>
  -  Be sure to use place this vm within the same <b>Resource group</b> as vm1
  -  Key settings such as <b>security type</b>, <b>username</b>, <b>password</b>, and <b>virtual network</b> will match that of vm1
  -  Select <b>Ubuntu Server 20.04 LTS - x64 Gen2</b> as the image
  -  Select <b>Review + create</b> at the bottom, await validation, and finalize creation
 
- Confirm that both vm's are sharing the same <b>virtual network</b>

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/bc30bea0-f7e2-4dba-b8a1-fe0c148a811d)

<br />

<h2>Observing Traffic</h2>

<h3>ICMP</h3>

- Use <b>Remote Desktop</b> to connect to your Windows 10 virtual machine
  -  Locate <b>vm1's public ip address</b> and copy it
  -  Open <b>Remote Desktop</b> by typing it into your <b>Windows search bar</b> and opening the app
  -  Paste <b>vm1's ip address</b> and use the log in settings created for vm1 to connect
 
  ![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/1db301f6-8b4f-4db9-963f-0cdbe6b0ee64)

<br />

- Within your Windows 10 Virtual Machine, <b>Install Wireshark</b>
  -Open your default web browser application and use https://www.wireshark.org/download.html to download the <b>Windows x64 Installer for Wireshark</b>
<b>Note: This free application will allow us to monitor all forms traffic needed for thsi tutorial</b>

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/9f3a1af7-c51c-4b2e-830f-15592afa0c5e)
![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/9e9ab8d2-7991-4435-9662-482807ed2293)

<br />

- Open Wireshark and filter for ICMP traffic only
  -  To start this process, locate the 'blue fin' icon in top left corner of the app and click it.  This will begin the monioring process for all available traffic
  -  Next, locate the <b>filter search bar</b> and type <b>'icmp'</b>

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/0270dadd-60ea-47ca-a375-9aaca7b9e988)

<br />

- Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM
  -  Once the ip address has been located and copied, navigate back to vm1, open <b>Command Prompt<b>, and use ping (ip address)
  -  After pinging vm2, attempt to ping a <b>public website such www.google.com</b>
  -  Observe the traffic results you created within Wireshark

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/5ac03fdc-15d1-4e49-819d-6806d64983a7)
![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/4e9c04a5-d4a6-48f8-b80d-10d983263055)
![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/0b4a080f-6636-4765-ba03-46aed37dd3a1)

<br />

<h4>ICMP Network Security Group Test</h4>

- Initiate a <b>perpetual/non-stop ping</b> from your Windows 10 VM to your Ubuntu VM by using <b>ping -t</b> 

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/ea85f372-4a4e-4837-b204-50ebb3e5f133)
 
 <br />

- Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic
  -  To do this navigate to Azure click on vm2 and click on <b>Network settings</b> on the left
  -  Locate the <b>nsg link (network security group)</b> and click <b>Network Security Rules</b> on the left
  -  Click <b>+ Add</b> and you see a new window pop up on the right side
  -  Select <b>ICMP</b> and choose the <b>Deny</b> option below.
  -  Notice an entry box to set a Priority level.  Each rule has a priority level that determines which rules activate before others, in this case SSH currently having the highest priortity of 300.
  -  Set the ICMP rule's priority to <b>any number that comes before 300, i.e. 299</b> and confirm with <b>Add</b>
  -  Refresh the vm if needed and observe the changes made back in vm1's Wireshark application.

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/f7afdd6d-2a7d-4d60-8181-d17fbacde85c)
![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/56b945b4-e8ad-4490-a19a-99363794af79)

<br />

- Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
- Once again take notice of the changes made within Wireshark and Command Prompt
- End your ping operation in Command prompt with <b>ctrl+C</b>

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/a5076eef-6e61-4eba-a558-d1f4fe3d2929)
![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/fda56e1b-4896-436f-a330-d7a07bbbddab)

<br />

<h3>SSH</h3>

- Back in Wireshark, filter for SSH traffic only

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/95ebca18-f8a8-442d-879b-be3f48151a7d)

<br />

- From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address)
  -  Within Command Prompt, type <b>ssh vmuser(vm2's private ip address)</b> and hit enter
  -  A prompt will request vm2's <b>log in password</b>.  You will not visually see the password text being entered, but continue to type it in to confirm ssh connection.

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/7520a43b-cd2f-4734-b38b-c8d618f9514d)
![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/0042c262-b35d-4898-8724-de574964e4e9)

<br />

- Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
- Exit the SSH connection by typing <b>‘exit’ and pressing [Enter]</b>

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/24aa1abb-570a-42fd-aa49-5b9933cc2b07)

<br />

<h3>DHCP</h3>

- Back in Wireshark, filter for <b>DHCP</b> traffic only

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/a0782bbe-403b-466c-838d-ed007b98a5ea)

<br />

- From your Windows 10 VM, attempt to issue your VM a new IP address from the command line <b>(ipconfig /renew)</b>
- Observe the DHCP traffic appearing in WireShark

<b>Note: Within Azure, there is a dhcp server active that should initiate the ip renewel process.</b>

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/f236769e-831e-4759-abe6-c12dce6bf4fc)

<br />

<h3>DNS</h3>

- Back in Wireshark, filter for DNS traffic only
- From your Windows 10 VM within a command line, use <b></b>nslookup</b> to see what google.com and disney.com’s IP addresses are
- Observe the DNS traffic being show in WireShark

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/0240f999-cfaa-48a0-ba20-4e55d12fa1f4)

<br />

<h3>RDP</h3>

- Back in Wireshark, filter for <b>RDP traffic only ( may have to type: tcp.port == 3389 for the filter to apply)</b>
  - Oserve the immediate non-stop spam of traffic.  This is because of RDP's behavior to live stream what is on screen at all times

![image](https://github.com/dgrofsick/azure-network-protocols/assets/148154704/84883105-79a3-4f38-a870-54d2c820d019)

<br />

<h2>Resource Cleanup</h2>

- Close your Remote Desktop connection
- Delete the Resource Group(s) created at the beginning of this turtorial, including the Network Watcher
- Verify Resource Group Deletion
