You work as a Tier 1 Security Analyst L1 for a Managed Security Service Provider (MSSP). Again, you're tasked with monitoring network alerts.

An alert triggered: **Misc activity**, **A Network Trojan Was Detected**, and **Potential Corporate Privacy Violation**. 

The case was assigned to you. Inspect the PCAP and retrieve the artifacts to confirm this alert is a true positive. 

Your tools:

- [Brim](https://tryhackme.com/room/brim)
- [Network Miner](https://tryhackme.com/room/networkminer)
- [Wireshark](https://tryhackme.com/room/wireshark)

---

<!--Deploy the machine attached to this task; it will be visible in the split-screen view once it is ready.

If you don't see a virtual machine load then click the Show Split View button.

![Show Split Screen if needed](https://assets.tryhackme.com/additional/challs/warzone2-split-view.png)  -->

#### Answer the questions below

What was the alert signature for **A Network Trojan was Detected**?

filter out suricata logs
alert.signature and alert.category

Answer: ET MALWARE Likely Evil EXE download from MSXMLHTTP non-exe extension M2

What was the alert signature for **Potential Corporate Privacy Violation**?

same as previous question
Answer: ET POLICY PE EXE or DLL Windows file download HTTP

What was the IP to trigger either alert? Enter your answer in a **defanged** format. 

Filter suricata logs by source and destination

Answer: `185[.]118[.]164[.]8`

Provide the full URI for the malicious downloaded file. In your answer, **defang** the URI. 

Filter http requests with the ip found, the first result gives us the full URI

Answer: awh93dhkylps5ulnq-be[.]com/czwih/fxla[.]php?l=gap1[.]cab

What is the name of the payload within the cab file? 

Grab the file hash then upload to virustotal

Answer: draw.dll

![](../screenshots/Warzone%202/warzone2_screenshot_001.png)

What is the user-agent associated with this network traffic?

Answer: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 10.0; WOW64; Trident/8.0; .NET4.0C; .NET4.0E)

What other domains do you see in the network traffic that are labelled as malicious by VirusTotal? Enter the domains **defanged** and in alphabetical order. (**format: domain[.]zzz,domain[.]zzz**)

Answer: a-zcorner[.]com, knockoutlights[.]com

There are IP addresses flagged as **Not Suspicious Traffic**. What are the IP addresses? Enter your answer in numerical order and **defanged**. (format: IPADDR,IPADDR)

Answer: 64[.]225[.]65[.]166, 142[.]93[.]211[.]176

For the first IP address flagged as Not Suspicious Traffic. According to VirusTotal, there are several domains associated with this one IP address that was flagged as malicious. What were the domains you spotted in the network traffic associated with this IP address? Enter your answer in a **defanged** format. Enter your answer in alphabetical order, in a defanged format. (**format: domain[.]zzz,domain[.]zzz,etc**)  

Answer: safebanktest[.]top, tocsicambar[.]xyz, ulcertification[.]xyz

Now for the second IP marked as Not Suspicious Traffic. What was the domain you spotted in the network traffic associated with this IP address? Enter your answer in a **defanged** format. (format: domain[.]zzz)  

The first detection is under serveeazy.com though other entries are not detected, might be false positive and is not the answer.

Answer: 2partscow[.]top