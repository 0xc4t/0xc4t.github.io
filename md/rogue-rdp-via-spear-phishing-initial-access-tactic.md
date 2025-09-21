## Rogue RDP via Spear Phishing: Initial Access Tactic
When I just woke up in the morning today, I immediately made coffee and while scrolling twitter (X) in the hope of getting inspiration or resources shared by people on twitter, about 15–20 minutes I scrolled, I found a post that shared an article from Black Hill InfoSec which discussed rogue rdp, After opening and reading the article I tried to apply it to my active directory infrastructure, where I have 2 domains, namely child (DC02) and parrent (DC01) and one windows server (EMP-Mechine) that joins the child domain (DC02), I tried this attack for Windows Server (EMP-Mechine) for initial access.


### Whats is RDP?
RDP (Remote Desktop Protocol) is a service in computer networks that allows users to access other computers over the network. By default, RDP runs on port 3389 and is commonly found on Windows networks, although in some cases, RDP is also used on Linux-although quite rarely.

### What is Rogue RDP?
Rogue RDP is a technique where an attacker creates an .rdp file with a configuration that allows (or forces) the client to share disks, such as C:// and D://. This allows the attacker to steal files from the client’s disk as well as embed malicious applications by uploading them to the Startup directory, so that they run automatically when the system starts up.

You can create this .rdp file using the built-in Remote Desktop Connection software from Windows, you can follow these steps to create a rogue rdp.

---
![1](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*aZ-CBdCFd5w-qDJsngP6kQ.png)
Click the show options section


---
![2](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*cmiOsI7XQG8CFHWvf1hKfw.png)
Navigate to the IP that will be the rogue RDP and checklist the Allow me to save credentials section.


---
![3](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*DTnSYUykUl08Ww7NSkT4Vg.png)
Select the Advanced Menu then change “if server authentucation fails:” to be as shown in this image


---
![4](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*jMXUcHFQDfJ-b6Vkw9KYNA.png)
Select “Load Resource” and then select “More”


---
![5](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*Ms9sQhsHf75G421Yuz1k_w.png)
Select drives and checklist all


---
![6](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*vacQSzKdGMFS14Kv_ynkUg.png)
Save the configuration file and you have created a rogue rdp file ready to be used for spear phishing.


To make the scheme more realistic, I have tried sending the .rdp file to the target email “EMP-Mechine” at kimmy.kim@mail.wtbank.local.


---
![7](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*IQWeoLCFCq4DbD2-o_fDIA.png)
Then when kimmy downloaded the rdp file and then connected to the RDP, he unknowingly shared his C drive access.


---
![8](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*1d7SLzwo8GHSHbTdANwN2w.png)
Connect


---
![9](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*JjKSF58khwuosYNb3P21pQ.png)
You can see here that the box account can access the C drive of the user kimmy and not only access the box account (rdp host) can also upload malware and place it in the startup application.


---
![10](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*IXZ3E8Bu_RLUzygdW8xudw.png)
Quite simple but dangerous, maybe this is all the discussion about rogue rdp, keep in mind that the article I made only focuses on practice rather than theory, because you can get the theory in articles on the internet, thank you for reading until the end.

### Reference
- [https://www.blackhillsinfosec.com/rogue-rdp-revisiting-initial-access-methods/](https://www.blackhillsinfosec.com/rogue-rdp-revisiting-initial-access-methods/)
- [https://www.trendmicro.com/en_us/research/24/l/earth-koshchei.html](https://www.trendmicro.com/en_us/research/24/l/earth-koshchei.html)
- [https://socprime.com/blog/rogue-rdp-attack-detection-uac-0215/](https://socprime.com/blog/rogue-rdp-attack-detection-uac-0215/)
