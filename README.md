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

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps
### [Build a Basic Home Lab (1/3)](https://www.youtube.com/watch?v=kku0fVfksrk&list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ&index=1&t=18s)
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
#### Kali Demo
#### Things to be aware of

