**Room Link - https://tryhackme.com/room/networkservices**

## SMB
enum4linux

![[Pasted image 20220204091354.png]]


Syntax

`smbclient //[IP]/[SHARE]`

![[Pasted image 20220204091502.png]]

Meanwhile on the machine we can see the following files.

![[Pasted image 20220204092655.png]]

I got the file using get 'filename'
and It gave us a user John Cactus

![[Pasted image 20220204092756.png]]


Next we can look into the .ssh folder and get the id_rsa file to ssh into the machine remotely and giving it chmod 600 .

![[Pasted image 20220204092837.png]]

## Telnet

![[Pasted image 20220204093607.png]]

Trying to get a reverse shell 

![[Pasted image 20220204095458.png]]

Since no command is working lets set up a tcpdump listener using

`sudo tcpdump ip proto \\icmp -i tun0`

we can see that we are getting responses

![[Pasted image 20220204095708.png]]

Then we craft a msfvenom payload to get a netcat reverse shell session and paste that payload into the telnet session

![[Pasted image 20220204100939.png]]

Yay we got the flag :)

## FTP

**Active vs Passive**

The FTP server may support either Active or Passive connections, or both. 

-   In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it. 
-   In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it.


**Method**

We're going to be exploiting an anonymous FTP login, to see what files we can access- and if they contain any information that might allow us to pop a shell on the system. This is a common pathway in CTF challenges, and mimics a real-life careless implementation of FTP servers.


As we can see in this nmap scan Anonymous FTP login is allowed

![[Pasted image 20220204104018.png]]

**Alternative Enumeration Methods**

It's worth noting  that some vulnerable versions of in.ftpd and some other FTP server variants return different responses to the "cwd" command for home directories which exist and those that don’t. This can be exploited because you can issue cwd commands before authentication, and if there's a home directory- there is more than likely a user account to go with it. While this bug is found mainly within legacy systems, it's worth knowing about, as a way to exploit FTP.  

This vulnerability is documented at: [https://www.exploit-db.com/exploits/20745](https://www.exploit-db.com/exploits/20745)

We can use the get command to grab the file and then open it
IT gives us the name of user MIKE

![[Pasted image 20220204105352.png]]

Time to fire up Hydra

![[Pasted image 20220204110148.png]]

Next up we use hydra to brute force the password 

![[Pasted image 20220204110102.png]]

## Refs

-   [https://medium.com/@gregIT/exploiting-simple-network-services-in-ctfs-ec8735be5eef](https://medium.com/@gregIT/exploiting-simple-network-services-in-ctfs-ec8735be5eef)
-   [https://attack.mitre.org/techniques/T1210/](https://attack.mitre.org/techniques/T1210/)
-   [https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/](https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/)
-   https://tryhackme.com/room/networkservices






