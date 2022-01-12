---
layout: post
title: 'TryHackMe Writeup #1: RootMe CTF'
permalink: /posts/1
published: true
---
# RootMe?
![](https://i.imgur.com/yCPx2uI.png)

RootMe is labeled as a "CTF for beginners", and contains 4 different sections with different tasks. The main goal of the CTF is to escalate privileges and gain root access. Let's go!

## Deploy the machine

This ones pretty self explanatory. Let's move on.

## Reconnaisance

Here we go..

Firstly, it asks us to scan how many ports are open on the machine. A simple NMAP will suffice.

NMAP photo

The scan showed **two** open ports, 22 and 80. Moving on, the next question asks what version of Apache is running. To find this, I used a more specfic NMAP scan, NMAP -sV. This will display the services running as well as their versions.

NMAP -sV photo

From the scan, I was able to see the version of Apache running was Apache **2.4.29**. Next, I needed to use the GoBuster tool to find directories on the web server. I used a common wordlist already given on the Linux machine on TryHackMe for the scan.

Gobuster photo

The scan showed 9 different directories found on the machine. The final question in the reconnaisance section asked what the hidden directory is. This directory is the **/panel** directory.

## Getting a shell

There is not a whole lot of guidance in this section. There was only one question that needed to be answered.

Question photo

To start, I tried to access the hidden directory by reaching it through a browser. Doing so, I found a page title HackIt.

Page found photo

The page featured a section to upload a file to. Unsure why this was featured or what to do with it, I tried uploading a file. This did nothing but provide me with a message saying the file upload was successful in what I believe to be Portuguese. 

Portuguese photo

Unsure what to do next, I checked the hints provided on TryHackMe.

Hint Photos

[File upload bypass](https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass) is done on websites with file upload mechanisms like this one. Through [php-reverse-shell](https://pentestmonkey.net/tools/web-shells/php-reverse-shell), I can upload a file to a site similar to this one, and access via the appropriate URL in browser. So that is what would be done here. I realized that the php-reverse-shell script needed to be changed in order to work properly in this situation, so I changed it accordingly. The IP and port both needed to be changed in order for this to work properly.

PHP photo

After trying to upload the file to the website, it was denied. THe website said that php files were not excecpted, in Portuguese again.

No php photo

Luckily, there was a workaround. The extension of the script can be modified to trick the machine and stil get the executable to run. Changing the extension of the script to php5 was enough to get the file uploaded. As can be seen here the file is in the uploads section of the website.

upload screencap

Next, I needed to start a listener on the port I specified in the reverse php file.

listener photo

Then, I ran the php reverse shell script using the curl command.




