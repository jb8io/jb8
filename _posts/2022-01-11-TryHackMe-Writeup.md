---
layout: post
title: 'TryHackMe Writeup #1: RootMe CTF'
permalink: /posts/1
published: true
---
# RootMe?
![](https://i.imgur.com/yCPx2uI.png)

RootMe is labeled as a "CTF for beginners", and contains 4 different sections with different tasks. The main goal of the CTF is to escalate privileges and gain root access. I'll link it [here](https://tryhackme.com/room/rrootme) if you wanna try it. Let's go!

## Deploy the machine

This ones pretty self explanatory. Let's move on.

## Reconnaisance

Here we go..

Firstly, it asks us to scan how many ports are open on the machine. A simple NMAP will suffice.

![](https://i.imgur.com/kEcELXC.jpg)

The scan showed **two** open ports, 22 and 80. Moving on, the next question asks what version of Apache is running. To find this, I used a more specfic NMAP scan, NMAP -sV. This will display the services running as well as their versions.

![](https://i.imgur.com/hUCURiJ.jpg)

From the scan, I was able to see the version of Apache running was Apache **2.4.29**. Next, I needed to use the GoBuster tool to find directories on the web server. I used a common wordlist already given on the Linux machine on TryHackMe for the scan.

[](https://i.imgur.com/akpUv1m.jpg)

The scan showed 9 different directories found on the machine. The final question in the reconnaisance section asked what the hidden directory is. This directory is the **/panel** directory.

## Getting a shell

There is not a whole lot of guidance in this section. There was only one question that needed to be answered.

![](https://i.imgur.com/CezyacB.jpg)

To start, I tried to access the hidden directory by reaching it through a browser. Doing so, I found a page title HackIt. The page featured a section to upload a file to. Unsure why this was featured or what to do with it, I tried uploading a file. This did nothing but provide me with a message saying the file upload was successful in what I believe to be Portuguese. 

![](https://i.imgur.com/cEVY5Yp.jpg)

Unsure what to do next, I checked the hints provided on TryHackMe.

![](https://i.imgur.com/IDScB2W.jpg)

[File upload bypass](https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass) is done on websites with file upload mechanisms like this one. Through [php-reverse-shell](https://pentestmonkey.net/tools/web-shells/php-reverse-shell), I can upload a file to a site similar to this one, and access via the appropriate URL in browser. So that is what would be done here. I realized that the php-reverse-shell script needed to be changed in order to work properly in this situation, so I changed it accordingly. The IP and port both needed to be changed in order for this to work properly.  

![](https://i.imgur.com/VzFwyX3.jpg)

After trying to upload the file to the website, it was denied. THe website said that php files were not excecpted, in Portuguese again.

![](https://i.imgur.com/cpLnUub.jpg)

Luckily, there was a workaround. The extension of the script can be modified to trick the machine and stil get the executable to run. Changing the extension of the script to php5 was enough to get the file uploaded. As can be seen here the file is in the uploads section of the website.

![](https://i.imgur.com/O1elLbY.jpg)

Next, I needed to start a listener on the port I specified in the reverse php file.

![](https://i.imgur.com/zfZ9dO7.jpg)

Then, I ran the php reverse shell script using the curl command.

![](https://i.imgur.com/uZOvr4X.jpg)

After running this, I saw that there is a listener on my connection in the terminal.

![](https://i.imgur.com/2hksnYx.jpg)

This means the php reverse shell worked successfully, and I recieved access to a shell! Now all I needed to do was find the flag to finish this section. Considering user.txt was given to me in the original question, I searched for that file first to see if I can find the flag there. I searched using the find command, and was hit with countless error messages. Then, I searched excluding error messages and was able to find the file.

![](https://i.imgur.com/VmZSaHB.jpg)

Boom! I was able to find the file, now all that is left to do is find the flag and move on to the next section. 

I was unable to access the file using nano, so I used cat to try and get the contents of the file and was able find the flag. 

![](https://i.imgur.com/4YkwnML.jpg)

## Privilege Escalation

After accessing the shell, all there was left to do is find a way to escalate my priveleges. Before doing this, there is one more question to solve. The CTF wanted me to search for files with SUID permissions, meaning files that are able to be run as if I was the file owner.  We can search for these files by using the find command again.

![](https://i.imgur.com/ldjddPv.jpg)

A plethora of files showed up after putting in this search, but which one seemed **weird**? That file would be the python file, or /user/bin/python. This file should not be executable with SUID permissions because of how much harmful stuff can be done with this command. 
Finally, I reached the point where all I need to do is escalate my privilege to gain root access. To do so, I enlisted the help of [GTFObins](https://gtfobins.github.io/). This site has a list of different binaries to help bypass systems and escalate privileges. After searching python on the GTFObins website, I was able to find it.

![](https://i.imgur.com/y1UoyzL.jpg)

Following finding this, I was able to run the command given on the website with the python file. After running the command on the python file, I was able to gain root access!

![](https://i.imgur.com/ycJ50UR.jpg)

Now that I escalated my privileges to root access, all there was left to do was find the root.txt file and obtain the final flag. To do so, I used to find command one last time and was able to find the file, which held the last flag of the CTF.

![](https://i.imgur.com/wPZkgnc.jpg)

![](https://i.imgur.com/9lrjojR.jpg)
