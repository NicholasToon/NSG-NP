
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

## Network Protocol Breakdown

ICMP (Internet Control Message Protocol)
- No Port #

SSH (Secure Shell)
- Port: 22

DHCP (Dynamic Host Configuration Protocol)
- Port: 67 & 68

DNS (Domain Name System)
- Port: 53

RDP (Remote Desktop Protocol)
- Port: 3389
---
## Execution 

![Image](https://i.imgur.com/TMeKthS.png)

Start **Remote Desktop Connection** and input the public IP address from WindowsVM into the **Computer** field. Click on **Connect**, then choose **More choices**, select **Use a different account**, and enter the credentials you set up for the virtual machine. Click **Yes** when asked to verify; now you're in.

![Image](https://i.imgur.com/CJTTAng.png)

Once inside the Windows VM, I choose to switch the privacy settings off. Next, open Microsoft Edge (ignore prompts) and download and install [Wireshark](https://www.wireshark.org/). The download will be under **Get Acquainted**. When configuring, simply continue to press **Next** and **I Agree**. If it doesn't open automatically, then open it via the Start menu.
