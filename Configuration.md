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
- NAT: Default setting are OK! 
**Scenario 2: Analyze Malware and Identify Additional Indicators of Compromise**
- Probably best to select "Not Attached".  This won't work for all malware, but this is a good beginner protocol!
- Or, Internal Network.  This will allow the VMs to communicate with eachother, but not with the host.  
#### Demo: Scenario 1 - Test Tools Requiring Internet Connectivity
- All you should need to do is power on the desired VM.  You can check the Network Adapter by going to Settings > Network > Check "Enable Network Adapter" > Attached to: NAT > OK.
- Download tools that you want to test.
- Take a snapshot (maybe name it something like "Initial Toolkit")
- And start testing out the tools.
#### Demo: Scenario 2 - Analyze Malware and Identify Additional Indicators of Compromise
Here's where you'll need to complete some **configurations** to stay safe.
- Select the desired VM (for me, it'll be EllieDefends) > Settings > Network > Check "Enable Network Adapter" > Internal Network > name the network something like "BabyShark" > OK
- Select the desired VM (I just renamed my attack maching KaliShark) > Settings > Network > Check "Enable Network Adapter" > Internal Network > select the network named "BabyShark" > OK
_These machines are now in the same network._
Now, **statically assign IPs** to each machine so they can start talking to eachother.
- Select the **windows** machine.
- Right-click the Globe icon in the bottom right corner > Open Network & Internet Settings > Advanced network settings - Change adapter options.
- Right-click the Ethernet > Properties > Select Internet Protocol Version 4 (TCP/IPv4) > Properties
- Select "Use the following IP address" > Select an IP, and keep track of it! > Everything else as a default > OK
  - Private IP Address Ranges (RFC 1918):
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
  - Double-check your work!
    - Go to the start menu and in the search bar, enter "cmd" > Open the command prompt > `ipconfig` > and you should see the address you just assigned!
  - Take a snapshot.
  - Select the **kali** machine.
  - 
#### VMWare Demo
