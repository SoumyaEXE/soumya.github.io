---
layout: post
title: Hacking in the Cloud - ecs_takeover
description: We gain access to the targeted AWS account by finding an SSRF and RCE vulnerability on an AWS-hosted webapp. We then pivot to other containers and use the metdata credentialso f both the compromised EC2 instance and other docker containers to obtain elevated access within the AWS workload.
date:   2022-12-10
image:  '/images/ecs_takeover_web.png'
category: Videos
tags: [Videos, Cloudgoat, AWS, IAM, EC2, Lambda]
---

Starting with no access to the AWS account, we compromise a webapp hosted in an EC2 instance by finding both an SSRF and RCE vulnerability.
This webapp was hosted inside of a privileged docker container with the host's docker socket mounted on it. We were therefore able to pivot to other containers, and using the EC2 instance metadata role coupled with the container metadata credentials of other containers, it was possible to gain access to other containers outside of the compromised container instance.

<iframe src="https://www.youtube.com/embed/Dd-joQjyaZQ" frameborder="0" allowfullscreen></iframe>
<br>
[00:00](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=0s) - Video Context<br>
[00:51](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=51s) - Configuring AWSealion and accessing EC2 webapp<br>
[01:27](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=87s) - Finding SSRF and exfiltrating credentials<br>
[03:40](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=220s) - Enumerating EC2 instance role permissions<br>
[04:28](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=268s) - Finding RCE in the webapp and getting a reverse shell<br>
[07:50](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=470s) - Internal enumeration of EC2 instance<br>
[08:55](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=535s) - Escaping out of privileged webapp container<br>
[12:29](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=749s) - Pivoting to privileged ECS container<br>
[15:45](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=945s) - Performing enumeration to find privilege escalation path<br>
[19:43](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=1183s) - Finding potential ECS task exploitation pathway<br>
[21:48](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=1308s) - Analyzing task definitions<br>
[25:51](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=1551s) - Draining container instance to get the vault container<br>
[30:45](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=1845s) - Viewing contents of vault container<br>
[32:58](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=1978s) - Post-Exploitation Analysis<br>
[37:58](https://www.youtube.com/watch?v=Dd-joQjyaZQ&t=2278s) - Checking GuardDuty findings<br>
