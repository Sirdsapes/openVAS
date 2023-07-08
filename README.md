<p align="center">

![VAS](https://github.com/Sirdsapes/openVAS/assets/137962934/b6f392e5-fb37-4e9b-9b88-4b7fcdfc0719)

</p>

<h1>OpenVAS Vulnerability Management Tutorial</h1>
This tutorial outlines the implementation and usage of OpenVAS using Azure Virtual Machines and OpenVAS services.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- OpenVAS

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)

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

-The VM will be in the same Resource Group as before, i.e. Vulnerability-Management. Name it something like "VM-Vulnerable", or something akin to that, and make sure it's in the same region as OpenVAS that was done earlier. Image will be Windows 10 Pro instead of OpenVAS and the Size should have 2 VCPUs. Make sure to use an easy username/password for now.

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
-Within the remote connection, open Windows Defender Firewall with Advanced Settings. Click Windows Defender Firewall Properties. Turn the firewall for Domain, Public, and Private to Off and click OK. Make SURE it's the remote connection and not your own computer.
-Next, we're going to download and install some old software in the remote desktop to get some alerts for our scan later (https://drive.google.com/drive/u/2/folders/1n83ilCjZWZulbDdYnUe9wQPK2buY47_U).
-Restart the VM after everything has installed properly and leave the VM alone.
</p>
<br />

<p>

