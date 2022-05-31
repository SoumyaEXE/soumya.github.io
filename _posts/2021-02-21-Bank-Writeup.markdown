---
layout: post
title:  Bank Writeup
description: This is a really cool box that has a couple of interesting twists. I go through the unintended solution (the way I went about the machine) and the intended solution. There is a lot to learn here about web security and networking, and I highly encourage you to read this writeup.
date:   2021-02-21 
image:  '/reports/Bank/image5.png'
tags:   [Open Redirect, RCE]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.github.io/reports/Bank%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

![](/reports/Bank/image3.png)

Never underestimate the power of BurpSuite! In this box, I will go through the unintended and intended solution. There is a lot to learn in the unintended solution, and this is actually how I went about rooting the box. After I show you the unintended solution, I will demonstrate how the author of the box, @makelarisjr, intended to go about rooting the machine.

As always, we will enumerate the ports with nmap -sC -sV oA nmap/nmap 10.10.10.29

![](/reports/Bank/image7.png)

Immediately, something unusual strikes us. Port 53, the port for DNS, is running on the TCP protocol. Typically, this runs on UDP, because UDP is faster and DNS does not need to handle large requests. The only reason why this port would be run on TCP is perhaps for zone transferring.

![](/reports/Bank/image26.png)

Now as a habit, before I go onto trying to get root, I like to add the ip of the box to my /etc/hosts file. That way, I don’t have to memorize the ip of the machine.

![](/reports/Bank/image18.png)

Before we go to the web server, let’s enumerate the subdomains with the dig command.

![](/reports/Bank/image12.png)

The AXFR protocol is typically used in conjunction with TCP. Cool! Let’s add the subdomains to our /etc/hosts file.

![](/reports/Bank/image33.png)

Unfortunately, all of these subdomains (except for bank.htb) seem to be a rabbit hole. When I visited chris.bank.htb, ns.bank.htb, and www.bank.htb, I was met with the following default ubuntu page:

![](/reports/Bank/image30.png)

This same page appeared when I went to 10.10.10.29. Running a gobuster scan on all of these pages didn’t reveal anything interesting. However, bank.htb presented me with this login page:

![](/reports/Bank/image10.png)

I tried to see if this login page was vulnerable to SQL-injection, but I couldn’t find any vulnerabilities. After enumerating the directories with gobuster, I found the following::

![](/reports/Bank/image28.png)

The 302 status code is a redirection. This is why when I visited /support.php, I was redirected to /login.php. However, let’s see if this redirection was configured properly. A quick test for this is to curl the directory and pipe it to wc -c.

curl http://bank.htb/support.php|wc -c

We see that the response returns 3861 characters. This is very odd, as typically redirect pages don’t contain more than a couple hundred characters. Let’s see what happens if we look at this through burp. Make sure to intercept server responses in the “Options” tab under “Proxy”.

![](/reports/Bank/image1.png)

Let’s intercept our request when going on http://bank.htb/support.php

![](/reports/Bank/image4.png)

After forwarding this request, we intercept the server response which is quite long with html tags, indicating that it is displaying a webpage.

![](/reports/Bank/image31.png)

As expected, we get a 302 Found response. Note how we can view the source of /support.php. As you can probably deduce, this is a vulnerability in itself. This means that we don’t need to authenticate in order to view this page. Changing 302 Found to 200 OK and forwarding this request, we can now interact with the support.php page!

![](/reports/Bank/image13.png)

Here you can try to upload an image file, php file, or whatever you’d like. As it turns out, the webpage only allows image files. Eventually, you’ll find that you cannot upload a reverse shell (due to file restrictions) without knowing a particular secret. That secret lies in a comment embedded in the source code (always check the source code!):

![](/reports/Bank/image16.png)

