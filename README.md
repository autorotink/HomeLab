# Build a Home Lab
_I've worked with virtual machines before in VMWare...particularly with my AntiSyphon training.  I've also used Kali in VMWare for work, with plenty of guided instruction.  
I feel pretty confident with parts of that.  But I wanted to document the steps for building a home lab following YouTuber @MyDFIR.  There are other resources out there, but I like this guy (as well as many others)!  I'm documenting my progress to keep me accountable too!
MyDFIR has a whole Cybersecurity Projects Playlist! (HUZZAH!) [Cybersecurity Projects - YouTube](https://www.youtube.com/playlist?list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ)_

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

 
#### Windows Demo
#### Kali Demo
#### Things to be aware of

