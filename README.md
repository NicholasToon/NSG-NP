
![Image](https://i.imgur.com/xyzcfYE.png)

# Virtual Machine Traffic Inspection

In this tutorial we will observe various kinds of network trafffic between our two virtual machines with Wireshark. We will also briefly explore the Network Security Groups (NSGs) function within the Azure suite in this tutorial. This exploration will include augmenting rules and simulating various scenarios that one might encounter when monitoring network traffic.

This tutorial assumes that you have created virtual machines in Azure. If needed you can learn [here](https://github.com/NicholasToon/Creating-Resource-Groups-and-Deploying-Virtual-Machines-in-Azure) (Just skip to the VM section). The process will be the same for the Linux server: select **Ubuntu Server 20.04 LTS - x64 Gen2**, keep it in the same region. Change **Authentication type** to **Password**. We will be connecting via SSH later, so there is no need to have **SSH public key** selected. For the sake of ease, use the same credentials and write them down if needed (not recommended in a professional environment but okay for the sake of these low-level exercises). Go to the **Networking** tab and select the **Virtual network** that your first Windows VM created. Skip forward and **Create**.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection (RDC)
- Wireshark (Protocol Analyzer)
- Command-Line Tools 
- Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)

## Operating Systems Used

- Windows 10 (21H2)
- Linux/Ubuntu Server 20.04

<h2>High-Level Steps</h2>
First, I created two virtual machines - one with Windows 10 and another with Ubuntu.
Using Remote Desktop, I connected to the Windows 10 VM to proceed with the lab.
Inside the Windows 10 VM, I installed Wireshark and opened it along with Windows PowerShell.
I initiated a continuous ping to the Ubuntu VM from the Windows 10 VM, monitoring the ICMP traffic in Wireshark.
To observe the impact, I added an inbound rule in the Ubuntu VM's Network Security Group to block ICMP traffic, causing the pings to fail.
Afterwards, I re-enabled ICMP traffic in the Ubuntu VM's Network Security Group, which restored the ping functionality. I then ended the ping activity.
Next, I SSHed into the Ubuntu VM and observed the SSH traffic in Wireshark. After completing the observation, I exited the SSH session.
In the Windows 10 VM, I attempted to obtain a new IP address by running the command "ipconfig /renew" and monitored the DHCP traffic in Wireshark.
Using the nslookup command, I examined the DNS traffic in Wireshark.
Lastly, I observed the ongoing RDP (Remote Desktop Protocol) traffic in Wireshark.
As a final step, I deleted the Resource Groups in Azure.
<h2>Synopsis</h2>
<p>
<img src="https://i.imgur.com/PuaPt3D.png" height="70%" width="70%"/>
</p>
<p>
To start this lab, I created two virtual machines - one with Windows 10 and the other with Ubuntu. It was essential to ensure that both VMs were in the same virtual network (vnet) and subnet. In this case, VM1 represented the Windows 10 VM, while VM2 represented the Ubuntu VM.
</p>
<br />
<p>
<img src="https://i.imgur.com/aZ5e3QF.png" height="70%" width="70%"/>
</p>
<p>
In the top right corner, there was a public IP address. I copied this address and used Remote Desktop to establish a connection with VM1 for the lab.
</p>
<br />
<p>
<img src="https://i.imgur.com/qZAaOKM.png" height="70%" width="70%"/>
</p>
<p>
Within VM1, I installed Wireshark using Microsoft Edge. After opening Wireshark and Windows PowerShell, I filtered the traffic to focus on ICMP (ping) packets. To perform the lab tasks, I initiated a continuous ping to VM2 using its private IP address. Each ping request and reply between VM1 and VM2 was captured by Wireshark, providing a clear view of the ICMP traffic.
</p>
<br />
<p>
<img src="https://i.imgur.com/r2sTBjV.png" height="70%" width="70%"/>
</p>
<p>
Once I confirmed the successful ping, I accessed VM2's Network Security Group and added an inbound rule to block ICMP traffic. This configuration change effectively prevented VM2 from receiving ping requests.
</p>
<br />
<p>
<img src="https://i.imgur.com/hqFeBvp.png" height="70%" width="70%"/>
</p>
<p>
As expected, after blocking ICMP traffic, the ping requests from VM1 to VM2 started to time out. Wireshark's "Info" section indicated that while VM1 continued to send requests, there were no responses from VM2, accompanied by a "no response found!" message.
</p>
<br />

<p>
<img src="https://i.imgur.com/jDPXYbg.png" height="70%" width="70%"/>
</p>
<p>
I then went back to VM2's Network Security Group and selected the rule I created. By choosing the "Allow" option under "Action," I restored the ability for VM2 to receive incoming ICMP traffic.
</p>
<br />
<p>
<img src="https://i.imgur.com/rSfEOeL.png" height="70%" width="70%"/>
</p>
<p>
Now that ICMP traffic was allowed again, both the PowerShell terminal and Wireshark showed that VM2 was sending reply packets. To conclude the ICMP observation, I ended the ping activity in PowerShell.
</p>
<br />
<p>
<img src="https://i.imgur.com/0gWpLSn.png" height="70%" width="70%"/>
</p>
<p>
To examine DHCP traffic, I filtered Wireshark for DHCP packets and attempted to assign a new IP address to VM1 using the "ipconfig /renew" command. Although the private IP address remained unchanged, Wireshark captured the DHCP request and acknowledgement, indicating the generation of DHCP traffic.
</p>
<br />
<p>
<img src="https://i.imgur.com/wrRxGbT.png" height="70%" width="70%"/>
</p>
<p>
Next, I filtered Wireshark for DNS traffic and executed the "nslookup" command for google.com. The terminal displayed both IPv4 and IPv6 addresses, while Wireshark's "Info" section revealed the presence of A and AAAA records corresponding to the IP address types.
</p>
<br />
<p>
<img src="https://i.imgur.com/mFVXr7c.png" height="70%" width="70%"/>
</p>
<p>
Lastly, I utilized Wireshark to monitor RDP (Remote Desktop Protocol) traffic. Since RDP traffic was already being generated by my physical computer's connection to the VM, I did not need to rely on the PowerShell terminal. With continuous RDP traffic, Wireshark displayed each packet transmitted in real-time. While I typically filtered protocols by name in Wireshark, I decided to filter RDP traffic using its port number (tcp.port == 3389), ensuring accurate capture of the packets.
</p>
<br />
<p>
And that's a wrap! Capturing and analyzing packets is very interesting!
</p>
<br />
