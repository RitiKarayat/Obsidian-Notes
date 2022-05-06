# Getting Started
![[Pasted image 20220405101827.png]]

Start by modifying the hosts file with the given command.

`echo "10.10.65.21    overwrite.uploadvulns.thm shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm" | sudo tee -a /etc/hosts`

# Overwriting Existing Files
![[Pasted image 20220405103156.png]]

Just view the source for the filename and upload any image of that name to accomplish.

![[Pasted image 20220405103300.png]]

# RCE
![[Pasted image 20220405105111.png]]

Running a gobuster scan I found /resources/ to be the directory and uploaded a revshell.

![[Pasted image 20220405105126.png]]

# Filtering Types

## Client Side Filtering
When we talk about a script being "Client-Side", in the context of web applications, we mean that it's running in the user's browser as opposed to on the web server itself. JavaScript is pretty much ubiquitous as the client-side scripting language, although alternatives do exist.  Regardless of the language being used, a client-side script will be run in your web browser. In the context of file-uploads, this means that the filtering occurs before the file is even uploaded to the server. 
It is easy to bypass compared to server side filering.

## Server Side Filtering
 Server-side filtering tends to be more difficult to bypass, as you don't have the code in front of you. As the code is executed on the server, in most cases it will also be impossible to bypass the filter completely; instead we have to form a payload which conforms to the filters in place, but still allows us to execute our code.
 
 **_Extension Validation:_**

File extensions are used (in theory) to identify the contents of a file. In practice they are very easy to change, so actually don't mean much; however, MS Windows still uses them to identify file types, although Unix based systems tend to rely on other methods, which we'll cover in a bit. Filters that check for extensions work in one of two ways. They either _blacklist_ extensions (i.e. have a list of extensions which are **not** allowed) or they _whitelist_ extensions (i.e. have a list of extensions which **are** allowed, and reject everything else).
 
 
**_File Type Filtering:_**

Similar to Extension validation, but more intensive, file type filtering looks, once again, to verify that the contents of a file are acceptable to upload. We'll be looking at two types of file type validation:  

-   _MIME validation:_ MIME (**M**ultipurpose **I**nternet **M**ail **E**xtension) types are used as an identifier for files -- originally when transfered as attachments over email, but now also when files are being transferred over HTTP(S). The MIME type for a file upload is attached in the header of the request, and looks something like this:
![[Pasted image 20220407003618.png]]

**_File Length Filtering:_**

File length filters are used to prevent huge files from being uploaded to the server via an upload form (as this can potentially starve the server of resources). In most cases this will not cause us any issues when we upload shells; however, it's worth bearing in mind that if an upload form only expects a very small file to be uploaded, there may be a length filter in place to ensure that the file length requirement is adhered to. As an example, our fully fledged PHP reverse shell from the previous task is 5.4Kb big -- relatively tiny, but if the form expects a maximum of 2Kb then we would need to find an alternative shell to upload.

**_File Name Filtering:_**

As touched upon previously, files uploaded to a server should be unique. Usually this would mean adding a random aspect to the file name, however, an alternative strategy would be to check if a file with the same name already exists on the server, and give the user an error if so. Additionally, file names should be sanitised on upload to ensure that they don't contain any "bad characters", which could potentially cause problems on the file system when uploaded (e.g. null bytes or forward slashes on Linux, as well as control characters such as `;` and potentially unicode characters). What this means for us is that, on a well administered system, our uploaded files are unlikely to have the same name we gave them before uploading, so be aware that you may have to go hunting for your shell in the event that you manage to bypass the content filtering.

**_File Content Filtering:_**

More complicated filtering systems may scan the full contents of an uploaded file to ensure that it's not spoofing its extension, MIME type and Magic Number. This is a significantly more complex process than the majority of basic filtration systems employ, and thus will not be covered in this room.


# Bypassing Client Side Filtering

There are four easy ways to bypass your average client-side file upload filter:

