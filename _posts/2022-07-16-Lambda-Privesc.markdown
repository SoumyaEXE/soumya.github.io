---
layout: post
title: Hacking in the Cloud - lambda_privesc
description: We start off as a low-privileged user who can perform IAM Get and IAM List on all resources. In addition, this user can assume a role which has lambda:* and iam:PassRole on all resources. Using this permission, it was possible to create a function with another role that had AdministratorAccess attached to it. Therefore, we were able to attach AdministratorAccess on the low-privileged user.
date:   2022-07-16
image:  '/images/lambda_privesc.png'
category: Videos
tags:   [Videos, Lambda, Cloudgoat, AWS]
---

We start off as a low-privileged user who can perform IAM Get and IAM List on all resources. In addition, this user can assume a role which has lambda:* and iam:PassRole on all resources. Using this permission, it was possible to create a function with another role that had AdministratorAccess attached to it. Therefore, we were able to attach AdministratorAccess on the low-privileged user.

<iframe src="https://www.youtube.com/embed/RsFT5p6QXgk" frameborder="0" allowfullscreen></iframe>
<br>

[00:00](https://www.youtube.com/watch?v=RsFT5p6QXgk&t=0s) - Video Context 
[01:16](https://www.youtube.com/watch?v=RsFT5p6QXgk&t=76s) - Enumerating User Permissions 
[03:46](https://www.youtube.com/watch?v=RsFT5p6QXgk&t=226s) - Enumerating roles 
[08:05](https://www.youtube.com/watch?v=RsFT5p6QXgk&t=485s) - Identifying exploitation path and assuming lambaManager role 
[11:29](https://www.youtube.com/watch?v=RsFT5p6QXgk&t=689s) - Creating lambda function exploit 
[17:51](https://www.youtube.com/watch?v=RsFT5p6QXgk&t=1071s) - Running exploit with lambda:Invoke
