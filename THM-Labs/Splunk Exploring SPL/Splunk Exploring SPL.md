# Task 1  Introduction

Splunk is a powerful SIEM solution that provides the ability to search and explore machine data. **Search Processing Language (SPL)** is used to make the search more effective. It comprises various functions and commands used together to form complex yet effective search queries to get optimized results.

This room will dive deep into some key fundamentals of searching capability, like chaining SPL queries to construct simple to complex queries.  

Learning Objectives

This room will teach the following topics:

- What are Search processing Language?  
- How to apply filters to narrow down results.
- Using transformational commands.
- Changing the order of the results.

Room Prerequisites

- This room is based on the SIEM concepts covered in [Intro to SIEM](https://tryhackme.com/room/introtosiem) and [Splunk: Basics](https://tryhackme.com/jr/splunk101) rooms. Complete these rooms and continue to the next task.

# Task 2  Connect with the Lab

Room Machine

Before moving forward, deploy the machine. You can access this lab in the AttackBox or click MACHINE_IP to start the lab in your browser when the machine is fully started. The machine will take up to 3-5 minutes to start.

**Note:** For this room, we will work on the index `Windowslogs`.

What is the name of the host in the Data Summary tab?
![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231120230547.png]]

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231120230531.png]]

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231120224746.png]]
Ans:  cyber-host
# Task 3  Search & Reporting App Overview
**Search & Reporting App** is the default interface used to search and analyze the data on the Splunk Home page. It has various functionalities that assist analysts in improving the search experience.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/68395fd11ab6b3a4ebb5d4a4a77c8b36.png)  

Some important functionalities present in the search App are explained below:

**1) Search Head:**
Search Head is where we use search processing language queries to look for the data.  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/b162472e5f02c1a9ec6b92ee052ff415.png)


**2) Time Duration:**
This tab option provides multiple options to select the time duration for the search. **All-time** will display the events in real-time. Similarly, the **last 60 minutes** will display all the events captured in the last hour.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/d759303fcea6ffaf5b48bea06abb383f.png)

**3) Search History:**
This tab saves the search queries that the user has run in the past along with the time when it was run. It lets the user click on the past searches and look at the result. The filter option is used to search for the particular query based on the term.  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/3b9087b12c5dd849dae2e61cdd7ba93a.png)

**4) Data Summary:**
This tab provides a summary of the data type, the data source, and the hosts that generated the events as shown below. This tab is very important feature used to get a brief idea about the network visibility.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/81459123900e070704fe4fdac9f9b33e.gif)  

**5) Field Sidebar:**
The Field Sidebar can be found on the left panel of Splunk search. This sidebar has two sections showing selected fields and interesting fields. It also provides quick results, such as top values and raw values against each field.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/78cb1326f4351a7127062f2bc96273dc.png)  

Some important points to understand about the sidebar are explained below:

|   |   |
|---|---|
|**1- Selected Fields**|Splunk extracts the default fields like source, sourcetype, and host, which appear in each event, and places them under the selected fields column. We can select other fields that seem essential and add them to the list.|
|**2- Interesting Fields**|Pulls all the interesting fields it finds and displays them in the left panel to further explore.|
|**3- Alpha-numeric fields 'α'**|This alpha symbol shows that the field contains text values.|
|**4- Numeric fields '#'**|This symbol shows that this field contains numerical values.|
|**5- Count**|The number against each field shows the number of events captured in that timeframe.|

Answer the questions below

In the search History, what is the 7th search query in the list? (excluding your searches from today)
![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231120230823.png]]
Ans: ```index=windowslogs | chart count(EventCode) by Image```

In the left field panel, which Source IP has recorded max events?  

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231120235834.png]]

Ans: 172.90.12.11

How many events are returned when we apply the time filter to display events on 04/15/2022 and Time from 08:05 AM to 08:06 AM?

Use the Date time range and change the parameter to between. Set the given date and time. 
![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121000113.png]]

Ans: 134
# Task 4  Splunk Processing Language Overview

Splunk Search Processing Language comprises of multiple functions, operators and commands that are used together to form a simple to complex search and get the desired results from the ingested logs. Main components of SPL are explained below:

﻿**Search Field Operators**  

Splunk field operators are the building blocks used to construct any search query. These field operators are used to filter, remove, and narrow down the search result based on the given criteria. Common field operators are Comparison operators, wildcards, and boolean operators.  

Comparison Operators

﻿These operators are used to compare the values against the fields. Some common comparisons operators are mentioned below:  

