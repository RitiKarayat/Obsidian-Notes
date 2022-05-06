Multi/Handler is a superb tool for catching reverse shells. It's essential if you want to use Meterpreter shells, and is the go-to when using staged payloads.

Fortunately, it's relatively easy to use:

1.  Open Metasploit with `msfconsole`
2.  Type `use multi/handler`, and press enter

We are now primed to start a multi/handler session. Let's take a look at the available options using the `options` command:

![[Pasted image 20220215013634.png]]

We should now be ready to start the listener!

Let's do this by using the `exploit -j` command. This tells Metasploit to launch the module, running as a **j**ob in the background.


![[Pasted image 20220215013723.png]]

![[Pasted image 20220215013752.png]]
