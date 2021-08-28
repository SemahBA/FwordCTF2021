# listening? Challenge

![](https://i.imgur.com/RXF4ESC.jpeg)

## Credit

Credit to [0xMohamedHassan](https://twitter.com/0xMohamedHassan) for the excellent idea \o/ !

## TL;DR

1 - Sent data in ICMP Packet

2 - Oauth Creds

3 - Oauth playground and Read Mails

## Solution

Starting by exploring the PCAP

![](https://imgur.com/JNTuFTw.png)

from dns query there is **oauth2.googleapis.com**, we will leave this for now and keep looking into the PCAP.

![](https://imgur.com/Qe9rQKu.png)

we get this value: 

![](https://imgur.com/328ejQG.png)

so user-agent is: **google-oauth-playground** and got clientID, client_secret, refresh_token, and email.

Going to [oauth-playground](https://developers.google.com/oauthplayground/) and check **use our own OAuth credentials**

Go to Step2 and URL decode the refresh_token and put in **Refresh Token** and click on **Refresh access token**

![](https://i.imgur.com/ojK2XcO.jpeg)

now in Step 3, Set the Request URL to **https://gmail.googleapis.com/gmail/v1/users/fwordplayground@gmail.com/messages** 

[resource](https://developers.google.com/gmail/api/reference/rest/v1/users.messages/list)

To read it, just add /ID 

So going through the IDs, 

**https://gmail.googleapis.com/gmail/v1/users/fwordplayground@gmail.com/messages/17b7d85d21fc05ba** Will give the flag

![](https://i.imgur.com/SPIdwcC.jpeg)

Flag is: **FwordCTF{email_forensics_is_interesting_73489nn7n4891}** 
