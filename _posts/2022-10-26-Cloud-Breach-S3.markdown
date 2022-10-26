---
layout: post
title: Hacking in the Cloud - vulnerable_lambda
description: This is the first scenario in the CloudGoat series. We start off as a low-privileged user that can assume a role which gives Lambda:Invoke permissions. Using this permission we are able to exploit a high-privileged Lambda function via an SQL injection and obtain Administrator access.
date:   2022-10-26
image:  '/images/cloud_breach_s3_web.png'
category: Videos
tags:   [Videos, SSRF, Cloudgoat, AWS, S3]
---

This scenario is based off of a real cloud breach regarding Capital One's 2019 data breach that affected over 100 million customers. 

This scenario starts off by providing a public IP address for the targeted EC2 instance. After querying the instance's metadata service, the credentials can be used to obtain sensitive data in S3 buckets. 

We go over how to mitigate this misconfiguration, how to exfiltrate credentials stealthily, and we look into the GuardDuty findings and CloudTrail logs to see what our activity looks like from a defender standpoint.

<iframe src="https://www.youtube.com/embed/1MS0GPwmVMI" frameborder="0" allowfullscreen></iframe>
<br>
[00:00](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=0s) - Video Context<br>
[00:44](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=44s) - Querying EC2 Metadata Service <br>
[03:35](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=215s) - Bypassing GuardDuty Alerts <br>
[06:52](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=412s) - SneakyEndpoints GuardDuty Bypass <br>
[09:14](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=554s) - Retrieving Sensitive Data in S3 <br>
[10:42](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=642s) - Viewing CloudTrail Logs <br>
[13:07](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=787s) - Potential for New GuardDuty Finding <br>
[14:29](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=869s) - Looking at GuardDuty Findings<br>
[16:12](https://www.youtube.com/watch?v=1MS0GPwmVMI&t=972s) - Hardening EC2 Instance (IMDSv2)
