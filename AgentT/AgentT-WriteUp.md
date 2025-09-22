
# Agent T

Welcome to my write-up. Agent T (https://tryhackme.com/room/agentt) is a simple yet interesting challenge.

For this room I will use:

- Nmap

- Exploit-DB

Connect to the VPN, start your machine, and let's begin.


## Enumeration

The first step is always enumeration. I ran an Nmap scan against the target:

`nmap -sC -A <TARGET_IP>`

![Nmap scan screenshot](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/AgentT/screenshots/nmap-scan.png)

Here we can see a web server on port **80** running **PHP 8.1.0-dev**.

Open the target in your browser (`http://<TARGET_IP>`) and inspect the web page visually first.

![Admin dashboard](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/AgentT/screenshots/admin-dashboard.png)

We have access to an **admin dashboard** while authenticated as `admin`. With admin access, the attack surface increases significantly.

All of the visible buttons on the admin dashboard point to `/#`. So let's move on.







## Gaining a root shell
With quick research I found that **PHP 8.1.0-dev** is vulnerable to arbitrary code execution by sending a specially crafted `User-Agentt` HTTP header.

For more information see the public write-up on Exploit-DB: https://www.exploit-db.com/exploits/49933

I downloaded the exploit and executed it:

`python3 49933.py`

`http://<TARGET_IP>`

Running `id` confirmed that we gained root privileges — the exploit worked as expected.

![Shell](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/AgentT/screenshots/root-shell.png)








## Obtaining flag.txt
I notice that commands such as `cd` or `cd ..` do not work as expected. This is because sh is a simple shell that runs each command in a **separate process**. 
As a result, cd only affects that single process and the next command runs in the original working directory again.


![Shell](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/AgentT/screenshots/ls-cd.png)

The solution is to run multiple commands in a single invocation.

To get **flag.txt** I used:

`cd .. && cd .. && cd .. && ls -la` 
 
to locate the flag, and then:

`cd .. && cd .. && cd .. && cat flag.txt`

to view it.

![Flag](https://raw.githubusercontent.com/D3ad-Byt3/THM-WriteUps/main/AgentT/screenshots/AgentFlag.png)

## Thank You!
Thank you for taking the time to read my write-up on the **Agent T** room.  
I hope it was helpful and gave you a clear view of the steps I followed

Author: [@D3adByt3](https://www.github.com/D3ad-Byt3)
