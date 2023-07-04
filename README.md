<p align="center">

![VAS](https://github.com/Sirdsapes/openVAS/assets/137962934/b6f392e5-fb37-4e9b-9b88-4b7fcdfc0719)

</p>

<h1>OpenVAS Vulnerability Management Tutorial</h1>
This tutorial outlines the implementation and usage of OpenVAS using Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- 

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>


<p>
-First thing we'll do is go to the Azure Marketplace and find OpenVAS secured and supported by HOSSTED.
-We'll select Start with a pre-set configuration.
-We'll use Dev/Test and General purpose and click Continue to create VM.
-Create new Resource Group and name it Vulnerability-Management. Name the VM OpenVAS and use a nearby region (keep note of the region you selected). 
-You might get a warning about Azure Automanage not supporting the OpenVAS image. You'll need to go to Management within the VM and disable Automanage.
-Change SSH public key setting to Password and create a username and password.
-Click Review + create and click Next until you get to the Monitoring section and disable Boot diagnostics.
-Click Review + create and then Create when validation has passed.
</p>
<br />

<p>
  
![OpenVAS Start](https://github.com/Sirdsapes/openVAS/assets/137962934/8008907b-45b6-4520-89ca-94d941f1f8c2)
![VAS Dev](https://github.com/Sirdsapes/openVAS/assets/137962934/7bbd5f38-793a-4811-af4e-90c187209d09)
![Vm Automan](https://github.com/Sirdsapes/openVAS/assets/137962934/7c0ce867-4912-44d9-9c28-99c76a129ace)
![VAS Vm](https://github.com/Sirdsapes/openVAS/assets/137962934/dd9b61c8-cb2c-4cbc-8897-2d2ec5c52741)
![VAS Create](https://github.com/Sirdsapes/openVAS/assets/137962934/47e30912-1d73-498c-ac70-3583109eb3a9)

</p>
<p>
-Once that is done deploying, open PowerShell in Windows or Terminal in Mac.
-Open the OpenVAS VM we just created and copy the Public IP address.
-Go back to PowerShell and type in "ssh (username used for VM)@(VM's Public IP)" -- without quotations -- and type yes when asked if you want to continue connecting and enter the password you used when creating the VM.
-Two things to keep of note: underneath HOSSTED, it will list an https:// address to access OpenVAS and underneath that it will list the username and password to access it.
-Open a new window in your internet browser and paste the https link. Login using the username/password given in the terminal.
</p>
<br />

<p>

![VAS IP](https://github.com/Sirdsapes/openVAS/assets/137962934/87fac746-0a85-410b-882b-adf5cc27f8d9)
![VAS Shell](https://github.com/Sirdsapes/openVAS/assets/137962934/c015068f-91f2-4086-a241-74ceac09e7ad)
![VAS SSH](https://github.com/Sirdsapes/openVAS/assets/137962934/915eb48f-9d0d-420c-92ad-377997f35f76)
![VAS Login](https://github.com/Sirdsapes/openVAS/assets/137962934/f063c87f-a134-402c-9ca2-30e18ff41310)

</p>
<p>
-Next, we'll be creating a new VM that will be vulnerable so we can scan it.
-Go back to the Azure portal and create a new Virtual Machine.
-The VM will be in the same Resource Group as before, i.e. Vulnerability-Management. Name it something like VM-Vulnerable, or something akin to that, and make sure it's in the same region as OpenVAS that was done earlier. Image will be Windows 10 Pro instead of OpenVAS and the Size should have 2 VCPUs. Make sure to use an easy username/password for now.
-Click Review + create and Create after validation has passed.
</p>
<br />

<p>

![VM Vuln](https://github.com/Sirdsapes/openVAS/assets/137962934/caa9d9d4-09a1-4b66-a4df-b6628521234f)
![VM Vuln depl](https://github.com/Sirdsapes/openVAS/assets/137962934/47114d7c-35e3-40d5-a09d-8093b679a3c1)


</p>
<p>
-After the VM has successfully deployed, we're going to remote into it. 
-Go to the VM we just created, i.e. Win10-Vulnerable, and copy the Public IP address -- just like we did earlier. You can ignore the "status not ready" message.
-Open Remote Desktop Connection and paste the VM's IP. Enter the username/password you used to create the VM and allow the connection.
-We'll make this VM vulnerable for scanning. 
-Within the remote connection, open Windows Defender Firewall with Advanced Settings. Click Windows Defender Firewall Properties. Turn the firewall to Off and click OK. Make SURE it's the remote connection and not your own computer.
-Next, we're going to download and install some old software in the remote desktop to get some alerts for our scan later (https://drive.google.com/drive/u/2/folders/1n83ilCjZWZulbDdYnUe9wQPK2buY47_U).
-Restart the VM after everything has installed properly and leave the VM alone.
</p>
<br />

<p>

![RDP VM](https://github.com/Sirdsapes/openVAS/assets/137962934/d3d39f7e-aabd-4a8a-b7cd-4434b1677e26)
![Remote Firewall](https://github.com/Sirdsapes/openVAS/assets/137962934/bd821abc-a772-43a8-babb-d607b27693be)

</p>
<p>
-Next, we'll go back to the OpenVAS website we signed into earlier. You might need to log back in.
-We need to add the vulnerable VM to OpenVAS. Go to Assets at the top, click Hosts, and then New Host. It will ask for an IP Address. We need to use the Private IP of the vulnerable VM.
-Go back to the Azure portal, open the vulnerable VM and copy the Private IP, not the Public IP. Private IP should be listed under Networking. Paste the IP. You can Comment with the name of the VM or not. Click OK.
</p>
<br />

<p>

![VAS Host](https://github.com/Sirdsapes/openVAS/assets/137962934/f133c3a8-8bf0-4147-9067-1ffa34873066)
![VM Priv](https://github.com/Sirdsapes/openVAS/assets/137962934/6dea39f2-6365-42a6-9b3b-a31ce2766ae6)
![VM Vuln Host](https://github.com/Sirdsapes/openVAS/assets/137962934/0b6feda4-8755-4500-842b-e669578f2e7c)

</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
