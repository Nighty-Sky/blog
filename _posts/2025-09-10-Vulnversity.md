---
title: "Writeup: THM Room - Vulnversity"
categories: [writeup, pentesting]
tags: [writeup, pentest, tryhackme, tools , nmap, gobuster, shell, OWASP zap]
layout: default 
---
My First Privilege Escalation: TryHackMe Vulnversity Lab Write-up

Two weeks ago, I completed the Vulnversity room on TryHackMe, and it was my first experience performing a full reconnaissance-to-privilege-escalation workflow. In this post, I’ll walk you through my journey, the challenges I faced, and the lessons I learned.


---

Setting Up

I started by deploying the vulnerable VM and connecting to TryHackMe’s VPN using Kali Linux. My goal was to explore the machine, find vulnerabilities, and escalate privileges to root.


---

Step 1: Reconnaissance

Reconnaissance is always the first step in any penetration test. I ran an Nmap service scan:

nmap -sV MACHINE_IP

This revealed that a web server was running on the target machine. To enumerate directories, I used Gobuster:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/small.txt

Initially, I tried the medium wordlist, but it was too slow. Most results were JS or CSS files, but eventually, I discovered an internal upload directory, which became crucial later.


---

Step 2: Exploiting the Web Application

I wanted to see if the server allowed file uploads. Using OWASP ZAP, I confirmed that uploads were possible. Following the hints from the room, I uploaded a .phtml web shell.

Next, I set up a Netcat listener:

nc -lvnp 4444

By accessing the shell through the web interface, I obtained an initial reverse shell. To make it more stable, I upgraded it to a Python reverse shell using a script generated with ChatGPT.


---

Step 3: Privilege Escalation

Privilege escalation was both challenging and rewarding. Here’s the approach I followed:

1. Checked which commands I could run as root:



sudo -l

2. Searched for SUID files:



find / -user root -perm -4000 -exec ls -ldb {} \;

3. Used GTFOBins, a website that provides techniques for exploiting Linux binaries with elevated privileges.



By using systemctl through the identified SUID binary, I successfully escalated my privileges to root and retrieved the root flag.


---

Lessons Learned

This lab taught me several valuable lessons:

Reconnaissance is key: Understanding the services and directories running on a machine can reveal potential attack vectors.

Tools matter: Gobuster, OWASP ZAP, and Netcat were essential for exploitation.

Patience is important: Privilege escalation requires careful analysis of permissions and binaries.

Small wins feel huge: Completing my first privilege escalation was extremely satisfying and motivating.



---

Conclusion

The Vulnversity lab was an amazing introduction to real-world penetration testing. It gave me hands-on experience with reconnaissance, web exploitation, and privilege escalation. Completing this lab boosted my confidence and made me eager to tackle more advanced challenges.


---
