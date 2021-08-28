# Crypt Challenge

![Description](https://i.imgur.com/bONs6u6.jpeg)

## TL;DR
**1 -** Checking installed app, find thunderbird.

**2 -** Extracting inbox and spam files.

**3 -** Get the private key and passphrase location

**4 -** Import key and get flag

## Solution

As always, the first thing to do after downloading the challenge file is identifying the profile to work with

```bash
vol.py -f challenge.raw imageinfo
```

the profile is : **Win7SP1x64**

Now checking cmdline, we will find many interesting things: 

```
C:\Windows\system32\NOTEPAD.EXE" C:\Users\SemahAB\AppData\Roaming\Thunderbird\Profiles\gb1i6asd.default-release\ImapMail\imap.yandex.com\INBOX
C:\Users\SemahAB\Downloads\Secret content\flag.txt.asc
C:\Windows\system32\NOTEPAD.EXE" C:\Users\SemahAB\AppData\Roaming\Thunderbird\Profiles\gb1i6asd.default-release\ImapMail\imap.yandex.com\Spam
```
Thunderbird related files which is email application, so this is our way since the challenge asks about DateOfReceive and Sender ...

So dumping those files and exploring them:

### INBOX

It's from ``ctf user`` okay so we got ourselves the Sender, and Date: ``Friday, 20 Aug 2021 5:03``

![](https://i.imgur.com/KTouqjB.jpeg)

And at the end of the mail, there is ``Don't forget about the decoding part we told you by phone !``

### SPAM

![](https://i.imgur.com/zrcAeRQ.jpeg)

So there is an environment variable ``345YACCESSTOGENERATEP455`` 

getting the envars 

```bash
vol.py --profile=Win7SP1x64 -f challenge.raw envars | grep -i 345YACCESSTOGENERATEP455
```

we get the value: ``pPaAsSpPhHrR445533`` 

### flag.txt.asc

![](https://i.imgur.com/kjxHSlJ.jpeg)

**PGP message**

Okay so putting all pieces together, we have an encrypted file and from the inbox mail ``importing this`` so this is the private key, and from the SPAM that's the passphrase?

Okay trying all this with the following steps: 

1 - Starting by decoding the base64 encoded string as mentioned in the mail and put it in a file called key

2 - import the key ``gpg --import key`` with passphrase **pPaAsSpPhHrR445533**

3 - decrypt the file: ``gpg --decrypt flag.txt.asc``

and yes we got the content: ``23b2e901f3c3c3827a70589efd046be8``

So perfect! Now putting all pieces together: 

FwordCTF{DateOfReceive_SenderUsername_contentofencrypted File} with the format mentioned in the description:

Flag : ``FwordCTF{08202021-05:03_ctf.user_23b2e901f3c3c3827a70589efd046be8}``
