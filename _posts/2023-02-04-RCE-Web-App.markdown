---
layout: post
title:  Hacking in the Cloud - rce_web_app
description: The objective of this scenario was to gain access to an RDS instance. We were provided with the credentials of two different users, and exploited this AWS environment in two different ways.
date:   2023-02-04 
image:  '/images/rce_web_app_web.png'
category: Videos
tags:   [Videos, Cloudgoat, Walkthrough, EC2, RDS, AWS]
---

The objective of this scenario was to gain access to an RDS instance. We were provided with the credentials of two different users.

The McDuck user had access to an S3 bucket that contained an SSH private key with which we could connect to the EC2 instance. The EC2 instance then has access to an S3 bucket with the credentials for the RDS instance.

The Lara user has access to an S3 bucket with logs for an ELB within the workload. This allows us to find a hidden directory within the application that contains an RCE vulnerability, thus allowing us to gain access to the EC2 instance.

<iframe src="https://www.youtube.com/embed/Izs7BBDMmv0" frameborder="0" allowfullscreen></iframe>

[00:00](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=0s) - Video Context<br>
[01:12](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=72s) - Configuring AWSealion and users<br>
[02:38](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=158s) - McDuck - Enumeration<br>
[04:35](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=275s) - McDuck - Finding S3 permission<br>
[06:41](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=401s) - McDuck - SSH into instance<br>
[09:09](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=549s) - McDuck - Finding RDS instance<br>
[11:12](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=672s) - McDuck - Accessing RDS instance<br>
[13:45](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=825s) - Lara - Enumeration<br>
[14:57](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=897s) - Lara - Finding logs and discovering ELB<br>
[17:38](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=1058s) - Lara - Accessing webapp<br>
[18:56](https://www.youtube.com/watch?v=Izs7BBDMmv0&t=1136s) - Lara - Gaining RCE and getting shel<br>


