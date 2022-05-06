**Room Link - https://tryhackme.com/room/networkservices2**

## NFS

**What is NFS?**

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

**How does NFS work?**

First, the client will request to mount a directory from a remote host on a local directory just the same way it can mount a physical device. The mount service will then act to connect to the relevant mount daemon using RPC.

The server checks if the user has permission to mount whatever directory has been requested. It will then return a file handle which uniquely identifies each file and directory that is on the server.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

-    The file handle
-    The name of the file to be accessed
-    The user's, user ID
-    The user's group ID

![[Pasted image 20220205114954.png]]


### Enumeration

After conducting the nmap scan we got this

![[Pasted image 20220205123455.png]]

Use the showmount command for viewing the shares and then mount it on a local dir

![[Pasted image 20220205123003.png]]

Now we can ssh into the machine using the id_rsa file


![[Pasted image 20220205123028.png]]


### Exploitation


**What is root_squash?**

By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.


Now all its left is to create a SUID file with owner root 
Link for the executable file 
https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash

 NFS Access ->

        Gain Low Privilege Shell ->

            Upload Bash Executable to the NFS share ->

                Set SUID Permissions Through NFS Due To Misconfigured Root Squash ->

                    Login through SSH ->

                        Execute SUID Bit Bash Executable ->

                            ROOT ACCESS

Lets do this!

![[Pasted image 20220205125349.png]]

Now we just download the file and give it perms and ownership of root
ssh in the machine and run the SUID
Yay we got root..!!

![[Pasted image 20220205125442.png]]


## SMTP

![[Pasted image 20220206120842.png]]


1. The mail user agent, which is either your email client or an external program. connects to the SMTP server of your domain, e.g. smtp.google.com. This initiates the SMTP handshake. This connection works over the SMTP port- which is usually 25. Once these connections have been made and validated, the SMTP session starts.  

2. The process of sending mail can now begin. The client first submits the sender, and recipient's email address- the body of the email and any attachments, to the server.  

3. The SMTP server then checks whether the domain name of the recipient and the sender is the same.

4. The SMTP server of the sender will make a connection to the recipient's SMTP server before relaying the email. If the recipient's server can't be accessed, or is not available- the Email gets put into an SMTP queue.  

5. Then, the recipient's SMTP server will verify the incoming email. It does this by checking if the domain and user name have been recognised. The server will then forward the email to the POP or IMAP server, as shown in the diagram above.  

6. The E-Mail will then show up in the recipient's inbox.
 
For More Info
[https://computer.howstuffworks.com/e-mail-messaging/email3.htm](https://computer.howstuffworks.com/e-mail-messaging/email3.htm)

[https://www.afternerd.com/blog/smtp/](https://www.afternerd.com/blog/smtp/)

### Enumeration

**Enumerating Server Details**

Poorly configured or vulnerable mail servers can often provide an initial foothold into a network, but prior to launching an attack, we want to fingerprint the server to make our targeting as precise as possible. We're going to use the "_smtp_version_" module in MetaSploit to do this. As its name implies, it will scan a range of IP addresses and determine the version of any mail servers it encounters.

**Enumerating Users from SMTP**

The SMTP service has two internal commands that allow the enumeration of users: VRFY (confirming the names of valid users) and EXPN (which reveals the actual address of user’s aliases and lists of e-mail (mailing lists). Using these SMTP commands, we can reveal a list of valid users

We can do this manually, over a telnet connection- however Metasploit comes to the rescue again, providing a handy module appropriately called "_smtp_enum_" that will do the legwork for us! Using the module is a simple matter of feeding it a host or range of hosts to scan and a wordlist containing usernames to enumerate.

After the initial nmap scan we found the OS i.e Ubuntu 
And ssh and smtp services open.

![[Pasted image 20220206122523.png]]

Lets fire up metasploit and use the smtp_version module 

![[Pasted image 20220206122650.png]]

And we got more info about the smtp server

Now its time to enumerate usernames and this can be done using the smtp_enum module of metasploit.
We will not be using the default wordlist instead we will use **Seclists**.

![[Pasted image 20220206123140.png]]

Yay we got a valid user **administrator**.


### Exploitation

Time to call our friend hydra to the rescue.
Now that we have found a valid username and we know ssh is open we can bruteforce..!!

![[Pasted image 20220206123832.png]]

Seems like hydra and rockyou.txt are a perfect pair

Username - **administrator**
Password - **alejandro**

![[Pasted image 20220206124057.png]]


## MySQL

### Enumeration

Initial Nmap scan revealed the ports of ssh and mysql 
since we have the credentials **root:password** we try to login to mysql server

![[Pasted image 20220207021307.png]]

Mysql wont be the first point of enumeration in CTFs but fot the sake of this room they've given us the creds.

Again, we're going to be using Metasploit for this; it's important that you have Metasploit installed, as it is by default on both Kali Linux and Parrot OS.

**Alternatives**

As with the previous task, it's worth noting that everything we will be doing using Metasploit can also be done either manually or with a set of non-Metasploit tools such as nmap's mysql-enum script: 
[https://nmap.org/nsedoc/scripts/mysql-enum.html](https://nmap.org/nsedoc/scripts/mysql-enum.html) 

**Results for the Nmap script**

![[Pasted image 20220207023424.png]]

or 

[https://www.exploit-db.com/exploits/23081](https://www.exploit-db.com/exploits/23081). I recommend that after you complete this room, you go back and attempt it manually to make sure you understand the process that is being used to display the information you acquire.

Okay, enough talk. Let's get going!

Fire up msfconsole and seach for **mysql_sql** module
set the following options and run it

![[Pasted image 20220207022043.png]]

we got some version info because its the default sql option set.

Lets change the options to **show databases**

![[Pasted image 20220207021929.png]]



## Exploitation

Lets seach for **mysql_schemadump** module to get the schemas returned to us

![[Pasted image 20220207022659.png]]

Lets run the exploit..!!

![[Pasted image 20220207022730.png]]

Beautiful but thats not all we can exploit further using the **mysql_hashdump** module to get all the password hashes stored in the server.

![[Pasted image 20220207021803.png]]

Now we can see many users here one of them being root whose password we already know. Another user of particular interest is **carl**.
Lets try to crack the hash of carl. Copy the full hash like **carl:hash** and paste it into a file hash.txt. 
Then use john to crack the hash.

![[Pasted image 20220207023103.png]]

And we got the password as **doggie**
Going back we had an ssh port open on this machine. Lets try to login to that using carl's password.

![[Pasted image 20220207023201.png]]

Hurray..!! We got the flag.


![[Pasted image 20220207021642.png]]


## Additional Resources


-   [https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-sg-en-4/ch-exploits.html](https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-sg-en-4/ch-exploits.html)
-    [https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/](https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/)






