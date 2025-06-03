<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
  
![image](https://github.com/user-attachments/assets/f1ed7a45-a387-4fa0-b2f0-0570f5b68841)
</p>
<p>
First, create two virtual machines on Microsoft Azure. One should be a Linux VM, and the other a Windows 10 VM. Each machine must be configured with two CPUs and placed on the same Virtual Network (VNET) to ensure they can communicate with each other. Once the VMs are set up, connect to the Windows 10 machine and download Wireshark from the official website: https://www.wireshark.org/download.html. After installing Wireshark, open the application and set a filter for ICMP traffic only. ICMP (Internet Control Message Protocol) is a network layer protocol used to send diagnostic messages and report network connectivity issues. The ping command uses ICMP to test connectivity between hosts. By filtering for ICMP packets in Wireshark and pinging the private IP address of the Linux VM from the Windows VM, you can visually observe the ICMP packets being transmitted and received in real time.

</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/ad587921-8d8f-4873-b31d-d43f59ebac46)
</p>
<p>
We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/d3925a00-8881-4f54-89ee-5ce10fcedc54)
</p>
<p>
In the next part of the lab, we’ll use the ping -t command on the Windows machine to continuously ping the Linux machine. While this is running, go to the Linux machine and block incoming ICMP traffic using its firewall. Once ICMP is blocked, the Windows machine will stop receiving ping replies from the Linux machine. To block ICMP, create a new Network Security Group (NSG) for the Linux VM and add a rule to deny ICMP traffic. If you want to allow ICMP again later, you can do that by changing the NSG settings in the Azure portal.
</p>
<br />

<p>  
  
![image](https://github.com/user-attachments/assets/5eb7f7a4-0424-4203-ae2f-cdd1075f750e)

![image](https://github.com/user-attachments/assets/385623af-b16a-4571-8e41-f67dbaf0c997)
</p>
<p>
Next, we’ll use the Windows machine to connect to the Linux machine using SSH. SSH doesn’t have a graphical interface—it just gives us access to the Linux command line. Before connecting, we’ll set the Wireshark filter to show only SSH packets. Then, on the Windows machine, we’ll run this command in Command Prompt: ssh labuser@10.0.0.5 As soon as we do this, Wireshark will start showing SSH traffic between the two machines.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/2ce1de70-4e5a-465d-8e08-2173992bfa72)
</p>
<p>
Now we’ll use Wireshark to filter for DHCP traffic. DHCP (Dynamic Host Configuration Protocol) uses ports 67 and 68 and is responsible for assigning IP addresses to devices. To see this in action, we’ll run the command ipconfig /renew on the Windows machine. This asks for a new IP address. When we do this, Wireshark will capture the DHCP packets being sent and received.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/fcfbb613-b06c-492f-9fcf-0239ca6a8e43)
</p>
<p>
Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup disney.com" this command essentially asks our DNS server what is disney's IP address.
</p>
<br />

<p>
  
![image](https://github.com/user-attachments/assets/240a1235-4ddc-46a1-8ecb-d3c956bbe8dc)
</p>
<p>
Lastly we will filter for RDP traffic. When we enter tcp.port traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine.
</p>
<br />
