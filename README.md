# Build a Home Lab
_I've worked with virtual machines before in VMWare...particularly with my AntiSyphon training.  I've also used Kali in VMWare for work, with plenty of guided instruction.  
I feel pretty confident with parts of that.  But I wanted to document the steps for building a home lab following YouTuber @MyDFIR.  There are other resources out there, but I like this guy (as well as many others)!  I'm documenting my progress to keep me accountable too!
MyDFIR has a whole Cybersecurity Projects Playlist! (HUZZAH!) [Cybersecurity Projects - YouTube](https://www.youtube.com/playlist?list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ)_

_Also, as I run through the steps, I tend to have a lot of different thoughts running through my brain, you'll see those annotated with a squirrel 🐿️.  Feel free to skip those guys, or read them!_

# HOME LAB

## Objective
To build a functional home lab environment that demonstrates foundational skills. The project, inspired by tutorials from YouTuber @MyDFIR, aims to simulate real-world scenarios, showcase technical proficiency, and enhance my resume with practical experience in setting up and managing a home lab. This project includes configuring a network of virtual machines, deploying essential forensic tools, and documenting processes and findings in a GitHub repository.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Download the hypervisor VirtualBox to host Virtual Machines. 
- Download and install Windows10 and Kali as VMs within VirtualBox.
- Download 7-Zip in order to extract Kali file.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps
### [Build a Basic Home Lab (1/3)](https://www.youtube.com/watch?v=kku0fVfksrk&t=30s)
#### Introduction
A home lab is used as an environment to test new tools and learn new techniques. 
- 🐿️ I have some experience with this from previous work, where there's the production environment (where everything really happens), and the development environment where our engineers would test out new rules and configurations.  It's better to break things in a dev environment, instead of in the production environment (think the [2024 CrowdStrike-related IT outages](https://en.wikipedia.org/wiki/2024_CrowdStrike-related_IT_outages?form=MG0AV3)). 
- 🐿️To test out any update, it's recommended to have comprehensive testing, staged rollouts, rollback mechanisms, monitoring and analytics, and communication and transparency ([Crowd-Striked: Lessons Learned and Best Practices for Future Prevention](https://www.globallogic.com/insights/blogs/crowd-striked-lessons-learned-and-best-practices-for-future-prevention/?form=MG0AV3)).

A homelab is useful for testing out new tools, break things, and use malware in order to see what indicators of compromise (IOC) might look like.  
- 🐿️ John Strand discusses this in his course too! (As I'm sure everyone does!)  How are you supposed to know how to apply a tourniquet, if you don't know what a wound or bleeding looks like?

A safe way to have a dev environment is to do it virtually!  Build a virutal environment, preferably a sandbox environment. 
- 🐿️ I love @NetworkChuck's video on [virtual machines](https://www.youtube.com/watch?v=wX75Z-4MEoM)! I listened to him describe VMs and it was really great!
- 🐿️ I was doing the [Summit Room](https://tryhackme.com/room/summit), and one of the first things we did was to open up our "malware" within a sandbox called PicoSecure's Malware Sandbox from [any.run](https://any.run/plans?form=MG0AV3).  But using a virtual machine also acts as a sandbox because it's isolated from the host machine. 

Vendors include VMware (Broadcom), VirtualBox (Oracle), Microsoft Hyber-V, Windows Sandboxed Environment.  MyDFIR will be using VirtualBox.
- 🐿️ I've used VMWare before...and I also have VirtualBox from some previous experimenting! (HUZZAH!) 

#### Virtual Box Demo
Install from (https://www.virtualbox.org). 
Pick your host machine.
Download!
- 🐿️ I already have VirtualBox installed.  But I checked the website anyways, and found that the most current version is 7.1.6.  I checked my installed version, (Help > About Virtual Box), and it's an older version.  After finding this out, I checked for updates (File > Check For Updates), and lo and behold-there's an update.  So I ran through the Setup Wizard, and got the most updated version of Virtual Box.
- 🐿️ Also, if I had a VM that I was using in the old version of VirtualBox, I wouldn't lose that VM by completeing this update (see forums.virtualbox.org)

- I ran the wizard and came across this warning:

![image](https://github.com/user-attachments/assets/f0c63025-fdd0-4a91-a1e4-1005ec8b6408)

- 🐿️ So...I went to the Virtual Box SDK to see what this was all about!
I consider myself decently OK with computering things...so I found the SDK, and CTRL+F'd my way to python bindings. And I drilled down to the important sentence from paragraph 2.1.2 all about Python:
"The VirtualBox OOWS for Python relies on the Python ZSI SOAP implementation (see http://pywebsvcs.sourceforge.net/zsi.html), which you will need to install locally before trying the examples."
So I went to the recommended page.  Guess what?  I'm not smart...yet...someday tho! 

- 🐿️ Next up: Copilot, HELP!
  Q: "I'm updating my virtualbox, and the wizard says I'm missing dependencies Python core / win32api. I'm reading the SDK manual, how do I set up the python bindings manually (like it says to do in the wizard?)?"
  A: To set up the Python bindings manually for VirtualBox, follow these steps:
  - 1. **Install Python**: Make sure you have Python installed on your system. You can download it from the official Python website. During installation, ensure you check the option to add Python to the PATH.
  - 2. **Open Command Prompt**: Open Command Prompt (or PowerShell) as an administrator.
  - 3. **Install pywin32**: Run the following command to install the `pywin32` package:
   ```
   py -m pip install pywin32
   ```
  - 4. **Update pip**: If there's a new version of pip available, update it by running:
   ```
   python.exe -m pip install --upgrade pip
   ```
  - 5. **Restart your system**: After installing `pywin32`, restart your system to ensure all changes take effect.
 -Once you've completed these steps, the Python bindings should be set up, and you should no longer see the missing dependencies error when using VirtualBox.
- 🐿️ Action: So, now I'm going to go do the things Copilot says...in a second.


MyDFIR is having us look at the SHA256 Checksums to verify that the download file has not been altered.
 - Select the SHA256 Checksums from https://www.virtualbox.org/wiki/Downloads
 - Select your file explorer, and right click on the downloads file and open a terminal.
 - In Powershell, enter: `Get-FileHash VirtualBox`, from there you can press tab to complete the line, and press enter.
 - SHA256 is generated by default.  Copy the hash to your clipboard in Powershell by double-clicking the hash and pressing `Ctrl+c`.
 - Go to the virtualbox.org tab in your browser.
 - Press `Ctrl+f` to open the find box, and `Ctrl+v`to paste the hash from your Powershell that you just copied.
 - It should find the hash.
 - We have now verified that the downloaded file has not been changed from the vendor's website (HUZZAH!).
Time to run the VirtualBox application and see if the dependency issues comes up during the install...wait...NOW is the time to do the thing with Python!

#### Windows Demo
🐿️ I've used images of VMs before for labs, etc. (thank you, SANS and AntiSyphon!).  It'll be cool to see how to do it another way!
You'll need a valid license to install Windows.
##### Windows Installation and Setup
- Go to: [Microsoft Software Download/Windows10](https://www.microsoft.com/en-ca/software-download/windows10) > Create Windows 10 Installation Media > "Download Now".
This download is a Media Creation Tool that will help to generate a Windows ISO image file.
🐿️ An ISO image file is like a virtual copy of a physical disc.  The name comes from the ISO 9660 file system used in CD-ROM media.
- Open the file > "Yes" > "Accept" > "Create installation media (USB flash drive, DVD, or ISO file) for another PC" > "Next"
- Can change the language, architecture and edition. I'm just going to use the default. > "Next"
- Choose which media to use > "ISO file" > "Next" > Save to a prefered file location.  It will start downloading...which will take a minute or two (a great time to get coffee! ☕)
- After the file is downloaded, open VirtualBox > "New" 
  - Name: whatever you prefer
  - Folder: Default
  - ISO Image > select dropdown > "other" > navigate to the ISO image downloaded earlier and double-click
  - Select "Skip Unattended Installation" to install manually install the operating system.
 ![InstallVMWin1](https://github.com/user-attachments/assets/110df5eb-c4e2-4328-8f54-2f79ceda4242)
🐿️ I'm just going to follow what MyDFIR does.  I did a bit of a Q&A with Copilot about Host Machine Capability.
  - Base Memory: 4GB = 4096MB
  - 1 CPU
  - "Next"
  - Virtual Hard Disk, leaving at 50GB > "Next"
  - See the Summary Page!
 ![InstallVMWin2](https://github.com/user-attachments/assets/a1480a34-b6d8-4503-8016-588fd81ef022)
Now that setup is done, this is what the machine should look like!
![InstallVMWin3](https://github.com/user-attachments/assets/d8f02e21-5768-4dbf-a375-1ecfda69402f)
🐿️ All the setup that was just completed should be able to be adjusted by selecting "Settings". (I've had to make some adjustments on a previous VM that I was troubleshooting)

##### Power On! Setting Up Windows in the VM
Select the green "Start" arrow.  You can also right click on the machine name in the left panel and select the same green "Start" arrow (helpful if you have multiple VMs you're managing).
We are now installing the OS in the VM.  
- Setup Options > "Next" > "Install Now" > Select "I don't have a product key" > "Windows 10 Pro" > "Next" > "I accept the license terms" (of COURSE we do!) > "Next" > "Custom: Install Windows only (advanced)" (because of COURSE we are!) > "Next" --Windows 10 is now installing in the background.
- 🐿️ The installation is completed for my Windows machine (I named it "EllieDefends"!) I'm going to go a little on my own and finish setting up this machine before I move onto the Kali machine.  I'm keeping everything for an American keyboard, and set things up for personal use.  I'm going to go ahead and take a snapshot of the machine, as it is, and come back to it whenever we happen to get to that point in the training videos!
#### Kali Demo
##### Kali Installation and Setup
🐿️ I already installed Kali before, but it was for VMWare.  There's a separate image for VirtualBox.
- Go to: [Kali Virtual Machines](https://www.kali.org/get-kali/#kali-virtual-machines) to download the pre-built virtual machine > Select the first VirtualBox option's download arrow (great time to get another coffee! ☕).
  ![image](https://github.com/user-attachments/assets/26604ffd-4cf0-4192-b1f2-554227ad4463)
   - While that's downloading, go to: [7-Zip](https://7-zip.org) to download the 7-Zip application to be able to extract the Kali files.
   - ![image](https://github.com/user-attachments/assets/a69d5435-0442-4e6e-a551-91cca842c03b)
🐿️ I actually completed this step when I downloaded AntiSyphon's VM! (Huzzah!) You can tell that the Kali file requires the 7-Zip application by looking at the file extension (`.7z`). 7-Zip can alos handle `.zip`, `.rar`, and `.tar.gz`.  
- Go to the Downloads folder > Right-Click the zipped Kali-linux file and extract the file with 7-Zip to extract to the directory.  
- 🐿️ For some reason, I didn't get the 7-Zip option, so I had to open 7-Zip > Right Click the Kali file > Hover over 7-Zip > Extract to "kali-linux...PATH"
  ![InstallKali1](https://github.com/user-attachments/assets/3c4be535-8c83-43da-9daf-2997db346b6d)
- If you can't see the file extensions, go to your file explorer, select go to "View" > "Show" > "File name extensions".
![image](https://github.com/user-attachments/assets/072d5524-0c32-45c9-868a-6548193c90b4)
- 🐿️ After the extraction was completed, I remained in 7-Zip and opened the kali-linux file and then double-clicked the kali-linux option the the `vbox` file extension.
- The Kali machine should auto-magically open in VirtualBox!
##### Power On!  Setting up Kali in the VM
Select the green "Start" arrow.  You can also right click on the machine name in the left panel and select the same green "Start" arrow (helpful if you have multiple VMs you're managing).
- The login credentials for the Kali machine is kali/kali.
#### Voila!
🐿️ Here's about what the two machines should look like!  Maybe it's small potatoes to seasoned folks...but this is pretty amazing!
![image](https://github.com/user-attachments/assets/9bd21868-c4c1-45d7-b89d-23e29b684890)

#### Things to be aware of
1. Sandbox Environment: You have to make sure you have your configurations for your VMs done correctly.  Don't download malware from the internet to your VM, because if it isn't properly configured, your "Host is Toast"!  You can craft your own malware in Kali, and attack your own windows machine to see what IOCs may be evident.  You control the malware, so you can have a better understanding of what you should be seeing as well!
2.  Snapshots: Take them before you break the machine!  If you're testing something, you want a machine you can revert to.
  - Way 1: In the VM, go to Machine > Take Snapshot.  Name it something somewhat memorable in case you need to go back to different versions!
![image](https://github.com/user-attachments/assets/cc294346-ddf8-40ad-88ea-497542b0551a)
  - Way 2: In the left panel select the list next to the machine name > Snapshot > a new screen will open > Take
  - Your snapshots will be listed, in case you need to revert back!  You select the snapshot
![image](https://github.com/user-attachments/assets/799b68a7-151b-45d2-8053-6ddc68ac24d3)
3. Don't overspec your machine.  This is where my primer comes in!  Check out my attached file concerning how to check out what a host machine can handle!
[Host Machine Capacity.txt](https://github.com/user-attachments/files/18812592/Host.Machine.Capacity.txt)
### [Build a Basic Home Lab (2/3)](https://www.youtube.com/watch?v=5iafC6vj7kM)
#### Intro
#### How to Reduce Risk
#### Virtual Box Demo
#### Different Network Options
#### Two Sample Scenarios
#### Demo
#### VMWare Demo
### [Build a Basic Home Lab (3/3)](https://www.youtube.com/watch?v=-8X7Ay4YCoA&t=13s)
#### Intro
#### Disclaimer
#### Kali Demo
#### Windows Demo
#### Splunk Demo
