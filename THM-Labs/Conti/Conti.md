An Exchange server was compromised with ransomware. Use Splunk to investigate how the attackers compromised the server.

# Task 1: SITREP

Some employees from your company reported that they can’t log into Outlook. The Exchange system admin also reported that he can’t log in to the Exchange Admin Center. After initial triage, they discovered some weird readme files settled on the Exchange server.  

Below is a copy of the ransomware note.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/15db974b80239a7eb2c52fb26c458933.png)  

**Warning**: Do **NOT** attempt to visit and/or interact with any URLs displayed in the ransom note.   

Read the latest on the Conti ransomware [here](https://www.bleepingcomputer.com/news/security/fbi-cisa-and-nsa-warn-of-escalating-conti-ransomware-attacks/). 

---

Connect to OpenVPN or use the AttackBox to access the attached Splunk instance. 

Splunk Interface Credentials:

**Username**: `bellybear`

**Password**: `password!!!`

**Splunk URL**: `http://MACHINE_IP:8000`

Special thanks to [Bohan Zhang](https://www.linkedin.com/in/bohansec?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAACFkYBwB9L43-CozJsTYeFoIV29KBlKU9qc&lipi=urn%3Ali%3Apage%3Ad_flagship3_search_srp_all%3BWgzBOFb8RQWd%2B24UFVSw%2Fw%3D%3D) for this challenge.

Answer the questions below

Start the attached virtual machine.

 Completed

# Task 2: Exchange Server Compromised
Below are the error messages that the Exchange admin and employees see when they try to access anything related to Exchange or Outlook.  

**Exchange Control Panel**:  
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/214468bd3cc7466762b2358993bb3069.png)

**Outlook Web Access**:

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/c7d87fb962d961d81502f02dd1fdba77.png)  

**Task**: You are assigned to investigate this situation. Use Splunk to answer the questions below regarding the Conti ransomware. 

Answer the questions below

Can you identify the location of the ransomware?

```
index="main" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11
```

![](screenshots/Pasted%20image%2020240429220724.png)

Ans: `c:\Users\Administrator\Documents\cmd.exe`

What is the Sysmon event ID for the related file creation event?  

https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon

![](screenshots/Pasted%20image%2020240429212335.png)

Ans: 11

Can you find the MD5 hash of the ransomware?  

```
index="main" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" Image="C:\\Users\\Administrator\\Documents\\cmd.exe" Hashes="*"
```

What file was saved to multiple folder locations?

```
index="main" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11 RuleName=Downloads
```

![](screenshots/Pasted%20image%2020240429213919.png)

Ans: readme.txt

What was the command the attacker used to add a new user to the compromised system?

I decided to filter the parentcommandline to see

```
index="main" source="WinEventLog:Microsoft-Windows-Sysmon/Operational"| rare limit=20 ParentCommandLine
```

Ans: `net user /add securityninja hardToHack123$`

The attacker migrated the process for better persistence. What is the migrated process image (executable), and what is the original process image (executable) when the attacker got on the system?

```
index="main" source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

Hint said to look at Event Code 8, there are only 2 entries here which I assumed when the attacker started and migrated.

![](screenshots/Pasted%20image%2020240430211437.png)

Adding SourceImage in the selecting field shows us the answers.

Ans: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,  C:\Windows\System32\wbem\unsecapp.exe `

The attacker also retrieved the system hashes. What is the process image used for getting the system hashes?  

There are 2 values in the TargetImages field, lsass.exe and unsecapp.exe.

lsass.exe is a windows process is used in security related tasks such as verifying user access, password changes, and creates access tokens. This is also often targeted by attackers because of the sensitive information it holds. [Extra Sauce](https://redcanary.com/threat-detection-report/techniques/lsass-memory/)

On the other hand, unsecapp.exe is used to execute WMI scipts.

![](screenshots/Pasted%20image%2020240430220231.png)

Ans: `C:\Windows\System32\lsass.exe`

What is the web shell the exploit deployed to the system?  

Move from sysmon logs to iis and searched for unfamiliar post requests in the cs_uri_stem.

```
index="main" sourcetype=iis cs_method=POST
```

![](screenshots/Pasted%20image%2020240430222834.png)

Webshell deployed in IIS are in aspx

Ans: `/owa/auth/i3gfPctK1c2x.aspx`

What is the command line that executed this web shell?  

Moved back to sysmon logs and filtered the .aspx extension.

Ans:
```
[attrib.exe -r \\\\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx](http://10.10.175.111:8000/en-US/app/search/search?earliest=0&latest=&q=search%20index%3D%22main%22%20source%3D%22WinEventLog%3AMicrosoft-Windows-Sysmon%2FOperational%22%20.aspx&display.page.search.mode=smart&dispatch.sample_ratio=1&workload_pool=&display.events.fields=%5B%22host%22%2C%22source%22%2C%22sourcetype%22%2C%22cs_uri_stem%22%5D&display.prefs.fieldFilter=&display.general.type=events&display.visualizations.charting.chart=line&display.page.search.tab=events&sid=1714487382.104#)
```

What three CVEs did this exploit leverage?  

Ans: CVE-2020-0796,CVE-2018-13374, CVE-2018-13379