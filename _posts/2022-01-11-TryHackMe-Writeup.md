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