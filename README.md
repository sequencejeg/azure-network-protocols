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
- Ubuntu Server 20.04

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

Open Wireshark and start packet capture
Within Wireshark, filter for ICMP traffic only
Retrieve the private IP address of the Ubuntu VM (linux-vm) and attempt to ping it from within the Windows 10 VM
Observe ping requests and replies within WireShark
From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark

Part 3
(Configuring a Firewall [Network Security Group])
Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM
Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic
Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is
Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
Stop the ping activity

(Observe SSH Traffic)
Log back into the windows-vm
Back in Wireshark, start a packet capture up
Filter for SSH traffic only
From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address)
Open PowerShell, and type: ssh labuser@<private IP address>
Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
Exit the SSH connection by typing ‘exit’ and pressing [Enter]

(Observe DHCP Traffic)
Back in Wireshark, filter for DHCP traffic only
From your Windows 10 VM, attempt to issue your VM a new IP address from the command line
Open PowerShell as admin and run: ipconfig /renew
Observe the DHCP traffic appearing in WireShark

Observe DNS Traffic)
Back in Wireshark, filter for DNS traffic only
From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are
Observe the DNS traffic being show in WireShark

(Observe RDP Traffic)
Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
Observe the immediate non-stop spam of traffic? Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?
Answer: because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted
