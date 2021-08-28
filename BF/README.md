# BF Challenge

![](https://i.imgur.com/XYliZuY.jpeg)

## Credit

Credit to [0xMohamedHassan](https://twitter.com/0xMohamedHassan) for the excellent Challenge \o/.

## TL;DT
1. Check requests in the PCAP and find requests to cloudme which indicates cloudme software on the victim machine.

2. Search for cloudme exploit to find BoF vuln.

3. Check TCP requests to 8888.

4. dump the payload and analyze it with scdbg or sctest.

## Solution

Okay starting by exploring the PCAP as always. Following the requests, we see that there is requests to cloudme 

![](https://imgur.com/RoM6xUW.png) 

This indicates cloudme software on the victim machine. 

Looking for cloudme software exploits, and we find interesting results: 

[BOF Exploit](https://www.exploit-db.com/exploits/48389)

In the exploit there is: **s.connect((target,8888)** 

Wireshark filter: **tcp.port == 8888**

![](https://imgur.com/Kc8bXk9.png)

Copy data of Packet 48961 and reverse the hexdump.

Open the file in scdbg and launch it:

![](https://i.imgur.com/q4tktZy.jpg)

Flag : **FwordCTF{f0r3n51c4_15n0t_4b0u7_70015_4f73r_411}**