|   |   |   |   |
|---|---|---|---|
|**Field Name  <br>**|**Operator  <br>**|**Example**|**Explanation  <br>**|
|**Equal  <br>**|=|UserName=Mark|This operator is used to match values against the field. In this example, it will look for all the events, where the value of the field UserName is equal to Mark.|
|**Not Equal to  <br>**|!=|UserName!=Mark|This operator returns all the events where the UserName value does not match Mark.|
|**Less than  <br>**|<|Age < 10|Showing all the events with the value of Age less than 10.|
|**Less than or Equal to  <br>**|<=|Age <= 10|Showing all the events with the value of Age less than or equal to 10.|
|**Greater than  <br>**|>|Outbound_traffic > 50 MB|This will return all the events where the Outbound traffic value is over 50 MB.|
|**Greater Than or Equal to  <br>**|>=|Outbound_traffic >= 50 MB|This will return all the events where the Outbound traffic value is greater or equal to 50 MB.|

﻿﻿Lets use the comparison operator to display all the event logs from the index "windowslogs", where AccountName is not Equal to "System"

**Search Query:** `index=windowslogs AccountName !=SYSTEM`  

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121205846.png]]

Boolean Operators

Splunk supports the following Boolean operators, which can be very handy in searching/filtering and narrowing down results.  

|   |   |   |
|---|---|---|
|**Operator  <br>**|**Syntax  <br>**|**Explanation  <br>**|
|**NOT  <br>**|field_A **NOT** value|Ignore the events from the result where field_A contain the specified value.|
|**OR  <br>**|field_A=value1 **OR** field_A=value2|Return all the events in which field_A contains either value1 or value2.|
|**AND  <br>**|field_A=value1 **AND** field_B=value2|Return all the events in which field_A contains value1 and field_B contains value2.|

﻿To understand how boolean operator works in SPL, lets add the condition to show the events from the James account.

**Search Query:** `index=windowslogs AccountName !=SYSTEM **AND** AccountName=James`  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/42c8963dccbd05128f52665c38877f47.png)  

  

Wild Card

Splunk supports wildcards to match the characters in the strings.  

|   |   |   |
|---|---|---|
|**Wildcard symbol**|**Example**|**Explanation**|
|***  <br>**|status=fail*|It will return all the results with values like<br><br>status=failed<br><br>status=failure|

In the events, there are multiple DestinationIPs reported. Let's use the wildcard only to show the **DestinationIP** starting from 172.*

**Search Query:** `index=windowslogs DestinationIp=172.*`  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5530cae0739755e6a682641f5057b1a5.png)  

Answer the questions below

How many Events are returned when searching for Event ID 1 **AND** User as *James*?

``` SPL
index=windowslogs EventID=1 AND User=*James*
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121212937.png]]

Ans: 4

How many events are observed with Destination IP 172.18.39.6 AND destination Port 135?  

```SPL
index=windowslogs DestinationIp=172.18.39.6 AND DestinationPort="135"
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121213142.png]]

Ans: 4

What is the Source IP with highest count returned with this Search query?  
Search Query: index=windowslogs  Hostname="Salena.Adam" DestinationIp="172.18.38.5"  

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121213315.png]]

Ans: 172.90.12.11

In the index windowslogs, search for all the events that contain the term **cyber** how many events returned?

