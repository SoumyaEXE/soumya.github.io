---
layout: post
title: Hacking in the Cloud - iam_privesc_by_attachment
description: We start off as a fairly high-privileged user who can perform multiple IAM and EC2 API calls. Using these permissions, it was possible to obtain full control over the AWS account by creating an EC2 instance with a high-privileged instance profile. 
date:   2022-11-12
image:  '/images/iam_privesc_by_attachment_web.png'
category: Videos
tags:   [Videos, Cloudgoat, AWS, IAM, EC2]
---

We start off as a fairly high-privileged user who can perform multiple IAM and EC2 API calls. Using these permissions, it was possible to obtain full control over the AWS account by creating an EC2 instance with a high-privileged instance profile. 


<iframe src="https://www.youtube.com/embed/1MS0GPwmVMI" frameborder="0" allowfullscreen></iframe>

[00:00](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=0s) - Video context<br>
[01:07](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=67s) - Configuring profile & AWSealion<br>
[03:31](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=211s) - Enumerating environment<br>
[05:25](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=325s) - Finding privesc pathway<br>
[07:31](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=451s) - Starting EC2 instance creation process<br>
[11:35](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=695s) - Modifying instance profile<br>
[16:53](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=1013s) - Creating EC2 instance<br>
[18:41](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=1121s) - SSH to instance<br>
[19:54](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=1194s) - Configuring AWS CLI<br>
[22:28](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=1348s) - Enumerating "mighty" role permissions<br>
[23:28](https://www.youtube.com/watch?v=wCKTkoXyGYs&t=1408s) - Discussing the misconfiguration & remediation<br>
