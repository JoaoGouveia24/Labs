# Pickle Rick Machine Report


**Recon**
First we are using a simple command to map the IP that we have:

    sudo nmap -p- -sS -sV 10.112.152.184

Result:

PORT STATE SERVICE VERSION

22/tcp open ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)

80/tcp open http Apache httpd 2.4.41 ((Ubuntu))


  
With this we know that the ip has a website hosted in the port 80, lets check it...
 
When inspecting the website we can see inside a "div" a word... (*R1ckRul3s*)... must be a username somehow, lets keep going...

After cheeking the entire website, its time to see if it has different paths, for this we are using a tool called "Gobuster"

    gobuster dir -u http://10.112.152.184/ -w /usr/share/wordlists/dirb/common.txt
    

Result:
  
===============================================================

Gobuster v3.8.2

by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)

===============================================================

[+] Url: http://10.112.152.184/

[+] Method: GET

[+] Threads: 10

[+] Wordlist: /usr/share/wordlists/dirb/common.txt

[+] Negative Status codes: 404

[+] User Agent: gobuster/3.8.2

[+] Timeout: 10s

===============================================================

Starting gobuster in directory enumeration mode

===============================================================

- .hta (Status: 403) [Size: 279]
- .htpasswd (Status: 403) [Size: 279]
- .htaccess (Status: 403) [Size: 279]
- assets (Status: 301) [Size: 317] [--> http://10.112.152.184/assets/]
- index.html (Status: 200) [Size: 1062]
- robots.txt (Status: 200) [Size: 17]
- server-status (Status: 403) [Size: 279]
- Progress: 4613 / 4613 (100.00%)

===============================================================

Finished

===============================================================

The result was interesting because we could find some interesting paths, like the "robots.txt"
  

If we try the following link http://10.112.152.184/Robots.txt, we are sent to a new page with the word "Wubbalubbadubdub"

We presume that this must be the password... so we have these:

    Username:R1ckRul3s

    Password: Wubbalubbadubdub


Using the gobuster tool we have a path called "assets" and we have an image called "Portal.jpg", strange usually portal is a login page lets trt "/Portal"

In the end we  have a new path http://10.112.152.184/Portal.php, we are sent to a login page, great chance to use the credential we have above.  

*Tadaaa! We are in!!!*

In this page we as we can see we have an input field called "Console"
By chance i tried the command `ls`


Result:

 - Sup3rS3cretPickl3Ingred.txt 
 - assets 
 - clue.txt 
 - denied.php 
 - index.html
 -  login.php 
 - portal.php 
 - robots.txt

 
First i needed to see the first ".txt" http://10.112.152.184/Sup3rS3cretPickl3Ingred.txt

With that we have the first ingredient: `mr. meeseek hair`

If the console could read the command "ls" the same console must execute the comand "sudo"
  
With that in mind i tried this way `sudo ls ../../../*`
This command lists every folder and sub folder in the directory...

In the path "/home/rick/second ingredients" we found the second ingredient : `1 Jerry Tear`

In the path "/root/3rd.txt" we found the therd ingredient: `fleeb juice`

**Machine Pawned!!!**
