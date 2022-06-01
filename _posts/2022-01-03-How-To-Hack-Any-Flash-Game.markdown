---
layout: post
title:  Writer Writeup
description: This system contained an SQL injection vulnerability which could be leveraged to not only log into an application with admin privileges, but also could be used to read local files on the target. After leaking the source code of the website, an insecure usage of handling files was exploited to get RCE. With a www-data shell on the system, an insecure password of a local user located in the SQL database was cracked. Eventually the system was fully compromised through misconfigurations relating to SMTP and APT.
date:   2021-08-22 
image:  '/images/Writer.png'
tags:   [Video, SMTP, RCE, Flash]
---

This video showcases how to hack any flash game using BurpSuite and JPEXS Decompiler. Note that the information showed in this video should not be used for any illegal purposes.

<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=0s">00:00</a> - Video context
<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=49s">00:49</a> - Tools used for the hack
<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=142s">02:22</a> - Swords and Sandals 2 before hack
<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=304s">05:04</a> - Clearing cache and downloading SWF file
<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=528s">08:48</a> - Decompiling SWF file and modifying the code
<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=1022s">17:02</a> - Serving the modified SWF file to the server response
<a href="https://www.youtube.com/watch?v=bnSUBy9YrOc&t=1069s">17:49</a> - Playing our hacked version of Swords and Sandals 2