```SPL
index=windowslogs cyber
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121213417.png]]

Ans: 0

Now search for the term **cyber\***, how many events are returned?

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121213501.png]]

Ans: 12256

# Task 5  Filtering the Results in SPL
Our network generates thousands of logs each minute, all ingesting into our SIEM solution. It becomes a daunting task to search for any anomaly without using filters. SPL allows us to use **Filters** to narrow down the result and only show the important events that we are interested in. We can add or remove certain data from the result using filters. The following commands are useful in applying filters to the search results.

**Fields**

|   |   |
|---|---|
|**Command**|**fields**|
|**Explanation  <br>**|Fields command is used to add or remove mentioned fields from the search results. To remove the field, minus sign ( - ) is used before the fieldname and plus ( + ) is used before the fields which we want to display.|
|**Syntax  <br>**|\| fields <field_name1>  <field_name2>|
|**Example**|\| `fields + HostName - EventID`|

Let's use the fields command to only display host, User, and SourceIP fields using the following syntax.  

**Search Query:** `index=windowslogs | fields + host + User + SourceIp`  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/dd77983b5fccf5eacfa73aacbeb7a314.png)  

**Note:** Click on the **More field** to display the fields if some fields are not visible.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/30b2c651a041bd1c2db77b661dc8cc8e.png)  

**Search**

|   |   |
|---|---|
|**Command**|**search**|
|**Explanation  <br>**|This command is used to search for the raw text while using the chaining command `\|`|
|**Syntax  <br>**|\| search  <search_keyword>|
|**Example**|\| `search "Powershell"`|

Use the search command to show all the events containing the term Powershell. This will return all the events that contain the term "**Powershell**".  

**Search Query:** `index=windowslogs | search Powershell`  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/02d2ece97e7f977e32b4c11fd86e41eb.png)  

**Dedup**

|   |   |
|---|---|
|**Command**|**dedup**|
|**Explanation  <br>**|Dedup is the command used to remove duplicate fields from the search results. We often get the results with various fields getting the same results. These commands remove the duplicates to show the unique values.|
|**Syntax  <br>**|\| dedup <fieldname>|
|**Example**|\| `dedup EventID`|

We can use the dedup command to show the list of unique **EventIDs** from a particular hostname.

**Search Query:** `index=windowslogs | table EventID User Image Hostname | dedup EventID`

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/47bb3fb904c84acdbe7cb89dda535ac1.png)  

**Rename**

|   |   |
|---|---|
|**Command**|**rename**|
|**Explanation  <br>**|It allows us to change the name of the field in the search results. It is useful in a scenario when the field name is generic or log, or it needs to be updated in the output.|
|**Syntax  <br>**|\| rename  <fieldname>|
|**Example**|\| `rename User as Employees`|

Let's rename the User field to Employees using the following search query.

**Search Query**:`index=windowslogs | fields + host + User + SourceIp | rename User as Employees`  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/61e56df15649aa12b4be7c91d8cc91ce.png)  

Answer the questions below

What is the third EventID returned against this search query?

Search Query: `index=windowslogs | table _time EventID Hostname SourceName | reverse` 

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121215439.png]]

Ans: 4103

Use the dedup command against the Hostname field before the reverse command in the query mentioned in Question 1. What is the first username returned in the Hostname field?

```SPL
index=windowslogs | table _time EventID Hostname SourceName | dedup Hostname 
| reverse
```
![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121215601.png]]

Ans: Salena.Adam

# Task 6  SPL - Structuring the Search Results
SPL provides various commands to bring structure or order to the search results. These sorting commands like `head`, `tail`, and `sort` can be very useful during logs investigation. These ordering commands are explained below:

Table  

|   |   |
|---|---|
|**Explanation  <br>**|Each event has multiple fields, and not every field is important to display. The Table command allows us to create a table with selective fields as columns.|
|**Syntax  <br>**|\| table <field_name1> <fieldname_2>|
|**Example**|\| `table`  <br><br>\| `head 20` # will return the top 20 events from the result list.|

This search query will create a table with three columns selected and ignore all the remaining columns from the display.  

**Search Query:** `index=windowslogs | table EventID Hostname SourceName`

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/643f5cc127276645eb0fd7fdb339cb29.png)

  

Head

|   |   |
|---|---|
|**Explanation  <br>**|The **head** command returns the first 10 events if no number is specified.|
|**Syntax  <br>**|\| head <number>|
|**Example**|\| `head`   # will return the top 10 events from the result list<br><br>\| `head 20`    # will return the top 20 events from the result list|

The following search query will show the table containing the mentioned fields and display only the top 5 entries.  

**Search Query:** `index=windowslogs |  table _time EventID Hostname SourceName | **head 5**`

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/3a9df7382d1f5e2e3302750dc5016809.png)

Tail

|   |   |
|---|---|
|**Explanation  <br>**|The **Tail** command returns the last 10 events if no number is specified.|
|**Syntax  <br>**|\| tail <number>|
|**Example**|\| `tail` # will return the last 10 events from the result list<br><br>\| `tail 20`   # will return the last 20 events from the result list|

The following search query will show the table containing the mentioned fields and display only 5 entries from the bottom of the list.

**Search Query:** `index=windowslogs |  table _time EventID Hostname SourceName | tail 5`

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/917811648cc95bd3f34c820a473b9c1b.png)

sort  

|   |   |
|---|---|
|**Explanation  <br>**|The **Sort** command allows us to order the fields in ascending or descending order.|
|**Syntax  <br>**|\| `sort` <field_name>|
|**Example**|\| `sort Hostname` # This will sort the result in Ascending order.|

The following search query will sort the results based on the Hostname field.

**Search Query:** `index=windowslogs |  table _time EventID Hostname SourceName | sort Hostname`

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/9589d101b5a07f69fc4771a6c1e54e14.png)  

  

Reverse

|   |   |
|---|---|
|**Explanation  <br>**|The reverse command simply reverses the order of the events.|
|**Syntax  <br>**|\|  reverse|
|**Example**|`<Search Query> \| reverse`|

**Search Query:** `index=windowslogs | table _time EventID Hostname SourceName | reverse`

  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/0de4064ec74ee5f64a399e0a7bed9200.png)  

Answer the questions below

Using the Reverse command with the search query index=windowslogs | table _time EventID Hostname SourceName - what is the HostName that comes on top?

```SPL
index=windowslogs | table _time EventID Hostname SourceName 
| reverse
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121220531.png]]

