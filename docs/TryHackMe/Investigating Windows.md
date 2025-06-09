Whats the version and year of the windows machine?  
Settings > System > About
systeminfo

Windows Server 2016

Which user logged in last?  
Event Viewer > Windows Logs > Security
Filter events, 4624 (Logon)
Sort by Date and Time

EC2AMAZ-I8UHO76$, current User, look for next one
Administrator

When did John log onto the system last?

Answer format: MM/DD/YYYY H:MM:SS AM/PM

net user -> lists all users
net user John

Get-LocalUser | Select Name, Lastlogon -> lists all last login time of all accounts

03/02/2019 5:48:32 PM

What IP does the system connect to when it first starts?  
10.34.2.3 -> cmd popup on startup

Regedit > HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Run

What two accounts had administrative privileges (other than the Administrator user)?

Answer format: username1, username2
Get-LocalUser | Select Name
Computer Management -> System Tools > Local Users and Groups > Users
Check user properties > Member of

Jenny, Guest

Whats the name of the scheduled task that is malicous.  
Task Scheduler > Task Scheduler Library

Clean file system -> Sussy as it does not actually clean anything but runs a PS script

nc.ps1 -> nc is a netcat listener

GameOver and falshupdate22 also looks sussy

What file was the task trying to run daily?  
Clean file system > Actions
nc.ps1

What port did this file listen locally for?  
1348

When did Jenny last logon?  
Net user Jenny
Never

At what date did the compromise take place?

Answer format: MM/DD/YYYY


During the compromise, at what time did Windows first assign special privileges to a new logon?

Answer format: MM/DD/YYYY HH:MM:SS AM/PM
4672 -> special logon

What tool was used to get Windows passwords?  
Look at GameOver > Actions
Event runs mim.exe -> this is mimikatz

What was the attackers external control and command servers IP?  
`C:\Windows\System32\drivers\etc\hosts`
127.0.01 is local

What was the extension name of the shell uploaded via the servers website?  
.jsp

What was the last port the attacker opened?  
Firewall > Inbound > First entry
1337

Check for DNS poisoning, what site was targeted?
google.com