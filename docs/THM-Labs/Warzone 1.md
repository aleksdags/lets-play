You work as a Tier 1 Security Analyst L1 for a Managed Security Service Provider (MSSP). Today you're tasked with monitoring network alerts.

A few minutes into your shift, you get your first network case: **Potentially Bad Traffic** and **Malware Command and Control Activity detected**.  Your race against the clock starts. Inspect the PCAP and retrieve the artifacts to confirm this alert is a true positive. 

**Your tools**:

- [Brim](https://tryhackme.com/room/brim)
- [Network Miner](https://tryhackme.com/room/networkminer)
- [Wireshark](https://tryhackme.com/room/wireshark)

---

Deploy the machine attached to this task; it will be visible in the split-screen view once it is ready.

If you don't see a virtual machine load then click the Show Split View button.

![Split View](https://assets.tryhackme.com/additional/challs/warzone1-split-view.png)  

#### Answer the questions below

What was the alert signature for **Malware Command and Control Activity Detected**?

Use Brim

1. Manually search for *Malware Command and Control Activity*
2. Filter Suricata alerts, alert.category and alert.signature

Ans: ET MALWARE MirrorBlast CnC Activity M3

What is the source IP address? Enter your answer in a **defanged** format. 

Use built query, Suricata Alerts by Source and Destination.
There's only 1 output, get the src ip then defang

Ans: ```172[.]16[.]1[.]102```

What IP address was the destination IP in the alert? Enter your answer in a **defanged** format. 

Same as the previous question

Ans: ```169[.]239[.]128[.]11```

Inspect the IP address in VirsusTotal. Under **Relations > Passive DNS Replication**, which domain has the most detections? Enter your answer in a **defanged** format. 

Ans:  ```fidufagios[.]com```

Still in VirusTotal, under **Community**, what threat group is attributed to this IP address?

Ans: ta505

What is the malware family?

Ans: MirrorBlast

Do a search in VirusTotal for the domain from question 4. What was the majority file type listed under **Communicating Files**?

Ans: Windows Installer

Inspect the web traffic for the flagged IP address; what is the **user-agent** in the traffic?

Brim

Find HTTP then show details

Ans: ``REBOL View 2.7.8.3.1``

Retrace the attack; there were multiple IP addresses associated with this attack. What were two other IP addresses? Enter the IP addressed **defanged** and in numerical order. (**format: IPADDR,IPADDR**)

Filter HTTP requests, sort dest ip addresses

There are 4 other ip addresses, checking their URI, there are two that stand out

Ans: ```185[.]10[.]68[.]235, 192[.]36[.]27[.]92```

What were the file names of the downloaded files? Enter the answer in the order to the IP addresses from the previous question. (**format: file.xyz,file.xyz**)

File for 2nd ip is already in the URI. To find the other one, look at file activity

 Ans: ```filter.msi, 10opd3r_load.msi```

Inspect the traffic for the first downloaded file from the previous question. Two files will be saved to the same directory. What is the full file path of the directory and the name of the two files? (**format: C:\path\file.xyz,C:\path\file.xyz**)

Select log for filter.msi then click packets
This will open wireshark, then follow tcp stream
at the bottom of the page you can find the two files

Ans: `C:\ProgramData\001\arab.bin, C:\ProgramData\001\arab.exe`


Now do the same and inspect the traffic from the second downloaded file. Two files will be saved to the same directory. What is the full file path of the directory and the name of the two files? (format: C:\path\file.xyz,C:\path\file.xyz)

Same as before but this time use the log for 10opd3r_load.msi

Ans: `C:\ProgramData\Local\Google\exemple.rb, C:\ProgramData\Local\Google\rebol-view-278-3-1.exe`