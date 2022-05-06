
![[Pasted image 20220131032112.png]]


## Find

![[Pasted image 20220131035711.png]]

![[Pasted image 20220131035732.png]]

## Vim

- https://tryhackme.com/room/toolboxvim
- https://vim.rtorr.com/
- https://devhints.io/vim

## SCP

![[Pasted image 20220131045824.png]]

## PS & TOP

![[Pasted image 20220131051358.png]]

**Managing Processes**

You can send signals that terminate processes; there are a variety of types of signals that correlate to exactly how "cleanly" the process is dealt with by the kernel. To kill a command, we can use the appropriately named `kill` command and the associated PID that we wish to kill. i.e., to kill PID 1337, we'd use `kill 1337`.

Below are some of the signals that we can send to a process when it is killed:

-   SIGTERM - Kill the process, but allow it to do some cleanup tasks beforehand
-   SIGKILL - Kill the process - doesn't do any cleanup after the fact
-   SIGSTOP - Stop/suspend a process


![[Pasted image 20220131051518.png]]

 **Getting Processes/Services to Start on Boot**

Some applications can be started on the boot of the system that we own. For example, web servers, database servers or file transfer servers. This software is often critical and is often told to start during the boot-up of the system by administrators.

In this example, we're going to be telling the apache web server to be starting apache manually and then telling the system to launch apache2 on boot.

Enter the use of `systemctl` -- this command allows us to interact with the **systemd** process/daemon. Continuing on with our example, systemctl is an easy to use command that takes the following formatting: `systemctl [option] [service]`

For example, to tell apache to start up, we'll use `systemctl start apache2`. Seems simple enough, right? Same with if we wanted to stop apache, we'd just replace the `[option]` with stop (instead of start like we provided)

We can do four options with `systemctl`:

-   Start
-   Stop
-   Enable
-   Disable


## Backgrounding & Foregrounding

![[Pasted image 20220131051622.png]]

Here we're running `echo "Hi THM"` , where we expect the output to be returned to us like it is at the start. But after adding the `&` operator to the command, we're instead just given the ID of the echo process rather than the actual output -- as it is running in the background.

This is great for commands such as copying files because it means that we can run the command in the background and continue on with whatever further commands we wish to execute (without having to wait for the file copy to finish first)

We can do the exact same when executing things like scripts -- rather than relying on the & operator, we can use `Ctrl + Z` on our keyboard to background a process. It is also an effective way of "pausing" the execution of a script or command like in the example below:

![[Pasted image 20220131051651.png]]

![[Pasted image 20220131051732.png]]


## Some Other Rooms

-   The find command - [https://tryhackme.com/room/thefindcommand](https://tryhackme.com/room/thefindcommand)
-   Bash Scripting - [https://tryhackme.com/room/bashscripting](https://tryhackme.com/room/bashscripting)
-   Regular Expressions - [https://tryhackme.com/room/catregex](https://tryhackme.com/room/catregex)