Uploading a php reverse shell with the extension of htb instead of php should do the trick! I used the reverse shell from pentestmonkey which you can find by visiting the following link: [https://github.com/pentestmonkey/php-reverse-shell](https://www.google.com/url?q=https://github.com/pentestmonkey/php-reverse-shell&sa=D&source=editors&ust=1653958332694174&usg=AOvVaw0gEEdKo_pDQeFwuDAFt-yg)

Changing the php-reverse-shell.php to php-reverse-shell.htb allows us to upload the shell successfully! Visiting [http://bank.htb/uploads/reverse\_shell.htb](https://www.google.com/url?q=http://bank.htb/uploads/reverse_shell.htb&sa=D&source=editors&ust=1653958332694734&usg=AOvVaw1wapbxBfFbOzX3EwJx7SwX) gets us a reverse shell!

![](/reports/Bank/image11.png)

![](/reports/Bank/image19.png)

As www-data, we can read the user.txt file in /home/chris/. Now let’s get root.txt

The first thing I always do when getting a reverse shell is sudo -l and groups. Nothing was out of the ordinary, so I went to do the next thing that I always do. I set up a python http server on my machine hosting linpeas.sh, then on the reverse shell I went to the /tmp directory, downloaded the linpeas.sh file, and ran it (you can find linpeas using the following link: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite](https://www.google.com/url?q=https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite&sa=D&source=editors&ust=1653958332695769&usg=AOvVaw1MOcd4iGwIozIAy_2spgF6)). After running linpeas, the first thing I do is skim the output for anything that is highlighted in yellow with red text (this indicates a high chance of a privilege escalation vector). If linpeas doesn’t find anything completely out of the ordinary, I look through the output more closely. For this box, there was no need to look any further as a critical vulnerability was found:

![](/reports/Bank/image29.png)

We have write access to /etc/passwd! If we create a user with id 0 in the /etc/passwd file, then we will be root. The first thing to do is to create a unix crypt password (the password hash used for linux systems), and put this in the /etc/passwd file corresponding to a user that we create.

![](/reports/Bank/image25.png)

This is the /etc/passwd file before editing:

![](/reports/Bank/image32.png)

This is the /etc/passwd file after editing:

![](/reports/Bank/image34.png)

Now we can su to 0xd4y (the user that we created) with the password of password.

![](/reports/Bank/image27.png)

Incidentally, I could have also just changed the root password by replacing the “x”of the root entry with the password hash. This works because /etc/passwd takes precedence over /etc/shadow. Additionally, adding a new user with id 0 also works because Unix systems identify users by their id, not by their username. This was a very fun box and really highlighted the power of BurpSuite. However, as it turns out, this way of rooting the box was unintended! So let’s go through the way that the author intended.

Intended Solution

As it turns out, when running a gobuster scan on [http://bank.htb](https://www.google.com/url?q=http://bank.htb&sa=D&source=editors&ust=1653958332697464&usg=AOvVaw1HXJjloI3LGver3kWPOIXQ), there was a directory /balance-transfer/ (my scan did not find this and I have no idea what wordlist would even have this entry). Visiting the page we are just flooded with files:

![](/reports/Bank/image22.png)

Clicking on one of the files reveals the following:

![](/reports/Bank/image8.png)

I tried base64 decoding these parameters, but it was just a nonsensical output due to the encryption. So, I recursively downloaded all the files in this directory with wget -r [http://bank.htb/balance-transfer/](https://www.google.com/url?q=http://bank.htb/balance-transfer/&sa=D&source=editors&ust=1653958332698299&usg=AOvVaw1es7j4P8cUpJhLx9QTm6AE) in order to inspect these files further on my machine. I assumed since this is a CTF, there is probably a file in this big mess that is different. I checked the Full Name parameter of each file to see if there was something different in one of the files using grep Name -r .

This command lists the Full Name parameter in all the files in the directory. After doing this I noticed a pattern. Each encrypted full name is 128 characters in length. I isolated each full name by using the tr command.

![](/reports/Bank/image9.png)

Now we have Full Name, the encrypted full name, and the file path on different lines. We are only interested in the encrypted name and the file path for now, so let’s do a grep -v on “Full Name”. So far our entire command looks like this: grep Name -r .|tr ":" "\\n"|grep -v Name 

Now we have a bunch of strings that are 128 characters in length, so let’s do a grep -v on any string that’s more than 120 characters (I arbitrarily chose this number, you could have chosen whatever number suits you as long as it isn’t too small and not more than 128). After grepping out the 128 character length strings, my terminal was now only flooded by the path of each file:

![](/reports/Bank/image2.png)

Finally, let’s do a grep -v on “acc” to get rid of the flooding of those file paths. After doing all of this we get the following output:

![](/reports/Bank/image17.png)

Interesting! There must be a file somewhere in this directory with the full name of Chris Christopoulos. Let’s grep for this specific string in the directory to find the file containing this string.

![](/reports/Bank/image6.png)

![](/reports/Bank/image23.png)

These aren’t the ssh credentials for chris, so let’s use these credentials on /login.php.

![](/reports/Bank/image20.png) 

We are greeted with a nice dashboard and an interesting balance. There is nothing out of the ordinary to be seen here. The real vulnerability lies in the support.php directory, so let’s visit that.

![](/reports/Bank/image15.png)

Well, that looks a lot nicer than what we saw in Burp. I will skip the methodology of getting the reverse shell, as I already covered this in the unintended solution above.

A very common way to escalate privileges is by abusing setuid binaries. This can be viewed using the find / -perm -4000 2>/dev/null  command. Using this, we find the following binaries:

![](/reports/Bank/image24.png)

One setuid binary in particular stands out: /var/htb/bin/emergency. Let’s just run it and see what happens:

![](/reports/Bank/image14.png)

![](/reports/Bank/image21.png)

Bonus

When I was first doing this box, I actually did not know about the trick of intercepting a server response and modifying it to allow unauthorized access to a page. Instead, I curled [http://bank.htb/support.php](https://www.google.com/url?q=http://bank.htb/support.php&sa=D&source=editors&ust=1653958332701455&usg=AOvVaw3mPbt5yUpdAMfy7vs2jdXx) to find all the parameters needed to create a python script which automatically uploads a file of your choice. You can check out the script here: [https://github.com/0xd4y/WriteUps/blob/gh-pages/HackTheBox/bank\_upload.py](https://www.google.com/url?q=https://github.com/0xd4y/WriteUps/blob/gh-pages/HackTheBox/bank_upload.py&sa=D&source=editors&ust=1653958332701929&usg=AOvVaw2vwKphjcxIghPRvkCWoMPw)

Thanks for reading this writeup! I hope you learned something new. Have a wonderful day and best of luck on your journey in cybersecurity!
