There are times when we encounter websites that allow us an opportunity to upload, in some way or another, an executable file. Ideally we would use this opportunity to upload code that would activate a reverse or bind shell, but sometimes this is not possible. In these cases we would instead upload a _webshell_. See the [Upload Vulnerabilities Room](https://tryhackme.com/room/uploadvulns) for a more extensive look at this concept.

"Webshell" is a colloquial term for a script that runs inside a webserver (usually in a language such as PHP or ASP) which executes code on the server. Essentially, commands are entered into a webpage -- either through a HTML form, or directly as arguments in the URL -- which are then executed by the script, with the results returned and written to the page. This can be extremely useful if there are firewalls in place, or even just as a stepping stone into a fully fledged reverse or bind shell.

As PHP is still the most common server side scripting language, let's have a look at some simple code for this.

In a very basic one line format:

`<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>`

This will take a GET parameter in the URL and execute it on the system with `shell_exec()`. Essentially, what this means is that any commands we enter in the URL after `?cmd=` will be executed on the system -- be it Windows or Linux. The "pre" elements are to ensure that the results are formatted correctly on the page.

Let's see this in action:

![[Pasted image 20220215014754.png]]

As mentioned previously, there are a variety of webshells available on Kali by default at `/usr/share/webshells` -- including the infamous [PentestMonkey php-reverse-shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) -- a full reverse shell written in PHP. Note that most generic, language specific (e.g. PHP) reverse shells are written for Unix based targets such as Linux webservers. They will not work on Windows by default.

When the target is Windows, it is often easiest to obtain RCE using a web shell, or by using msfvenom to generate a reverse/bind shell in the language of the server. With the former method, obtaining RCE is often done with a URL Encoded Powershell Reverse Shell. This would be copied into the URL as the `cmd` argument:

![[Pasted image 20220215014824.png]]

