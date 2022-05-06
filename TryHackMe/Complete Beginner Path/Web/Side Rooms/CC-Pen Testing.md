**Room Link - https://tryhackme.com/room/ccpentesting**

## Nmap

![[Pasted image 20220210120555.png]]

![[Pasted image 20220210120613.png]]


## Netcat

Netcat aka nc is an extremely versatile tool. It allows users to connect to specific ports and send and receive data. It also allows machines to receive data and connections on specific ports, which makes nc a very popular tool to gain a [Reverse Shell.](https://resources.infosecinstitute.com/icmp-reverse-shell/#gref)

  

After you connect to a port with nc you will be able to send data, this also has the consequence of the user being able to pipe data through nc. For example one can do `echo hello | nc <ip> 1234` to send the string hello to the service running on port 1234  

  

Note: There are multiple versions of nc, so if you are unable to find an answer in your specific man page, try reading the man page for others!


![[Pasted image 20220210120656.png]]

## Gobuster

![[Pasted image 20220210120742.png]]

![[Pasted image 20220210120755.png]]

## Nikto

nikto is a popular web scanning tool that allows users to find common web vulnerabilities. It is commonly used to check for common CVE's such as shellshock, and to get general information about the web server that you're enumerating.

![[Pasted image 20220210121330.png]]


## Metasploit

### 1. msfconsole

![[Pasted image 20220210121827.png]]

![[Pasted image 20220210124058.png]]

### 2. meterpreter

Once you've run the exploit, ideally it will give you one of two things, a regular command shell or a meterpreter shell. Meterpreter is metasploits own "control center" where you can do various things to interact with the machine. A list of common meterpreter commands and their uses can be found [here](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)  

Note: Regular shells can usually be upgraded to meterpreter shells by using the module `post/multi/manage/shell_to_meterpreter'

![[Pasted image 20220210124207.png]]

### 3.  CTF

Initial nmap scan gave away nostromo serving at port 80

![[Pasted image 20220210125043.png]]

Searching for it in msfconsole I found an exploit. Set the options and yay we got a shell..

![[Pasted image 20220210125152.png]]

![[Pasted image 20220210125211.png]]

Used the shell command to spawn an interactive shell. Not stabilized though

![[Pasted image 20220210125402.png]]

![[Pasted image 20220210125414.png]]


## Hashcat

Hashcat does not automatically detect the hash type like [[John	]].

hashcat -m 900 tmp.txt -a 0 -o cracked.txt /usr/share/wordlists/rockyou.txt 

https://resources.infosecinstitute.com/topic/hashcat-tutorial-beginners/

![[Pasted image 20220211081453.png]]

![[Pasted image 20220211081504.png]]


## John

Always use **Raw** when using --format option.

![[Pasted image 20220211083511.png]]

![[Pasted image 20220211083535.png]]

## Sqlmap

![[Pasted image 20220211084310.png]]

![[Pasted image 20220211084327.png]]

### Vuln Machine

**Payload - sqlmap -u http://10.10.20.34 --dump --forms  **

![[Pasted image 20220211090232.png]]

![[Pasted image 20220211090247.png]]

![[Pasted image 20220211090149.png]]

## Smbmap

smbmap is one of the best ways to enumerate samba. smbmap allows pen-testers to run commands(given proper permissions), download and upload files, and overall is just incredibly useful for smb enumeration.

![[Pasted image 20220211090835.png]]

## Smbclient

smbclient allows you to do most of the things you can do with smbmap, and it also offers you and interactive prompt.

![[Pasted image 20220211091121.png]]

**impacket is a collection of extremely useful windows scripts. It is worth mentioning here, as it has many scripts available that use samba to enumerate and even gain shell access to windows machines. All scripts can be found [here](https://github.com/SecureAuthCorp/impacket).**


## [[Privilege Escalation]]

[[Privilege Escalation]] is such a large topic that it would be impossible to do it proper justice in this type of room. However, it is a necessary topic that must be covered, so rather than making a task with questions, I shall provide you all with some resources.

General:

[https://github.com/swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) (A bunch of tools and payloads for every stage of pentesting)[  
](https://github.com/swisskyrepo/PayloadsAllTheThings)

  

Linux:

[https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) (a bit old but still worth looking at)

[https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum) (One of the most popular priv esc scripts)

[https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh](https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh) (Another popular script)

[https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester) (A Script that's dedicated to searching for kernel exploits)

  

[https://gtfobins.github.io](https://gtfobins.github.io/) (I can not overstate the usefulness of this for priv esc, if a common binary has special permissions, you can use this site to see how to get root perms with it.)  

  

Windows:

  

[https://www.fuzzysecurity.com/tutorials/16.html](https://www.fuzzysecurity.com/tutorials/16.html)  (Dictates some very useful commands and methods to enumerate the host and gain intel)

  

[https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerUp](https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerUp) (A bit old but still an incredibly useful script)

  

[https://github.com/411Hall/JAWS](https://github.com/411Hall/JAWS) (A general enumeration script)

## Final Challenge

Gobuster Scan

![[Pasted image 20220211101051.png]]

It revealed a secret dir and then upon searching further extensions like .txt , .php , .html 
I found a secret.txt file

Upon visiting that url I got the hashed password of user nyan

![[Pasted image 20220211101421.png]]

which was nyan xD
After then it was easy to ssh and get user.txt
and for privesc I used the **/bin/su** SUID 

![[Pasted image 20220211101535.png]]

![[Pasted image 20220211101603.png]]


