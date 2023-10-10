
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

![Image](https://i.imgur.com/LkfzHWw.png)

Double click **Ethernet**. You will now see the live traffic that is being transmitted throughout the virtual machine. Currently it is spamming traffic, which does not help us when it comes to troubleshooting and observation. We are first going to filter through ICMP (Internet Control Message Protocol), the protocol that Ping uses, Ping is used to test connectivity between devices and computers on a network. The traffic will appear to have stopped only becuase we are filtering a protocol that will need to be manually activated. We cannot ping **LinuxVM02** (our target) without knowing the ip address.

![Image](https://i.imgur.com/WRdmqJ7.png)

Return to the Azure portal to copy the **Private Ip address** of the Linux virtual machine. Return to WindowsVM.

![Image](https://i.imgur.com/MI5jSQq.png)

Open up **Windows PowerShell** from the Start menu. Enter **ping 10.0.0.5** (your private IP address may be different) and observe the requests and replies. We will then ping a website like www.google.com. Enter **ping www.google.com -4**. The "-4" indicates that we will receive the traffic in IPv4. You can expand the bars under the packet information for further insight into what gets sent through a simple command such as ping.

Afterwards, we will use a perpetual ping to the LinuxVM02 so we can manipulate the network security groups within LinuxVM02. The command to execute the perpetual ping is **ping 10.0.0.5 -t** (once again, your private IP address will probably be different).

![Image](https://i.imgur.com/gl6UB98.png)

Back at the Azure portal, we will block ICMP traffic through LinuzVM02's firewall, which will cancel out the perpetual ping we initiated on WindowsVM01. Click on **Network Settings** within the **Networking** tab, then choose **Create port rule**, followed by **Inbound port rule**. Since ICMP does not have a port number, you can either select ICMP or enter an asterisk in the **Destination port ranges**. Switch the action to **Deny** and adjust the priority and descriptors to your preference.Changing the priority isn't necessary for this specific exercise, but it may not always be the case; the higher the number, the higher the priority.

![Image](https://i.imgur.com/AGLoYxC.png)

On WindowsVM01, we will observe that the ICMP ping requests are timing out. The port rule is in effect and functioning correctly. When we change the rule back to **allow**, traffic will resume without timeouts.

![Image](

Now, let's move on to SSH traffic. We will log into LinuxVM02 using the PowerShell SSH command. To filter the traffic, use either **ssh** or **tcp.port == 22**. In PowerShell, type the following command: **ssh Labuser@10.0.0.5**, press Enter, and then type **yes** and press enter. Enter the password you used during VM creation. You won't be able to see your password as you type it, but have faith that it is being entered. Remember that the username and password are extremely case-sensitive, so ensure they match exactly as entered when the VM was generated. Once successful, you can manipulate Linux through PowerShell if desired, all while observing the traffic of your manipulation with Wireshark. To exit the Ubuntu server, simply type **exit** and press Enter.
