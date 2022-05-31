---
layout: post
title:  PythonPlayground Writeup
description: Python has a lot of features that some people may not know about. In this challenge, it was important to be creative throughout each step of exploitation and piece each compromised puzzle piece together.
date:   2021-03-26
image:  '/images/0xd4y-logo-gray.png'
tags:   [Python, PATH Injection, Logic]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.github.io/reports/PythonPlayground%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

![](/reports/PythonPlayground/image18.png)

![](/reports/PythonPlayground/image16.png)

As the name suggests, this box was all about using python to exploit its vulnerabilities. Each part of this box was like a puzzle piece which, when connected together, gave you the ability to escalate to root. Let’s get right into the box, as I’ll go into detail about each aspect of exploiting this box’s vulnerabilities.

RECON

As usual, I will start by scanning the ports with nmap:

![](/reports/PythonPlayground/image10.png)

Looks like only http and ssh are open. There is not much information from this nmap scan, other than that we know the box is running Ubuntu.

![](/reports/PythonPlayground/image27.png)

Clicking on Login or Sign up doesn’t lead to anything interesting, as we are just met with this page:

![](/reports/PythonPlayground/image5.png)

It looks like each web page is appended with .html, so let’s run a gobuster on the root page with the extension .html:

![](/reports/PythonPlayground/image22.png)

The /admin.html directory particularly stands out, so let’s check it out.

Getting Credentials

![](/reports/PythonPlayground/image2.png)

Immediately from the name of this login form, it looks like there is probably a user named Connor. It’s good practice to check out the source code to see if there are any interesting comments or links to other directories on the website.

![](/reports/PythonPlayground/image26.png)

![](/reports/PythonPlayground/image23.png)

So we can see from the code above that the login form takes a password and “hashes it”. I put “hash” in quotes because this code does not actually hash an input. In an actual password hash, the output does not give information about the input. For example, in mathematical operation of addition we know that 1+1=2. However, let’s say I added two numbers together without telling you which numbers I added, and all I told you was that the sum of those two numbers is equal to 2. Then I asked you, “Which two numbers did I add to get the number 2?”. It could be 1+1, or 2+0, or maybe even 1.7+0.3. The possibilities are endless.

The problem with the code on the webpage, is that it leaks the hash to us as well as how the hash was produced, but more importantly, the length of the hash is dependent on the length of the input. This means that data is not lost during the hashing process. All of this considered, we can reverse the hash to get the password. A very simple way to do this is by taking the javascript code that we saw in the source of the webpage, and inputting all characters in the order determined by the ascii table. Using this, we can see the output that the hash creates for each letter (remember about how this code does not lose inputted data)?

![](/reports/PythonPlayground/image15.png)

Using this method, our code does not need a lot of lines. You can view my script [here](https://www.google.com/url?q=https://github.com/0xd4y/Writeups/blob/gh-pages/TryHackMe/crack.py&sa=D&source=editors&ust=1653953672142990&usg=AOvVaw17cVA7TMh7fjsB-E1XDH-f) if you’d like. I have since edited it to be more user-friendly.

Incidentally, even if we did not know the code of the hash, we can see that the length of the output is always four times the length of the input (because data is not lost).

![](/reports/PythonPlayground/image6.png)

Anyways, let’s try the cracking function!

![](/reports/PythonPlayground/image20.png)

And we get Connor’s password as spaghetti1245. Inputting the username connor and the password, we get redirected to /super-secret-admin-testing-panel.html.

It turns out that we could have just gone to this page without even needing Connor’s password:

![](/reports/PythonPlayground/image11.png)

I did not realize this the first time going through the box. Looks like authenticated cookies are not needed to view this site.

![](/reports/PythonPlayground/image25.png)

Typing python code into the text field, we see that this form runs our code. So let’s try a python reverse shell:

![](/reports/PythonPlayground/image12.png)

Unfortunately, this does not work as there is some sort of blacklist on the import keyword. I tried bypassing this by writing ‘imp’ to a file and then appending ‘ort’ to it. Finally, I could try executing it with the exec function:

![](/reports/PythonPlayground/image8.png)

Unfortunately, the exec function is blacklisted as well. However, if you have read my [Develpy Writeup](https://www.google.com/url?q=https://0xd4y.github.io/Writeups/TryHackMe/Develpy%2520Writeup.pdf&sa=D&source=editors&ust=1653953672145946&usg=AOvVaw2ZXMcxIegK7B_X2o89fk8o), then you know there is one more thing we have left to try:

![](/reports/PythonPlayground/image13.png)

Using the __import__ keyword, we can circumvent the blacklist.

![](/reports/PythonPlayground/image1.png)

Awesome! We’re root. But there’s a catch:

![](/reports/PythonPlayground/image14.png)

Looks like we are in a docker container :(. Well, let’s just get the first flag.

![](/reports/PythonPlayground/image21.png)

We can also ssh into the box using the credentials for Connor that we saw earlier.

![](/reports/PythonPlayground/image24.png)

ROOT PRIVESC

So we have a reverse shell inside a docker container and we are in the actual box through an ssh session, but how are we going to get root? I ran the [linPEAS](https://www.google.com/url?q=https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS&sa=D&source=editors&ust=1653953672147938&usg=AOvVaw3oP6L3vG7KcGjXT1fkgu0Z) privilege escalation enumeration script on the ssh session, but it did not find anything out of the ordinary. This part of rooting the box is really cool and is when we piece together our shells. Let’s run linPEAS on the reverse shell and see if we can find anyway to escape it and get root:

![](/reports/PythonPlayground/image17.png)

Above we can see that the log directory is mounted on the docker container. Using this mount, we can interact directly with the host system. It’s important to note that due to us being root in the docker container, we can make files on /var/log as root. We can create a setuid binary by compiling the following code and giving it setuid permissions:

![](/reports/PythonPlayground/image19.png)

Unfortunately, there is no way to download this using wget or curl as it is not on the docker container. However, in the theme of this box, we can download files using python with the urllib.request module:

![](/reports/PythonPlayground/image4.png)

![](/reports/PythonPlayground/image9.png)

![](/reports/PythonPlayground/image3.png)

And we are root!

BONUS

![](/reports/PythonPlayground/image7.png)

Here we can see all the blacklisted strings. Note how the script checks for “import “ rather than “import”. This seemingly unimportant space makes the difference between allowing __import__ and not. Changing this makes the program invulnerable (as far as I know). Thank you to @[deltatemporal](https://www.google.com/url?q=https://tryhackme.com/p/deltatemporal&sa=D&source=editors&ust=1653953672150496&usg=AOvVaw3HgV3Kx9FkxcmFnlH1YhHs) for the great challenge and cool privesc! Thank you to you as well for reading this writeup!
