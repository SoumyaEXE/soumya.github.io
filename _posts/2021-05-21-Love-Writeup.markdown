---
layout: post
title:  Love Writeup
description: This Windows box dealt with exploiting an SSRF vulnerability which allowed for the viewing of a sensitive webpage hosted internally on the target. After exploting a vulnerable version of “voting system” software, a shell as a low-privileged user was returned. Finally, by taking advantage of an HKLM misconfiguration, a shell as the SYSTEM user could be obtained (namely by installing a malicious MSI package).
date:   2021-08-22 
image:  '/images/0xd4y-logo-gray.png'
tags:   [Windows, SSRF, CVE, HKLM, MSI]
---

**This report can be read both on this site, and as its <a href = "https://zezul.github.io/reports/Writer%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**

Love

Exploitation of misconfigurations and insecure code

   ![](/images/0xd4y-logo-gray.png)

0xd4y

May 25, 2021

0xd4y Writeups

LinkedIn: [https://www.linkedin.com/in/segev-eliezer/](https://www.google.com/url?q=https://www.linkedin.com/in/segev-eliezer/&sa=D&source=editors&ust=1653797783689836&usg=AOvVaw3EtrlB0O5YhUWzWJZPfHF9) 

Email: [0xd4yWriteups@gmail.com](mailto:0xd4yWriteups@gmail.com)

Web: [https://0xd4y.github.io/](https://www.google.com/url?q=https://0xd4y.github.io/Writeups/&sa=D&source=editors&ust=1653797783690997&usg=AOvVaw0Mi5KIOQSTdiZuqNjkSogk) 

Table of Contents

[Executive Summary](#h.ip86uerwz5x0)        [2](#h.ip86uerwz5x0)

[Attack Narrative](#h.50z82q6r7ne3)        [3](#h.50z82q6r7ne3)

[Enumeration](#h.wblb7nvo4vdk)        [3](#h.wblb7nvo4vdk)

[Port Enumeration](#h.msuhfxc2c77t)        [3](#h.msuhfxc2c77t)

[HTTP Enumeration](#h.19by0voy47mg)        [5](#h.19by0voy47mg)

[SQL Injection (SQLi)](#h.bn9tu0cgpzid)        [6](#h.bn9tu0cgpzid)

[Admin Page](#h.4wslswsfx98j)        [9](#h.4wslswsfx98j)

[HTTPS Enumeration](#h.hwesglv7g5dx)        [11](#h.hwesglv7g5dx)

[Abusing beta.php](#h.u9wa319m4pff)        [11](#h.u9wa319m4pff)

[Reverse Shell](#h.627bknqnzbfu)        [15](#h.627bknqnzbfu)

[Privilege Escalation](#h.dmxvrwtbagc2)        [17](#h.dmxvrwtbagc2)

[Post Exploitation Analysis](#h.k14bgz5rbsj)        [19](#h.k14bgz5rbsj)

[SQL Injection](#h.t6wmor2q64ua)        [19](#h.t6wmor2q64ua)

[Beta.php Vulnerability](#h.rjqnrqcebcxi)        [19](#h.rjqnrqcebcxi)

[Conclusion](#h.1dlgk2gf7eqr)        [21](#h.1dlgk2gf7eqr)

Executive Summary
=================

No prior information was provided for this penetration test except for the IP of the vulnerable machine. This system contains multiple critical vulnerabilities. Along with an SQL injection vulnerability in the root page of the HTTP service, there is an insecure file scanner function within the HTTPS service which was responsible for the leakage of plaintext admin credentials. Additionally, a vulnerable version of Voting System software was installed which allowed for an easy route to returning a reverse shell.

After gaining the reverse shell, the box had a misconfigured group policy (AlwaysInstallElevated) which authorized the installation of packages as SYSTEM. The disabling of antivirus software on this machine facilitated the process of obtaining system privileges.

Attack Narrative
================

Enumeration
-----------

To determine the presence of a possible attack vector, it is essential to begin by enumerating the ports of the box.

### Port Enumeration

Along with enumerating open ports, their services and their versions are also examined using the \-sC (for default scripts) and \-sV (enumerate version) flags.

{% highlight bash %}
Nmap scan report for 10.10.10.239

Host is up (0.065s latency).

Not shown: 993 closed ports

PORT     STATE SERVICE      VERSION

80/tcp   open  http         Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1j PHP/7.3.27)

| http\-cookie\-flags:

|   /:

|     PHPSESSID:

|_      httponly flag not set

| http\-methods:

|_  Supported Methods: GET HEAD POST OPTIONS

|_http\-server\-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27

|_http\-title: Voting System using PHP

135/tcp  open  msrpc        Microsoft Windows RPC

139/tcp  open  netbios\-ssn  Microsoft Windows netbios\-ssn

443/tcp  open  ssl/http     Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)

|_http\-server\-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27

|_http\-title: 403 Forbidden

| ssl\-cert: Subject: commonName\=staging.love.htb/organizationName\=ValentineCorp/stateOrProvinceName\=m/countryName\=in

| Issuer: commonName\=staging.love.htb/organizationName\=ValentineCorp/stateOrProvinceName\=m/countryName\=in

| Public Key type: rsa

| Public Key bits: 2048

| Signature Algorithm: sha256WithRSAEncryption

| Not valid before: 2021\-01\-18T14:00:16

| Not valid after:  2022\-01\-18T14:00:16

| MD5:   bff0 1add 5048 afc8 b3cf 7140 6e68 5ff6

|_SHA\-1: 83ed 29c4 70f6 4036 a6f4 2d4d 4cf6 18a2 e9e4 96c2

|_ssl\-date: TLS randomness does not represent time

| tls\-alpn:

|_  http/1.1

445/tcp  open  microsoft\-ds Windows 10 Pro 19042 microsoft\-ds (workgroup: WORKGROUP)

3306/tcp open  mysql?

| fingerprint\-strings:

|   DNSVersionBindReqTCP, Help, JavaRMI, LDAPBindReq, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, TerminalServer, afp, ms\-sql\-s:

|_    Host '10.10.14.138' is not allowed to connect to this MariaDB server

5000/tcp open  http         Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)

|_http\-server\-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27

|_http\-title: 403 Forbidden

Host script results:

|_clock\-skew: mean: 2h52m30s, deviation: 4h02m31s, median: 32m28s

| smb\-os\-discovery:

|   OS: Windows 10 Pro 19042 (Windows 10 Pro 6.3)

|   OS CPE: cpe:/o:microsoft:windows_10::-

|   Computer name: Love

|   NetBIOS computer name: LOVE\\x00

|   Workgroup: WORKGROUP\\x00

|_  System time: 2021\-05\-07T14:50:29\-07:00

| smb\-security\-mode:

|   account_used: guest

|   authentication_level: user

|   challenge_response: supported

|_  message_signing: disabled (dangerous, but default)

| smb2\-security\-mode:

|   2.02:

|_    Message signing enabled but not required

| smb2\-time:

|   date: 2021\-05\-07T21:50:28

|_  start_date: N/A
{% endhighlight %}

Nmap detected this as a Windows box from the SMB service. Additionally, there are two HTTP services open: one on port 80, and a peculiar one on port 5000. Upon attempting to access the HTTP service on port 5000, the scan was met with a 403 error. Interestingly, this box is running Apache which is uncommon for the Windows operating system (this box is running Windows 10 pro 19042 which was detected through SMB). On port 443 there is an HTTPS service whose certificate leaks the domain name of staging.love.htb. Additionally, there is a mysql service, but remote connections are disabled.

The services running on each port do not appear to be outdated, and there are most likely no CVEs to take advantage of. Therefore, the penetration test will start by accessing the HTTP page, as web services tend to have a bigger attack surface than other services.

### HTTP Enumeration

Visiting the page on 10.10.10.239, the server responds with a simple login page.

![](/reports/Love/image13.png)

Attempting to login with common default credentials does not work:

![](/reports/Love/image10.png)

However, a useful error message pops up that says “Cannot find voter with the ID”. Accordingly, it may be viable to attain usernames by brute forcing ID’s.

#### SQL Injection (SQLi)

A common vulnerability among login pages is SQLi, so it makes sense to attempt this on the webpage:

![](/reports/Love/image12.png) 

Upon inputting a SQL query into the username field, an “Incorrect password” message pops up instead of “Cannot find voter with the ID”. Judging from this output, it is likely that this webpage is vulnerable to SQLi.
