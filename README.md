# CTF-TryHackMe-Basic_Pentesting

###  Find the services exposed by the machine

The first we are asked to do is find the services exposed by the machine. We can use nmap to find open ports and services.\
**nmap -A -T4 10.10.72.81 -oN nmap_initial**\
\
![nmap_Basic_pentesting 2022-01-21 110914](https://user-images.githubusercontent.com/86057471/150501345-127fda31-0ab3-48fa-be66-565319e415c7.png)\
\
We found six open ports and services on the machine.

### What is the name of the hidden directory on the web server(enter name without /)?

We can use a tool called gobuster to find all directories on the webserver we found open in the nmap scan.\
**gobuster dir -u 10.10.62.148 -w /usr/share/wordlists/dirb/common.txt  -oN gobuster**\
\
![gobuster_basicpentesting2022-01-21 111052](https://user-images.githubusercontent.com/86057471/150502129-8a4ce260-1524-4945-b04e-cdd560b13941.png)\
\
We find the hidden directory we were looking for: **devolopment**

### Use brute-forcing to find the username & password

In our nmap scan we found smb open. We can use a tool called enum4linux to bruteforce.\
**enum4linux -a 10.10.62.148**

### What is the username?

Running enum4linux we find two username.
**jan, kay**\
\
![enum4linux_basicpentesting2022-01-21 114847](https://user-images.githubusercontent.com/86057471/150505475-e1ca8ad6-7346-4469-b5d6-194a80a46457.png)

The username the question wanted is **jan**.

### What is the password?



