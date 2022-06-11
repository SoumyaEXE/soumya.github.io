---
layout: post
title: Hacking in the Cloud - vulnerable_lambda
description: This is the first scenario in the CloudGoat series. We start off as a low-privileged user that can assume a role which gives Lambda:Invoke permissions. Using this permission we are able to exploit a high-privileged Lambda function via an SQL injection and obtain Administrator access.
date:   2022-06-10 
image:  '/images/vulnerable_lambda.png'
category: Video
tags:   [Video, SQLi, Cloudgoat, AWS]
---

This is the first scenario in the CloudGoat series. We start off as a low-privileged user that can assume a role which gives Lambda:Invoke permissions. Using this permission we are able to exploit a high-privileged Lambda function via an SQL injection and obtain Administrator access.

<iframe src="https://www.youtube.com/embed/EhR3yvyqTWE" frameborder="0" allowfullscreen></iframe>
<br>
[00:00](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=0s) - Video context<br>
[00:57](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=57s) - Enumerating IAM roles and policies<br>
[06:53](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=413s) - Assuming lambda role <br>
[08:53](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=533s) - Further enumeration of IAM roles <br>
[14:51](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=891s) - Analyzing vulnerable lambda's source code <br>
[18:19](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=1099s) - Exploiting lambda function <br>
[24:17](https://www.youtube.com/watch?v=EhR3yvyqTWE&t=1457s) - Demonstrating SQLi<br>
