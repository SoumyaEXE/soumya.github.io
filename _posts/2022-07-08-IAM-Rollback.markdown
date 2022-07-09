---
layout: post
title: Hacking in the Cloud - IAM Privesc by Rollback
description: This is the second scenario in the CloudGoat series, and it is the simplest one at the time of writing. We start off as a high-privileged user who can change their defualt policy version. One of the versions of this user's managed policy allows for performing any action on any resource. The user can therefore change their default version to this policy version and obtain Administrator access.
date:   2022-06-10 
image:  '/images/iam_privesc_by_rollback.png'
category: Videos
tags:   [Videos, IAM, Cloudgoat, AWS]
---

This is the second scenario in the CloudGoat series, and it is the simplest one at the time of writing. We start off as a high-privileged user who can change their defualt policy version. One of the versions of this user's managed policy allows for performing any action on any resource. The user can therefore change their default version to this policy version and obtain Administrator access.

<iframe src="https://www.youtube.com/embed/mMq-A7mmnPc" frameborder="0" allowfullscreen></iframe>
<br>
[00:00](https://www.youtube.com/watch?v=mMq-A7mmnPc&t=0s) - Video Context<br> 
[00:52](https://www.youtube.com/watch?v=mMq-A7mmnPc&t=52s) - Enumerating user's permissions<br> 
[06:37](https://www.youtube.com/watch?v=mMq-A7mmnPc&t=397s) - Changing policy's default permissions<br>
[09:45](https://www.youtube.com/watch?v=mMq-A7mmnPc&t=585s) - Getting AdministratorAccess<br>