1.  _Turn off Javascript in your browser_ -- this will work provided the site doesn't require Javascript in order to provide basic functionality. If turning off Javascript completely will prevent the site from working at all then one of the other methods would be more desirable; otherwise, this can be an effective way of completely bypassing the client-side filter.
2.  _Intercept and modify the incoming page._ Using Burpsuite, we can intercept the incoming web page and strip out the Javascript filter before it has a chance to run. The process for this will be covered below.
3.  _Intercept and modify the file upload_. Where the previous method works _before_ the webpage is loaded, this method allows the web page to load as normal, but intercepts the file upload after it's already passed (and been accepted by the filter). Again, we will cover the process for using this method in the course of the task.
4.  _Send the file directly to the upload point._ Why use the webpage with the filter, when you can send the file directly using a tool like `curl`? Posting the data directly to the page which contains the code for handling the file upload is another effective method for completely bypassing a client side filter. We will not be covering this method in any real depth in this tutorial, however, the syntax for such a command would look something like this: `curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>`. To use this method you would first aim to intercept a successful upload (using Burpsuite or the browser console) to see the parameters being used in the upload, which can then be slotted into the above command.


Lets start burp and intercept the server response

![](https://i.imgur.com/T0RjAry.png)

![[Pasted image 20220407004328.png]]

Now lets try to upload our reverse shell. First intercept the homepage url and then forward until you get client-side-filter.js request and then intercept its response.

![[Pasted image 20220407005000.png]]

![[Pasted image 20220407005247.png]]

Delete all the js code

![[Pasted image 20220407005412.png]]

Now you can upload your file even if its a php

![[Pasted image 20220407005526.png]]

And we got a reverse shell back to our netcat listener

![[Pasted image 20220407005735.png]]


### Technique 2
Now lets change the shell.php to correct mime type that is shell.jpg
and try to intercept the response and manipulate the mime type after the filtering is done. Change the name to newshell.php and type to text/x-php or application/x-php
and forward the request.

![[Pasted image 20220407010125.png]]

And we got the shell and the flag

![[Pasted image 20220407010346.png]]

**Flag - THM{NDllZDQxNjJjOTE0YWNhZGY3YjljNmE2}**

# Bypassing SSF - File Extensions

We can have server side filters that don't allow files with certain extensions like .php but these can be bypassed using alternative extensions like
`.php3`, `.php4`, `.php5`, `.php7`, `.phps`, `.php-s`, `.pht` and `.phar`. Many of these bypass the filter (which only blocks`.php` and `.phtml`)

If some other extension is accepted like .jpg then we can upload our shell as shell.jpg.php or shell.php.jpg

**Gobuster Scan**
We found /privacy directory which has all the uploaded files.
![[Pasted image 20220409090205.png]]

Lets send this request to the intruder with the following list of alternative extensions and see which of them gets uploaded.

I am using sniper with this payload list - 

![[Pasted image 20220409090426.png]]

Now we can visit the privacy directory and find the uploaded shells

![[Pasted image 20220409090516.png]]

Lets try connecting to each one of them and setting a netcat listener at port 4444

![[Pasted image 20220409090734.png]]

We got reverse shell for the **.php5** file and hence captured the flag in /var/www.

**Flag  -  THM{MGEyYzJiYmI3ODIyM2FlNTNkNjZjYjFl}**

# Bypassing SSF -  Magic Numbers
Some servers check the filetype not by extensions but by magic numbers which are nothing but the first few hexadecimal numbers in the file to confirm the filetype.
The linux command **file** also works in the same way.

A quick look at the [list of file signatures on Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures) shows us that there are several possible magic numbers of JPEG files.

![[Pasted image 20220409092333.png]]

![[Pasted image 20220409092406.png]]

**Gobuster Scan**
![[Pasted image 20220409092113.png]]

/graphics dir is found. 

I found that only GIFs are allowed. Lets change our shell to a GIF using hexeditor or nano and inserting any of the following
In hexeditor - 
`47 49 46 38 37 61`  
`47 49 46 38 39 61`
In nano - 
`GIF87a`  
`GIF89a`

Since indexing is off on the server we can access the file directly at /graphics/newshell.php/

![[Pasted image 20220409092653.png]]

Hence we captured the flag.

**Flag - THM{MWY5ZGU4NzE0ZDlhNjE1NGM4ZThjZDJh}**