![RDP VM](https://github.com/Sirdsapes/openVAS/assets/137962934/d3d39f7e-aabd-4a8a-b7cd-4434b1677e26)
![Remote Firewall](https://github.com/Sirdsapes/openVAS/assets/137962934/bd821abc-a772-43a8-babb-d607b27693be)
![VM Fire](https://github.com/Sirdsapes/openVAS/assets/137962934/5a9928d9-deac-44c0-a9e4-d02b78d27a1d)

</p>
<p>
-Next, we'll go back to the OpenVAS website we signed into earlier. You might need to log back in.
-We need to add the vulnerable VM to OpenVAS. Go to Assets at the top, click Hosts, and then New Host. It will ask for an IP Address. We need to use the Private IP of the vulnerable VM.
-Go back to the Azure portal, open the vulnerable VM and copy the Private IP, not the Public IP. Private IP should be listed under Networking. Paste the IP. You can Comment with the name of the VM or not. Click Save.
-We then need to create a new Target from the Host we just made. Under Actions click Create New Target. Name the target something like "Vulnerable VMs", or whatever you decide. Leave everything as-is and click Save.
-Next, go to Scans and Tasks. Create a New Task. Name it something like "Scan - Azure Vulnerable VMs", or whatever you decide. Scan Targets will be the Target we just made, i.e. Azure Vulnerable VMs. Leave everything else as-is and click Save.
-It will keep us in the Task section of OpenVAS. We're going to start a scan of the vulnerable VM. To do this, we need to click Start under the Actions section. Click Start and it will begin scanning. The Status will show Requested and then % it has completed scanning until complete.
-You can click Last Report to open the scan details.
-This wasn't a credentialed scan, so nothing was really found. So, we will set up a credentialed scan.
</p>
<br />

<p>

![VAS Host](https://github.com/Sirdsapes/openVAS/assets/137962934/f133c3a8-8bf0-4147-9067-1ffa34873066)
![VM Priv](https://github.com/Sirdsapes/openVAS/assets/137962934/6dea39f2-6365-42a6-9b3b-a31ce2766ae6)
![VM Vuln Host](https://github.com/Sirdsapes/openVAS/assets/137962934/0b6feda4-8755-4500-842b-e669578f2e7c)
![VAS Threat](https://github.com/Sirdsapes/openVAS/assets/137962934/191797cc-aa54-4f8b-9aca-169865574567)
![VAS Task](https://github.com/Sirdsapes/openVAS/assets/137962934/cca19b2b-23f6-4842-a147-2c8b57834e0b)
![VAS Start](https://github.com/Sirdsapes/openVAS/assets/137962934/05f0df1e-18d3-4d8e-9916-b24dcd5b0db1)
![Scan perc](https://github.com/Sirdsapes/openVAS/assets/137962934/c28f4dc6-2186-49a7-a703-08e6de697e9b)
![Scan Done](https://github.com/Sirdsapes/openVAS/assets/137962934/6ee455f9-2929-4113-bed6-349566174feb)
![Scan Report](https://github.com/Sirdsapes/openVAS/assets/137962934/35cf4e72-4aa5-4457-baee-4d694d59b8a4)

</p>
<p>
-Go back to the remote desktop connection to the vulnerable VM from before. We need to disable User Account Control and Enable Registry.
-Open User Account Control Settings and drag it down to Never notify. Click OK.
-Next, open the Services app. Scroll down to Remote Registry. Double-click and change it from Disabled to Automatic. Click Apply and OK.
-Next, we'll set up a Registry Key. Open Registry Editor (regedit) as administrator, i.e. right-click and choose Run as administrator.
-Open HKEY_LOCAL_MACHINE. Open SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, right-click and create New DWORD (32-bit) Value. Name it LocalAccountTokenFilterPolicy. Set Value to 1.
-Restart the VM and remote back in.
</p>
<br />

<p>

![UAC Set](https://github.com/Sirdsapes/openVAS/assets/137962934/469d49a6-7f0d-4863-9935-f8512dc6adfa)
![Remote Reg](https://github.com/Sirdsapes/openVAS/assets/137962934/f8978024-5366-49da-aca0-12b6a0742e21)
![Reg Edit](https://github.com/Sirdsapes/openVAS/assets/137962934/881804ba-562b-4590-ad54-4821388b2ffb)

</p>
<p>
-Next, go back to OpenVAS. Go to Configurations click Credentials and create New Credential. Name it something like "Azure VM Credentials", or whatever you decide. Allow insecure use should be set to Yes. Create a username and password and click Save.
-Next, go to Configuration and open Targets. Clone the vulnerable VM target (click the sheep icon under Actions) and name the clone something like "Azure Vulnerable VMs - Credentialed Scan", or whatever you decide. Under the SMB section of the clone, select Azure VM Credentials -- what we just created prior -- and click Save.
</p>
<br />

<p>

![VAS Credential](https://github.com/Sirdsapes/openVAS/assets/137962934/e794a38f-f9ed-4942-aa2a-28867f76995e)
![VM Clone](https://github.com/Sirdsapes/openVAS/assets/137962934/c6bb491d-159c-4b8b-ac95-e0b0b20b548d)

</p>
<p>
-Go back to Scans and Tasks. Clone the scan we did earlier. Name it something like "Scan - Azure Vulnerable VMs - Credentialed", or whatever you decide. Change Scan Targets from Azure Vulnerable VMs to Azure Vulnerable VMs - Credentialed Scan and click Save.
-We'll perform a new scan just like before by clicking Start using the clone we just created. Click the Play button and let the scan start. This will likely take longer than the first scan we did. If VAS logs you out, log back in.
-You'll notice a big difference in terms of severity level. In my case, I accidentally forgot to completely turn off the firewalls in the first scan; however, non-credentialed scans don't uncover near as much as credentialed, regardless, because credentialed scans have access to the inner workings of a system or application. (see addendum at bottom)
-Clicking the credentialed report opens it. The Results section shows what threats were detected.
-We can see, in this instance, Adobe and Firefox showing up quite often. As you recall, these were old versions of programs we installed that aren't as secure/up-to-date as newer versions.
-If we click one of these vulnerabilities, we get a report on it. VAS will list all relevant information and even has a suggested Solution, or remediation.
</p>
<br />

<p>

![Clone Task](https://github.com/Sirdsapes/openVAS/assets/137962934/76e169ca-b9bc-465f-82ba-fbb3ade418d3)
![Clone Scan 2](https://github.com/Sirdsapes/openVAS/assets/137962934/511187c1-0226-4612-a108-84a780ed4102)
![Clone Threats](https://github.com/Sirdsapes/openVAS/assets/137962934/278f32c8-90d5-4ee3-9b54-5a4ded6d1a09)
![Clone Threat Info](https://github.com/Sirdsapes/openVAS/assets/137962934/c12d0626-7531-438a-95e7-ce57dcf84585)
![Clone Solut](https://github.com/Sirdsapes/openVAS/assets/137962934/edd6a516-a5d1-4752-a418-aa2fae3066d2)

</p>
<p>
-Next, we'll remote back into the vulnerable VM -- if it's not connected, already.
-Back in the remote desktop, we're going to uninstall the out-of-date programs we installed earlier. Type "uninstall" in the Windows Search Bar and open Add or remove programs.
-Restart the VM when the programs have finished uninstalling.
-Back in OpenVAS, perform another credentialed scan. Again, this will take a while.
-Even though the severity is still high, we can see a downward trend in threats.
-When we go to the Results page, the number of threats is FAR fewer than the previous scan. You'll also notice no threats from the programs we uninstalled, i.e. Firefox, Adobe Reader, and VLC.
-If you want to remediate any of these issues, you can always click one and find the solution proposed by OpenVAS and see if it works.
</p>
<br />

<p>

![VM Unin](https://github.com/Sirdsapes/openVAS/assets/137962934/06fba48e-a958-4900-abd0-f163145a5aa4)
![Second Scan](https://github.com/Sirdsapes/openVAS/assets/137962934/c59b5900-b736-4bb8-8ad6-05820cc6066d)
![Scan Result](https://github.com/Sirdsapes/openVAS/assets/137962934/a39e925c-34dc-40e4-bd4c-d6cd8e58013e)

</p>
<p>
-That concludes this lab of using OpenVAS to perform a basic vulnerability scan and usage of Azure services to help facilitate this project. Remember to delete all resource groups in Azure to not incur further costs.

-As an addendum, I re-ran a non-credentialed scan to show the base difference when firewalls are truly turned off v. accidentally leaving some on. Non-credentialed with Private/Public accidentally left on; below is Private/Public turned off.
</p>
<br />

![Scan Non Cred](https://github.com/Sirdsapes/openVAS/assets/137962934/206e2754-4e2a-4f4c-8a32-aa57af48c549)


