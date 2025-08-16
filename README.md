<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Prerequisite</h2>

- [Configuring A Virtual Machine and Remote Access](https://github.com/sequencejeg/configure-vm)


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Windows App (macOS)
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- macOS
- Windows 10 (21H2)
- Ubuntu Server 20.04 (Linux)

<h2>High-Level Steps</h2>

- Step 1: Connect to VM using RDP & Install Wireshark
- Step 2: Observe ICMP Traffic
- Step 3: Use NSG (Firewall) to Deny Ping
- Step 4: Observe SSH and DHCP Traffic
- Step 5: Observe DNS and RDP Traffic 

<h2>Actions and Observations</h2>

<h3>Step 1: Connect to VM using RDP & Install Wireshark</h3>
<p>

<p>
- Start by logging into your Azure account and go to the Virtual Machines section. From here, we will need to create our VMs. Create both VMs (A Windows-VM and a Linux-VM), note the Public IP for the Windows VM. We will need this later.*
</p>

<img width="1225" height="613" alt="Screenshot 2025-08-16 at 2 24 58 AM" src="https://github.com/user-attachments/assets/34001070-dd3e-416e-9340-e6f1167e2f6a" />
<img width="1225" height="613" alt="Screenshot 2025-08-16 at 2 25 24 AM" src="https://github.com/user-attachments/assets/e7a0807f-5a53-45ff-ae85-e9d74ada0ec5" />

<p>
- After deployment, the VMs should be up and running.

<p>
<img width="1223" height="705" alt="Screenshot 2025-08-16 at 2 27 54 AM" src="https://github.com/user-attachments/assets/51d266e5-5d85-4bde-92f6-7e313e69c008" />

</p>

<p>
- From your device, you will need to open Remote Desktop (Windows) or the Windows App (MacOS). I will be using the Windows App because I am running MacOS. Once you open the Windows App, click the "+" dropdown button at the top right of your screen. Then, select "Add PC".
</p>
<br />
<p>
<img width="1223" height="705" alt="Screenshot 2025-08-16 at 2 28 07 AM" src="https://github.com/user-attachments/assets/db435840-3e5c-47a8-8e26-9332b293cc59" />

 </p>
<p>
- Enter the Public IP address for the windows-vm in "PC name". For the Friendly name, I used "Win-vm" to keep it simple. Click "Add".   
</p>
<br />

<p>
<img width="1223" height="705" alt="Screenshot 2025-08-16 at 2 35 21 AM" src="https://github.com/user-attachments/assets/c4942e28-5ff9-48d0-8aac-e53c3ec0d871" />

</p>

<p>
- Enter the Username and Password for the windows-vm that we created in Azure. Click "Continue".
</p>
<br />

<p>
<img width="1229" height="797" alt="Screenshot 2025-08-16 at 2 38 19 AM" src="https://github.com/user-attachments/assets/14a10bf6-774d-4994-8c10-545bd4b88a97" />

</p>

<p>
- We are now connected to the Windows-vm. Using Microsoft Edge, go to www.wireshark.org to dowload Wireshark to the VM. Wireshark is a network protocol analzyer that lets you see what's happening on your network. We will use this tool to observe network traffic with various protocols.
</p>
<br />

<p>
<img width="1229" height="797" alt="Screenshot 2025-08-16 at 2 41 53 AM" src="https://github.com/user-attachments/assets/a9cfeda4-eda2-48e4-89a9-7e5477c85153" />

</p>

<p>
- Open file and run the Wireshark setup. Click "Next" and "Finish" a couple times to complete the setup.
</p>
<br />

<h3>Step 2: Observe ICMP Traffic</h3>
<p>
<img width="1226" height="500" alt="Screenshot 2025-08-16 at 2 45 40 AM" src="https://github.com/user-attachments/assets/68fae779-1ad8-4f1b-ae57-e8812e1133aa" />
</p>

<p>
- Once completed, open Wireshark.
</p>
<br />

<p>
 <img width="1226" height="500" alt="Screenshot 2025-08-16 at 1 15 19 PM" src="https://github.com/user-attachments/assets/606bfe95-7b83-4269-ab95-15fe7cbde32d" />
</p>

<p>
- Start packet capture
</p>
<br />

<p>
<img width="1226" height="500" alt="Screenshot 2025-08-16 at 1 17 40 PM" src="https://github.com/user-attachments/assets/d6cf0571-e65f-4b96-a3f5-e94f77da39bf" />
</p>

 <p>
  - Within Wireshark, filter for ICMP traffic only
 </p>
 <br />

<p>
 <img width="1227" height="727" alt="Screenshot 2025-08-16 at 1 24 25 PM" src="https://github.com/user-attachments/assets/1a02ca53-a6f2-4655-b00f-6616a6d4aecf" />
</p>

<p>
 - Retrieve the private IP address of the Linux-vm  and attempt to ping it from within the Windows 10 VM. Observe ping requests and replies within WireShark
</p>
<br />
 
 <p>
  <img width="1227" height="727" alt="Screenshot 2025-08-16 at 1 49 35 PM" src="https://github.com/user-attachments/assets/1f13215d-dcbd-4b50-9081-48fbc3ae4946" />
 </p>
  
 <p>
  - From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark
 </p>
<br />

<h3> Step 3: Configuring a Firewall [Network Security Group </h3>

<p>
<img width="942" height="321" alt="Screenshot 2025-08-16 at 2 09 01 PM" src="https://github.com/user-attachments/assets/c8942c09-2d79-475b-b379-e59ebfd188ce" />
</p>

<p> 
- Initiate a perpetual/non-stop ping from your Windows 10 VM to your Linux VM "ping private ip address -t" 
</p>
<br />

<p>
 <img width="1230" height="614" alt="Screenshot 2025-08-16 at 2 13 39 PM" src="https://github.com/user-attachments/assets/7676c409-3a91-4b66-8d82-ce7a4165e6e7" />
<img width="1230" height="614" alt="Screenshot 2025-08-16 at 2 14 46 PM" src="https://github.com/user-attachments/assets/0215afb3-4439-4c97-bf42-9760ba92570d" />
<img width="1230" height="614" alt="Screenshot 2025-08-16 at 2 16 58 PM" src="https://github.com/user-attachments/assets/c97fea4e-a477-4a60-8ceb-daf618a315eb" />
</p>
 
<p> 
 - Open the Network Security Group your Linux VM is using and disable incoming (inbound) ICMP traffic 
</p>
<br />

<p>
 <img width="1230" height="648" alt="Screenshot 2025-08-16 at 2 18 10 PM" src="https://github.com/user-attachments/assets/bde215da-6a06-427d-9c36-78ad9b2c5d89" />
</p>

<p> 
 - Back in the Windows VM, observe the ICMP traffic in WireShark and the command line Ping activity. It should start demosntrating errors. 
</p>
<br />

<p>
 <img width="1223" height="616" alt="Screenshot 2025-08-16 at 2 18 45 PM" src="https://github.com/user-attachments/assets/d2490024-9df2-471d-9cc7-7778ee06b514" />
<img width="1228" height="737" alt="Screenshot 2025-08-16 at 2 19 52 PM" src="https://github.com/user-attachments/assets/1c00083c-75d6-41cd-bc7d-118869b7f722" />
</p>

<p> 
 - Delete the inbound rule or Re-enable ICMP traffic for the Network Security Group your Linux VM. Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity. Now the connection should be working again. Afterwards, stop the ping activity. 
</p>
<br />

<h3> Step 4: Observe SSH and DHCP Traffic </h3>

<p>
 <img width="1230" height="422" alt="Screenshot 2025-08-16 at 2 25 45 PM" src="https://github.com/user-attachments/assets/3483cf98-cffc-4934-bb63-8dec8e3acd28" />
</p>

<p>
 - Back in  our Windows-VM, return to Wireshark, start a packet capture up and Filter for SSH traffic only
</p> 
<br />

<p>
 <img width="1230" height="735" alt="Screenshot 2025-08-16 at 2 26 11 PM" src="https://github.com/user-attachments/assets/9dce0966-c322-41f5-ac71-fb027614233c" />

</p>

<p> 
 - From your Windows Virtual Machine, “SSH into” your Linux Virtual Machine using its private IP address. To do this, open PowerShell, and type: "ssh labuser@private ip address"
 </p> 
 <br /> 

<p>
 <img width="1230" height="735" alt="Screenshot 2025-08-16 at 2 26 36 PM" src="https://github.com/user-attachments/assets/4ad3ecbc-2a23-45a9-9cd5-80922618ace6" />
<img width="1195" height="719" alt="Screenshot 2025-08-16 at 2 28 01 PM" src="https://github.com/user-attachments/assets/85aa0a4a-1284-4a22-94bb-80fb29570f9a" />
</p>
  
<p> 
 - Once logged in, Type commands (username, pwd, wrong commands, etc) into the linux SSH connection and observe SSH traffic spam in WireShark. Any action you take while your connected should reflect SSH traffic. 
</p>
<br />

<p>
 <img width="1138" height="72" alt="Screenshot 2025-08-16 at 2 28 38 PM" src="https://github.com/user-attachments/assets/1db1bb0a-be57-496f-92ee-89f164e7ecab" />
</p>

<p> 
 - Once finished observing, exit the SSH connection by typing ‘exit’ and pressing "Enter" 
</p> 
<br />

<p>
 <img width="1201" height="412" alt="Screenshot 2025-08-16 at 2 32 03 PM" src="https://github.com/user-attachments/assets/5d0baa4e-6ad7-4c46-b4ad-1a3b27acc10e" />
</p>

<p> 
 - Back in Wireshark, filter for DHCP traffic only 
</p> 
<br />

<p>
 <img width="1069" height="303" alt="Screenshot 2025-08-16 at 2 32 57 PM" src="https://github.com/user-attachments/assets/8ec1b5af-66ee-4127-8991-b851fc5c94b5" />
</p>

<p> 
 - From your Windows VM, attempt to issue your VM a new IP address from the command line. Open PowerShell as admin and run: ipconfig /renew 
</p> 
<br />

<p>
 <img width="1202" height="405" alt="Screenshot 2025-08-16 at 2 33 05 PM" src="https://github.com/user-attachments/assets/093dc442-66fb-4c32-8e56-86956bbc78f3" />
</p>
 
<p> 
 - Observe the DHCP traffic appearing in WireShark 
</p> 
<br />

<h3> Step 5: Observe DNS and RDP Traffic) </h3>

<p>
<img width="1202" height="405" alt="Screenshot 2025-08-16 at 2 34 53 PM" src="https://github.com/user-attachments/assets/48804d75-a459-4c22-99bb-b5df1891f2c4" />

</p>

<p>
 - Back in Wireshark, filter for DNS traffic only
</p>
<br />

<p>
 <img width="1064" height="249" alt="Screenshot 2025-08-16 at 2 36 56 PM" src="https://github.com/user-attachments/assets/b4f50dfb-8b51-43cc-a96d-d6f06d77650a" />
<img width="1199" height="408" alt="Screenshot 2025-08-16 at 2 37 24 PM" src="https://github.com/user-attachments/assets/fb1d0278-6003-47a1-8f0e-a932cde92d58" />
<img width="1200" height="713" alt="Screenshot 2025-08-16 at 2 37 56 PM" src="https://github.com/user-attachments/assets/2bd303e8-0ea6-450b-8a1f-0d081cab99ae" />
</p>

<p> 
 - From your Windows VM within a command line, use the "nslookup" command to see what google.com and disney.com’s IP addresses are. Observe the DNS traffic being show in WireShark
</p>
<br />

<p>
 <img width="1193" height="407" alt="Screenshot 2025-08-16 at 2 40 36 PM" src="https://github.com/user-attachments/assets/67d67265-996e-4aa7-b63e-a3a6a86c68c2" />
</p>

<p>
 - Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
Observe the immediate non-stop spam of traffic. This is the case because we are currently using the RDP network protocol (Windows app for macOS users or Remote Desktop Application for Windows) showing you a live stream from your computer to the VM, therefor traffic is always being transmitted
</p>
<br />

<h3> Conclusion> </h3>

<p> This concludes this project. To recap, we used two VMs (One running windows and the other linux) running on the same VNet and tested out diffrent network protocols. We used Wireshark to be able to see the diffrent kinds of traffic going throgh the network, filtered to see ICMP, SSH, DHCP, DNS, and RDP protocols respectively while we engaged with the VM using Powershell commands. </p>
