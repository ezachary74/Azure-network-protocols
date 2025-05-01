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

- Create two virtual machines
- Observe ICMP traffic
- Configure a firewall
- Observe SSH, DHCP, DNS and RDP traffic

<h2>Actions and Observations</h2>


![image](https://github.com/user-attachments/assets/8f7a1771-7900-4d02-89a3-0e2194be5a0f)

![image](https://github.com/user-attachments/assets/c8cc4878-c30d-429c-8ee7-5d46c10d4d72)


</p>
<p>
Create two virtual machines in the Azure portal. One VM uses a Windows 10, while the other uses Ubuntu 22. When creating both VMs ensure that both VMs are attached to the same virtual network. 
</p>
<br />


![image](https://github.com/user-attachments/assets/5a58553f-9e64-4ea8-a261-eb0782485003)

![image](https://github.com/user-attachments/assets/4f27188a-fb37-4ba6-bdbd-7c3b6119f72c)


</p>
<p>
After logging into the Windows VM utilizing remote desktop, download and install Wireshark from https://www.wireshark.org. This tool will be used to observe network traffic. After installing Wireshark open the application and filter for ICMP traffic. ICMP is used to check connectivity between network devices.
</p>
<br />

![image](https://github.com/user-attachments/assets/36b479eb-b5ca-4d8a-847e-ec8463da11c2)

![image](https://github.com/user-attachments/assets/c51c3e0a-ae94-4fbd-9e27-203bbec861a3)

![image](https://github.com/user-attachments/assets/7e7c44cc-061b-4ff0-ade9-f7757af9b47f)



</p>
<p>
Retrieve the private IP address of the linux VM and attempt to ping it from within the Windows 10 VM, via PowerShell. You should see ping requests and replies within Wireshark. You can also experiment and attempt to ping a public website, or use command -t to observe constant traffic from the linux VM.


![image](https://github.com/user-attachments/assets/276b5c79-a475-4391-ad4c-7e9e65c24039)

![image](https://github.com/user-attachments/assets/ebee53af-6019-4d8b-9f8b-84a707f79c6f)

![image](https://github.com/user-attachments/assets/21c9e721-2fba-42c9-ba84-c278d6ae5150)


Navigate to the Linux VM network settings/inbound security rules and create an inbound port rule to deny ICMP traffic. This creates a firewall, blocking the ICMP traffic that the Windows VM would ping to the Linux VM. Navigate to the Windows VM to observe Wireshark communication. You should see that the pings are no longer receiving replies from the Linux VM. In PowerShell you will see a repeated “Request Timed Out” response, as well as seeing "no response found!" on Wireshark.


![image](https://github.com/user-attachments/assets/0fb896d7-3e31-47c6-8703-b6856d4208f4)


Following this, go back to the Linux VM and delete the inbound port rule. This allows the ICMP traffic to be received between the two VMs once again.


![image](https://github.com/user-attachments/assets/c311ed69-352a-48c4-9160-a1dc22aa3c42)

![image](https://github.com/user-attachments/assets/98ae200f-cb75-4f50-a285-8aabc7440fc7)


Now, navigate to the Windows VM and filter SSH traffic using Wireshark. Open PowerShell the Windows VM and establish an SSH connection to the Linux VM using the command: ssh username@linux vm private ip address. When the command is performed Wireshark displays the SSH traffic. You can type "exit" in the command line to stop the ssh traffic.


![image](https://github.com/user-attachments/assets/09623088-b174-4765-8c34-bea1b0eaf24e)


To observe DHCP traffic navigate to Wireshark, filter for DHCP traffic only. In your Windows 10 VM, attempt to issue your VM a new IP address from the command line. Open PowerShell as admin and run the command: ipconfig /renew. You should see the DHCP traffic appearing in WireShark.


![image](https://github.com/user-attachments/assets/82309e20-d03b-444c-b87f-796a95922f29)


Continue by filtering DNS traffic in Wireshark. Begin by setting Wireshark to display DNS traffic only. To observe DNS activity use the command: nslookup www.google.com. This queries the DNS server for Google’s IP address.


![image](https://github.com/user-attachments/assets/b8ad193a-2911-48bc-a787-ca38bd4ae24a)


To conclude, filter for RDP traffic in Wireshark. The RDP traffic will appear rapidly since RDP is constantly streaming a "picture" of the server to the VM.




