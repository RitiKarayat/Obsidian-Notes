## 1. Reverse Shell
These are the shells in which we start a listener of our machine and execute the payload on the target which connects to our listener and allows us to perform RCE. These shells are useful for evading firewalls that may happen while using the shell on the target directly. However the configuration is needed for our own network so that the target can connect freely.

![[Pasted image 20220213115834.png]]

## 2. Bind Shells
These are the exact opposite of revshells in which we start a listener attached to a shell on the target and it opens a port for us to connect remotely. But these shells are prone to firewall rules.

![[Pasted image 20220213115851.png]]


The final concept which is relevant in this task is that of interactivity. Shells can be either _interactive_ or _non-interactive_.

-   _Interactive:_ If you've used Powershell, Bash, Zsh, sh, or any other standard CLI environment then you will be used to  
    interactive shells. These allow you to interact with programs after executing them. For example, take the SSH login prompt:
	
	![[Pasted image 20220213115948.png]]
	
_Non-Interactive_ shells don't give you that luxury. In a non-interactive shell you are limited to using programs which do not require user interaction in order to run properly. Unfortunately, the majority of simple reverse and bind shells are non-interactive, which can make further exploitation trickier. Let's see what happens when we try to run SSH in a non-interactive shell:

![[Pasted image 20220213120212.png]]