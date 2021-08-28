# eLearning Challenge

![](https://i.imgur.com/JmfEb9a.jpeg)

## TL;DR

1 - Installed Apps, There is zenmap, Mailbird

2 - dump store.db to get the email received

3 - Powershell history to get IP and tools used

4 - renamed file from Powershell history, looking for it and get its content

## Solution

Going through files, **Program Files** shows that there is Mailbird installed, so looking [Messages Location](https://www.bitrecover.com/blog/where-does-mailbird-store-emails-and-contacts/)

Going to %APPDATA%\Local\Mailbird\Store\Store.db

![](https://i.imgur.com/tIsrma9.jpeg)

Checking the FTS_Messages table, we found the email from Fwordelearn, looking through the email we find this: 

``I'm SBA, and will be your constructor during this course.`` [sorry it is 'instructor']

So instructor is **SBA**

So looking for nmap files, didn't give much. But maybe launched from Powershell?

Looking through Powershell history: **%APPDATA%\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt**

and we find this: 

![](https://i.imgur.com/TSbfiIj.jpg)

Nice, so executing nmap, dirsearch and wget!

From the history file we can extract:
- IP: 192.168.1.9

- Tools: nmap, dirsearch, wget

- file which is secret.txt

- the secret.txt has been renamed to file.txt, so going to file.txt path: **Documents** and read its content: **663cd2dfc9418f384d90c89a15319b3d**

now we have everything:

Following the flag format: FwordCTF{InstructorName_targetIPTested_toolsUsedToReach TheFileandDownloadIt_NameOfFoundFile_Content}

Flag is: **FwordCTF{SBA_192.168.1.9_dirsearch-nmap-wget_secret.txt_663cd2dfc9418f384d90c89a15319b3d}**
