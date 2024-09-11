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

- Windows 10 (22H2)
- Ubuntu Server 22.04

<h2>Actions and Observations</h2>

![Screenshot 2024-09-10 at 6 17 54 PM](https://github.com/user-attachments/assets/731b9a36-7b38-487e-99e6-8d63622731df)

Step 1. In Azure, Setup a Resource Groups 

![Screenshot 2024-09-10 at 6 19 36 PM](https://github.com/user-attachments/assets/34549991-3de8-4580-8d88-e824c490a784)
![Screenshot 2024-09-10 at 6 22 10 PM](https://github.com/user-attachments/assets/17cae3ab-fc3c-4ac6-ab41-4760175f56df)
![Screenshot 2024-09-10 at 6 27 28 PM](https://github.com/user-attachments/assets/bbfc138d-aec3-49da-956b-e258cc7e6e3f)
![Screenshot 2024-09-10 at 6 27 42 PM](https://github.com/user-attachments/assets/640de476-d7cb-4708-be09-7e3b37479f7f)
![Screenshot 2024-09-10 at 6 45 44 PM](https://github.com/user-attachments/assets/ea8cecd6-9bba-4afa-bdfc-b854010073a1)


Step 2. Create 2 Virtual Machines 
 - Create a Windows 10 Virtual Machine (Windows-VM)
 - While creating the VM, select the previously created Resource Group
 - While creating the VM, create a new Virtual Network (Vnet) 
 - Create a Linux Virtual Machine (Linux-VM)
 - While create the VM, select the previously created Resource Group and Vnet
 - Observe Your Virtual Network within Network Watcher

Note: Wait for the Windows-VM to finish Deploying before creating your Linux-VM. If not, the Vnet you created on your Windows-VM will not show up when creating the Linux-VM.

![Screenshot 2024-09-10 at 6 49 23 PM](https://github.com/user-attachments/assets/cafdc409-0572-47ab-8c16-628707f69ec7)

Step 3. Open Microsoft Remote Desktop (If on Mac)
 - Click Add PC
 - for PC name, use Windows-VM Public IP Address
 - Open Windows-VM on the Microsoft Remote Desktop after adding the PC

![Screenshot 2024-09-10 at 7 37 03 PM](https://github.com/user-attachments/assets/3aac608b-bdb4-4bae-ac35-90b8023d34d2)

Part 4. Install Software Within Windows-VM
 - Install WireShark
 - WireShark.org -> install -> Windows x64 installer
 - Open Wireshark filter for ICMP traffic only by hitting the blue shark icon and typing in "ICMP"

![Screenshot 2024-09-10 at 7 36 44 PM](https://github.com/user-attachments/assets/50faedb9-c365-4ba3-bb99-404f523515b3)
![Screenshot 2024-09-10 at 7 37 50 PM](https://github.com/user-attachments/assets/d1122397-0f24-4bd8-ae8f-f7bc1cfc0446)

 - Retrieve the private IP address of the Linux-VM and attempt to ping it from within the Windows-VM
 - Observe ping request and replies within wireshark

![Screenshot 2024-09-10 at 7 38 44 PM](https://github.com/user-attachments/assets/f8ef53fe-5f81-48ca-a143-6ea5c16a3848)



Part 5. Initiate a perpetual/non-stop ping from your Windows-VM to your Linux-VM
 - On PowerShell, type in the private IP again along with a nonspot pin "-t"
 - Observe the ICMP traffic in WireShark and the command line Ping activity

![Screenshot 2024-09-10 at 7 52 00 PM](https://github.com/user-attachments/assets/ab583e3e-c799-4b4f-b3f7-7ece5553adb2)

 - Open the Network Security Group your Linux-VM is using and disable incoming (inbound) ICMP traffic
 - Network Security group -> Linux-VM-nsg Inbound security rules -> Add
 - Back in the Windows-VM, observe the ICMP traffic in WireShark and the command line Ping activity after adding a new rule

![Screenshot 2024-09-10 at 7 48 10 PM](https://github.com/user-attachments/assets/b1aeee0a-48da-4362-88d1-0c25e81ecc4d)

 - "Request timed out"
 - Re-enable ICMP traffic for the Network Security Group your Linux-VM is using
 - Back in the Windows-VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
 - Stop the ping activity with shift+control+C

![Screenshot 2024-09-10 at 7 56 17 PM](https://github.com/user-attachments/assets/29598a28-b2c4-4bc2-9e63-896928ab91c0)
![Screenshot 2024-09-10 at 7 56 36 PM](https://github.com/user-attachments/assets/03a79535-4807-4376-8589-f201f181b0b6)

 Part 6. Back in Wireshark, filter for SSH traffic only
  - From your Windows-VM, “SSH" into your Linux-VM (via its private IP address)
  - Exit the SSH connection by typing ‘exit’ and pressing [Enter]


![Screenshot 2024-09-10 at 8 00 01 PM](https://github.com/user-attachments/assets/bbfbb040-fbc9-48c9-8cbd-b3787a918aa0)

Part 7. Filter for DHCP traffic only 
 - From your Windows-VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
 - Observe the DHCP traffic appearing in WireShark


![Screenshot 2024-09-10 at 8 02 07 PM](https://github.com/user-attachments/assets/0dbe3aa3-9b74-4f3f-9e01-2026d9af9ccc)
![Screenshot 2024-09-10 at 8 02 33 PM](https://github.com/user-attachments/assets/a35b28bc-1642-4c85-b535-6ac9480d4568)


Part 8. Filter for DNS traffic only 
 - From your Windows-VM within a command line, use nslookup to see what google.com and amazon.com’s IP addresses are
 - Observe the DNS traffic being show in WireShark

 - This concludes the lab

