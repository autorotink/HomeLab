## Steps (cont.)
### [Build a Basic Home Lab (1/3)](https://youtu.be/5iafC6vj7kM?si=p7a8ZvUKrfc2BrN9)
#### Introduction
How to propery configure VMs in VMware and VirtualBox for experimenting with malware and testing our tools.  Typically, we can just trust the default settings, but we should know how to Reduce our Risk ourselves!
#### Different Network Options
Focus on Networking!
- Open up VirtualBox (if you're anything like me, it's been a few days since I set up the lab!)
![image](https://github.com/user-attachments/assets/d27e5ad6-0759-4ae2-8303-6c412239cbf4)
- Pick a VM > Select "Settings" > "Network" > Select the drop-down to see the different Network Options:
![image](https://github.com/user-attachments/assets/867dcc8c-4ec5-44cb-b833-7b369061b271)
🐿️ MyDFIR did an excellent job in [explaining the different options](https://youtu.be/5iafC6vj7kM?si=mKXXwhpbDtszP3oP&t=89) (complete with diagrams and everything!) 
 After some wandering around the internet, what is provided in the video is pretty high-level!  The [VirtualBox Documentation](https://www.virtualbox.org/manual/ch06.html) provides more information.   I'll do my best to provide a brief summary!
  -**NAT**: Each VM gets assigned its own network accessed via the host network adapter.    Access to Internet, NAT can access LAN.
  - **NAT Network**: Each VM meshes into a single network.  Access to Internet.
  - **Bridged**: The VMs act as physical machines, they will be on the same network as your host machine. Don't Execute Malware with Bridged!  Access to the Internet and the LAN.
  - **Host-Only Network**: The VMs are only accessible to the host machine.  No access to the Internet nor the LAN.
  - **Internal Network**: Used when performing _malware analysis_, this option puts the VMs into their own network.  You'll have to statically assign each VM an IP. No access to the Internet, nor LAN.
  - **Not Attached**: No connection to anything!
  - There are others, but these are the ones covered in the video.
#### Two Sample Scenarios
**Scenario 1: Test Tools Requiring Internet Connectivity**
 NAT: Default setting are OK! 
**Scenario 2: Analyze Malware and Identify Additional Indicators of Compromise**
- Probably best to select "Not Attached".  This won't work for all malware, but this is a good beginner protocol!
- Or, Internal Network.  This will allow the VMs to communicate with eachother, but not with the host.  
#### Demo: Scenario 1 - Test Tools Requiring Internet Connectivity
- All you should need to do is power on the desired VM.  You can check the Network Adapter by going to Settings > Network > Check "Enable Network Adapter" > Attached to: NAT > OK.
- Download tools that you want to test.
- Take a snapshot (maybe name it something like "Initial Toolkit")
- And start testing out the tools.
#### Demo: Scenario 2 - Analyze Malware and Identify Additional Indicators of Compromise
##### Complete Critical Configuration
- Select the desired VM (for me, it'll be EllieDefends) > Settings > Network > Check "Enable Network Adapter" > Internal Network > name the network something like "BabyShark" > OK
- Select the desired VM (I just renamed my attack maching KaliShark) > Settings > Network > Check "Enable Network Adapter" > Internal Network > select the network named "BabyShark" > OK
_These machines are now in the same network._
##### Statically assign IPs
- Select the **windows** machine.
- Right-click the Globe icon in the bottom right corner > Open Network & Internet Settings > Advanced network settings - Change adapter options.
- Right-click the Ethernet > Properties > Select Internet Protocol Version 4 (TCP/IPv4) > Properties
- Select "Use the following IP address" > Select an IP, and keep track of it! (`192.168.20.10`) (`192.168.40.10`) > Leave everything else as a default > OK
  - Private IP Address Ranges (RFC 1918):
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
  - Double-check your work!
    - Go to the start menu and in the search bar, enter "cmd" > Open the command prompt > enter `ipconfig` > and you should see the address you just assigned!
  - Take a snapshot.
- Select the **kali** machine.
  - Right-click the Ethernet icon in the top right corner > Edit Connections > Double-click on "Wired connection 1"
  - IPv4 Settings > Change the Method from Automatic (DHCP) to Manual
  - Under Addresses > Select Add > Select an IP, and keep track of it! (`192.168.60.11`) > Leave everything else as default > Save
  - Double-check your work!
    - Right click anywhere and select "Open Terminal Here" > enter `ifconfig` > and you should confirm the address you just assigned!
    - Take a snapshot.
Here's an example of what each of the machines should look like during your QC!
![image](https://github.com/user-attachments/assets/4a70bb61-3058-4125-abc1-c8d2be28e2cd)
##### Test the Network!
🐿️ Oh...this is exciting!!
- On KaliShark enter the Windows machine IP: `ping 192.168.20.10`
![image](https://github.com/user-attachments/assets/d0c6fbc9-40b9-4d7f-a883-d5f07550a8e4)
  - That doesn't work (yet) because the Windows Firewall is blocking inbound ICMP traffic.
- On EllieDefends enter Kali machine IP: `ping 192.168.60.11`
![image](https://github.com/user-attachments/assets/62dc184b-b1d4-43c7-b078-20ad54238d45)
  - 🐿️ Ooop!  That does work! (the tutorial has it as being able to work...hmmmm...time to trouble-shoot!
##### Troubleshooting the Network! (1)
🐿️ I'm totally using all my Copiloting skills! lol
Here are some troubleshooting steps to get your Windows VM to ping your Kali Linux VM on an internal network in VirtualBox:
1. Check Network Adapter Settings: (🐿️ I did this first thing, before diving into Copilot steps)
- Ensure both VMs are connected to the same internal network.
- In VirtualBox, go to the settings for each VM, then navigate to Network and make sure the network adapter is set to Internal Network and the network name is the same for both VMs.

2. Disable Windows Firewall (Temporarily):
- On your Windows VM, open Control Panel, go to System and Security, then Windows Defender Firewall.
- Click on Turn Windows Defender Firewall on or off.
- Select Turn off Windows Defender Firewall for both private and public networks. Note: Remember to turn it back on after testing.

3. Disable Linux Firewall (Temporarily):
- Open a terminal on your Kali Linux VM.
- Run the command sudo ufw disable to disable the firewall. Note: Remember to turn it back on after testing using sudo ufw enable.

4. Check IP Configuration: (🐿️ I did this first thing, before diving into Copilot steps)
- On your Windows VM, open Command Prompt and run ipconfig to confirm the assigned IP address.
- On your Kali Linux VM, open a terminal and run ifconfig (or ip a) to check the IP address.

5. Ping Test:
- On your Windows VM, open Command Prompt and run ping <Kali_IP> (replace <Kali_IP> with the IP address of your Kali Linux VM).
- If the ping fails, ensure both VMs have the correct IP configurations and are on the same subnet.
  - I'm pretty sure I did this....  

6. Check Routing Table:
- On your Windows VM, open Command Prompt and run route print to check the routing table. Ensure there is a route to the internal network.
- On your Kali Linux VM, run route -n to check the routing table.
  - Didn't get around to this just yet.
 
##### Troubleshooting the Network! (2)
OK!  So I started up my VMs (it's a new day btw), and my Kali machine wasn't working...so I opened it to a snapshot...and then I couldn't change the network settings.
So I downloaded the extension for VirtualBox, and need to restart my computer...(Because...of course.)   
Still not a whole lotta luck.  I'll keep going when I have some more time...likely going to uninstall and re-install all the things (practice is good!)!

#### VMWare Demo
