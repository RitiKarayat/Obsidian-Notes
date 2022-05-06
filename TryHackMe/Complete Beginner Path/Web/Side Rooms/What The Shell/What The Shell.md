**Room Link - https://tryhackme.com/room/introtoshells **

# Tools

## 1. Netcat
It is the go to tool for network connection establishment and can be used the receive a reverse shell. The shells though are unstable. 

## 2. Socat
Socat is like netcat on steroids. It can do all of the same things, and _many_ more. Socat shells are usually more stable than netcat shells out of the box. In this sense it is vastly superior to netcat; however, there are two big catches:

1.  The syntax is more difficult
2.  Netcat is installed on virtually every Linux distribution by default. Socat is very rarely installed by default.

## 3. Metasploit 
1. auxiliary/multi/handler - This exploit can be used to get reverse shells like netcat and socat but more stable and reliable. It is also the only way to interact with a meterpreter shell.
2. msfvenom - It is a standalone utility to generate various types of payloads.

**Kali Linux also has its own list of webshells in /usr/share/webshells**

# [[Types of Shells]]

# [[Shell Stabilization]]

# [[Socat]]

# [[Socat Encrypted Shells]]

# [[Common Shell Payloads]]

# [[msfvenom]]

# [[Metasploit multi-handler]]

# [[WebShells]]

# Next Steps

Ok, we have a shell. Now what?  
  
  
  
We've covered lots of ways to generate, send and receive shells. The one thing that these all have in common is that they tend to be unstable and non-interactive. Even Unix style shells which are easier to stabilise are not ideal. So, what can we do about this?

On Linux ideally we would be looking for opportunities to gain access to a user account. SSH keys stored at `/home/<user>/.ssh` are often an ideal way to do this. In CTFs it's also not infrequent to find credentials lying around somewhere on the box. Some exploits will also allow you to add your own account. In particular something like [Dirty C0w](https://dirtycow.ninja/) or a writeable /etc/shadow or /etc/passwd would quickly give you SSH access to the machine, assuming SSH is open.

On Windows the options are often more limited. It's sometimes possible to find passwords for running services in the registry. VNC servers, for example, frequently leave passwords in the registry stored in plaintext. Some versions of the FileZilla FTP server also leave credentials in an XML file at `C:\Program Files\FileZilla Server\FileZilla Server.xml`  
 or `C:\xampp\FileZilla Server\FileZilla Server.xml`  
. These can be MD5 hashes or in plaintext, depending on the version.

Ideally on Windows you would obtain a shell running as the SYSTEM user, or an administrator account running with high privileges. In such a situation it's possible to simply add your own account (in the administrators group) to the machine, then log in over RDP, telnet, winexe, psexec, WinRM or any number of other methods, dependent on the services running on the box.

The syntax for this is as follows:

`net user <username> <password> /add`

`net localgroup administrators <username> /add`

---

_The important take away from this task:_

Reverse and Bind shells are an essential technique for gaining remote code execution on a machine, however, they will never be as fully featured as a native shell. Ideally we always want to escalate into using a "normal" method for accessing the machine, as this will invariably be easier to use for further exploitation of the target.

# [[Practice]]