---
layout: post
title:  Develpy Writeup
description: This box had a fun foothold, and it was a nice little challenge that showed the importance of making your programs secure. I highly recommend you read this writeup, as after exploiting the program, I go over how to patch it to make it more secure.
date:   2021-03-21 
image:  '/images/python.png'
tags:   [SQLi, SMTP, RCE, APT]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.com/reports/Develpy%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

![](/reports/Develpy/image28.png)

This was a nice little challenge created by @stuxnet. This box had quite a unique foothold, in the sense that it was made purely for challenging your knowledge about the security of python scripting. A lot of programmers do not think about security when they make their programs, rather they think about just getting their program to perform a desired function. A large part about cybersecurity is about finding bugs in programs and exploiting them to make them do functions that they were not designed to do. Let’s jump right in, and I will go into detail about security related to python scripting.

Reconnaissance

As usual, I will start with an nmap scan:

![](/reports/Develpy/image19.png)

We see there are two ports open: port 22 (ssh) and port 10000 (snet-sensor-mgmt?). As can be seen from the output, nmap is not sure what service is running on port 10000, but apparently it is getting some HTTP output. Let’s visit the webpage and see what we get:

![](/reports/Develpy/image26.png)

Getting User

We get a strange response that looks like an error in some python script. It is especially interesting that the name ‘GET’ is not defined. This looks a lot like a result of a GET request.. Let’s fire up burpsuite and verify this. Refreshing the page and intercepting our request we see the following:

![](/reports/Develpy/image2.png)

This is a very standard GET request. In the output we see:

![](/reports/Develpy/image10.png)

So it looks like this script is looking at our request method and inputting it into the script. The program most likely expects an integer as the input, so let’s modify our request so that we can see how the script is designed to behave:

![](/reports/Develpy/image22.png)

And we see the following response:

![](/reports/Develpy/image11.png)

Up to this point with the information that we have gathered about how this script works, it is likely that the script is performing some kind of evaluation on our input. One more thing I checked before I tried to input malicious code is to confirm that the script evaluates what we send. In our previous request, instead of inputting the number 4, let’s input 0:

![](/reports/Develpy/image25.png)

Now we don’t see the strings beginning with “Exploiting tryhackme”. So let’s change 0 to 0+1 to see if the python script is evaluating the math:

![](/reports/Develpy/image18.png)

Notice how now there is one string beginning with “Exploiting tryhackme”. This indicates that the script calculated 0+1. So we have confirmed that the script is using some kind of evaluating function. Let’s input some malicious python code so that we can get a reverse shell. We can import the os module to execute system commands:

![](/reports/Develpy/image16.png)

![](/reports/Develpy/image5.png)

Looks like we can execute commands as the user king. So let’s get a reverse shell:

![](/reports/Develpy/image12.png)

![](/reports/Develpy/image14.png)

![](/reports/Develpy/image9.png)

Privilege Escalation to Root

Cool! We got a shell. Let’s see what is in king’s directory:

![](/reports/Develpy/image3.png)

We see some interesting files, but the root.sh file especially stands out. It is one of the only files in king’s directory that is owned by root. Looking at the contents of the bash script, we see that it only has one line:

![](/reports/Develpy/image20.png)

First of all, this looks like a strange thing to have in a bash script. Why not just type that command in the first place? It seems likely that there is some cronjob running this script. Second of all, python is being run without specifying it’s full path! If there is indeed a cronjob, then it should only be running commands that are specified within a full path. Unfortunately however, we do not have write access to the python binary, and it is likely that root’s path is set to its default. So, it looks like we probably can’t do some sort of path privilege escalation. In /etc/crontab we see the following:

![](/reports/Develpy/image27.png)

Tthere are definitely cron jobs running on this system. At this point I downloaded [pspy](https://www.google.com/url?q=https://github.com/DominicBreuker/pspy&sa=D&source=editors&ust=1653961217842478&usg=AOvVaw3Xy4wBS6ssPN4XQD4bXbER) and saw the following interesting output when I ran it:

![](/reports/Develpy/image23.png)

It looks like there might be some service running on port 8080 that is only open to localhost. Running netstat, we can confirm this:

![](/reports/Develpy/image15.png)We can forward this port using socat so that it would be remotely accessible:

![](/reports/Develpy/image24.png)

![](/reports/Develpy/image13.png)

We see this is a web server that has an upload feature.

![](/reports/Develpy/image6.png)

Let’s just touch a file with the extension .py and see what happens:

![](/reports/Develpy/image1.png)

The file was uploaded to a directory called /media. Remember the root.sh script which ran any file within the /media directory? We can confirm that the server ran this script by looking at pspy:

![](/reports/Develpy/image7.png)

Let’s grab a python reverse shell from [pentest monkey](https://www.google.com/url?q=http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet&sa=D&source=editors&ust=1653961217844668&usg=AOvVaw3T0s2eu-ixwvfoqmeVXd6_) and put it in our script:

![](/reports/Develpy/image17.png)

After uploading, we wait a bit for the cronjob to run again, and eventually we will get a hit back:

![](/reports/Develpy/image4.png)

Bonus

That was a lot of fun, but let’s see how and why we were able to get a foothold on this box. We’ll start by inspecting the script that was related to the error on the webpage:

![](/reports/Develpy/image21.png)

The old version of python (python2) has a dangerous function called input which evaluates the input of a user. This is a security risk within the function, and it is strongly discouraged from being used. Instead, python encourages the use of raw\_input which treats the user’s input as a string regardless of how the user formats it (this is in contrast to the input function which does not modify the type of the input). Modifying the script to use raw\_input instead of input successfully patches the python injection vulnerability. Note that python3 has since modified the input function to behave like raw\_input so as to patch this vulnerability.

Thank you to @stuxnet for such a fun challenge! I hope you learned a bit from my red team analysis of the box and hopefully a bit of the blue team as well!
