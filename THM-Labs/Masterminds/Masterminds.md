8/6/2023

![](https://i.ibb.co/T1tPZdn/Depositphotos-174220344-s-2019.jpg)  

Three machines in the Finance department at Pfeffer PLC were compromised. We suspect the initial source of the compromise happened through a phishing attempt and by an infected USB drive. The Incident Response team managed to pull the network traffic logs from the endpoints. Use Brim to investigate the network traffic for any indicators of an attack and determine who stands behind the attacks. 

**NOTE: DO NOT** directly interact with any domains and IP addresses in this challenge. 

---

Deploy the machine attached to this task; it will be visible in the **split-screen** view once it is ready.

If you don't see a virtual machine load then click the **Show Split View** button.

![](https://assets.tryhackme.com/additional/brimchallenge/brim-split-view.png)

## Infection 1
Start by loading the Infection1 packet capture in Brim to investigate the compromise event for the first machine. All the PCAPs can be found here: `/home/ubuntu/Desktop/PCAPs`  

**Note**: For questions that require multiple answers, please separate the answers with a comma.

Answer the questions below

Provide the victim's IP address.

Ans: 192.168.75.249

The victim attempted to make HTTP connections to two suspicious domains with the status '404 Not Found'. Provide the hosts/domains requested. 

`_path=="http" | 192.168.75.249 | status_code == 404`

filter http with the victim ip and http status code for Not Found

Ans: cambiasuhistoria.growlab.es, www.letscompareonline.com

The victim made a successful HTTP connection to one of the domains and received the response_body_len of 1,309 (uncompressed content size of the data transferred from the server). Provide the domain and the destination IP address.

`_path=="http" | 192.168.75.249 | response_body_len == 1309`

Ans: ww25.gocphongthe.com, 199.59.242.153

How many unique DNS requests were made to cab[.]myfkn[.]com domain (including the capitalized domain)? 

Ans: 7

Provide the URI of the domain bhaktivrind[.]com that the victim reached out over HTTP. 

Ans: /cgi-bin/JBbb8/

Provide the IP address of the malicious server and the executable that the victim downloaded from the server. 

Ans: 185.239.243.112, catzx.exe

Based on the information gathered from the second question, provide the name of the malware using [VirusTotal](https://www.virustotal.com/gui/home/upload). 

Answer is not in VirusTotal, both links have no comments in the community tab. I had to go look for it in the internet

https://bazaar.abuse.ch/sample/a2d525c9bd8128160c64990fa84afc4da2bea8a72cfb4ca42f14cddac1343df2/

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807002025.png]]
## Infection 2
Please, navigate to the Infection2 packet capture in Brim to investigate the compromise event for the second machine.

Note: For questions that require multiple answers, please separate the answers with a comma.  

Answer the questions below

Provide the IP address of the victim machine. 

Use defined queries, Unique Network Connections

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807191956.png]]

In this image we can see that there's a lot of traffic coming from 192.168.75.146

Ans: 192.168.75.146

Provide the IP address the victim made the POST connections to. 

Using queries again, HTTP Post Requests, all of the POST requests are pointing to 5.181.156.252

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807192123.png]]

Ans: 5.181.156.252

How many POST connections were made to the IP address in the previous question?

Refer to the previous image

Ans: 3

Provide the domain where the binary was downloaded from. 

Since we are looking for downloads, we can filter our GET

`method=="GET" id.orig_h==192.168.75.146`

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807193907.png]]

Ans: hypercustom.top

Provide the name of the binary including the full URI.

Ans: /jollion/apines.exe

Provide the IP address of the domain that hosts the binary.

Ans: 45.95.203.28

There were 2 Suricata "A Network Trojan was detected" alerts. What were the source and destination IP addresses? 

User filter, Suricata Alerts by Category

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807194022.png]]

Ans: 192.168.75.146, 45.95.203.28

Taking a look at .top domain in HTTP requests, provide the name of the stealer (Trojan that gathers information from a system) involved in this packet capture using [URLhaus Database](https://urlhaus.abuse.ch/). 

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807194142.png]]

Ans: Redline Stealer
## Infection 3
Please, load the Infection3 packet capture in Brim to investigate the compromise event for the third machine.  

Note: For questions that require multiple answers, please separate the answers with a comma.  

Answer the questions below

Provide the IP address of the victim machine.

Filter out uniq connections, there are two ips that stand out

192.168.75.232 and 192.168.75.133, comparing the two ips, there are more connections on the former ip.

Ans: 192.168.75.232

Provide three C2 domains from which the binaries were downloaded (starting from the earliest to the latest in the timestamp)

Filter out GET method with the victim IP, then sort the timestamps or scroll to the end.

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807204242.png]]

Ans: efhoahegue.ru, afhoahegue.ru, xfhoahegue.ru

Provide the IP addresses for all three domains in the previous question.

Ans: 162.217.98.146, 199.21.76.77, 63.251.106.25

How many unique DNS queries were made to the domain associated from the first IP address from the previous answer? 

Use the Unique DNS Queries

`_path=="dns" | count() by query | query=="efhoahegue.ru"`

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807210322.png]]

Ans: 2

How many binaries were downloaded from the above domain in total? 

Filter HTTP GET requests using the first domain IP, looking at the uri, only 5 entries have a file.

`_path=="http" | method=="GET" | 162.217.98.146`

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807210432.png]]

Ans: 5

Provided the user-agent listed to download the binaries. 

From the same query as the previous question

Ans: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:25.0) Gecko/20100101 Firefox/25.0

Provide the amount of DNS connections made in total for this packet capture.

Count all the dns requests

`_path=="dns" | count()`

![[THM-Labs/Masterminds/screenshots/Pasted image 20230807210739.png]]

Ans: 986

With some OSINT skills, provide the name of the worm using the first domain you have managed to collect from Question 2. (Please use quotation marks for Google searches, don't use .ru in your search, and DO NOT interact with the domain directly).

Ans: phorphiex