Ans: James.browne

What is the last EventID returned when the query in question 1 is updated with the **tail** command?

```SPL
index=windowslogs | table _time EventID Hostname SourceName 
|  tail
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121220502.png]]

Ans: 4103

Sort the above query against the SourceName. What is the top SourceName returned?

```SPL
index=windowslogs | table _time EventID Hostname SourceName 
| sort SourceName
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231121220715.png]]

Ans: Microsoft-Windows-Directory-Services-SAM

# Task 7  Transformational Commands in SPL

Transformational commands are those commands that change the result into a data structure from the field-value pairs. These commands simply transform specific values for each event into numerical values which can easily be utilized for statistical purposes or turn the results into visualizations. Searches that use these transforming commands are called transforming searches. Some of the most used transforming commands are explained below.

General Transformational Commands  

Top  

|   |   |
|---|---|
|**Command**|**top**|
|**Explanation  <br>**|This command returns frequent values for the top 10 events.|
|**Syntax  <br>**|\| top  <field_name><br><br>\| top limit=6 <field_name>|
|**Example**|`top limit=3 EventID`|

The following command will display the top 7 Image ( representing Processes) captured.

**Search Query:** `index=windowslogs | top limit=7 Image`

  
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/ffef6c0a6ce0159e6d970de2c32921a3.png)  

**Rare**  

|   |   |
|---|---|
|**Command**|**rare**|
|**Explanation  <br>**|This command does the opposite of top command as it returns the least frequent values or bottom 10 results.|
|**Syntax  <br>**|\| rare <field_name><br><br>\| rare limit=6 <field_name>|
|**Example**|`rare limit=3 EventID`|

The following command will display the rare 7 Image (Processes) captured.

**Search Query:** `index=windowslogs | rare limit=7 Image`  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/8123e0932c169514d9842fbba8f5898c.png)  

**Highlight**

|   |   |
|---|---|
|**Command**|**highlight**|
|**Explanation  <br>**|The highlight command shows the results in raw events mode with fields highlighted.|
|**Syntax  <br>**|highlight      <field_name1>      <field_name2>|
|**Example**|`highlight User, host, EventID, Image`|

The following command will highlight the three mentioned fields in the raw logs  

**Search Query:** `index=windowslogs | highlight User, host, EventID, Image`

  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/61ad47b204639fa0f75b278bec21abac.gif)  

  

STATS Commands

SPL supports various stats commands that help in calculating statistics on the values. Some common stat commands are:

|   |   |   |   |
|---|---|---|---|
|**Command**|**Explanation**|**Syntax**|**Example**|
|**Average  <br>**|This command is used to calculate the average of the given field.|stats avg(field_name)|stats avg(product_price)|
|**Max  <br>**|It will return the maximum value from the specific field.|stats max(field_name)|stats max(user_age)|
|**Min**|It will return the minimum value from the specific field.|stats min(field_name)|stats min(product_price)|
|**Sum**|It will return the sum of the fields in a specific value.|stats sum(field_name)|stats sum(product_cost)|
|**Count**|The count command returns the number of data occurrences.|stats count(function) AS new_NAME|stats count(source_IP)|

**Splunk Chart Commands**  

These are very important types of transforming commands that are used to present the data in table or visualization form. Most of the chart commands utilize various stat commands.

Chart

|   |   |
|---|---|
|**Command**|**chart**|
|**Explanation  <br>**|The chart command is used to transform the data into tables or visualizations.|
|**Syntax  <br>**|\| chart <function>|
|**Example**|\| `chart count by User`|

**Search Query:** `index=windowslogs | chart count by User`

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/a954b0a1d37542650df294461d756c61.gif)  

Timechart

|   |   |
|---|---|
|**Command**|**timechart**|
|**Explanation  <br>**|The timechart command returns the time series chart covering the field following the function mentioned. Often combined with STATS commands.|
|**Syntax  <br>**|\| timechart function  <field_name>|
|**Example**|\| `timechart count by Image`|

The following query will display the Image chart based on the time.  

**Search Query:** `index=windowslogs | timechart count by Image`

  
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/bc7f90e4f40be4c047d250d5e3ee44c8.gif)

Answer the questions below

List the top 8 Image processes using the top command -  what is the total count of the 6th Image?

```
index=windowslogs 
| top limit=8 Image
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231122234811.png]]

Ans: 196

Using the rare command, identify the user with the least number of activities captured?

```
index=windowslogs 
| rare User
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231122235122.png]]

Ans: James

Create a pie-chart using the chart command - what is the count for the conhost.exe process?

```
index=windowslogs 
| chart count by Image
```

![[THM-Labs/Splunk Exploring SPL/screenshots/Pasted image 20231123001614.png]]

Ans: 70

# Task 8  Recap and Conclusion