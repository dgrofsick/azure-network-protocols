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





