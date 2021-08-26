## Nmap:

```bash
# Nmap 7.80 scan initiated Mon May 17 08:55:54 2021 as: nmap -sC -sV -oN main.nmap 10.200.19.222
Nmap scan report for 10.200.19.222
Host is up (1.3s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE       VERSION
22/tcp   open  ssh           OpenSSH for_Windows_7.7 (protocol 2.0)
| ssh-hostkey: 
|   2048 7e:4c:57:d7:20:e7:4d:b6:38:34:34:e9:3d:04:ce:f5 (RSA)
|   256 22:23:4a:cb:c9:7a:f0:24:f5:50:c6:d7:ad:e4:b1:d8 (ECDSA)
|_  256 90:00:01:0a:27:35:17:a6:14:64:af:19:01:4e:1f:4d (ED25519)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THROWBACK
|   NetBIOS_Domain_Name: THROWBACK
|   NetBIOS_Computer_Name: THROWBACK-WS01
|   DNS_Domain_Name: THROWBACK.local
|   DNS_Computer_Name: THROWBACK-WS01.THROWBACK.local
|   DNS_Tree_Name: THROWBACK.local
|   Product_Version: 10.0.19041
|_  System_Time: 2021-05-17T16:18:13+00:00
| ssl-cert: Subject: commonName=THROWBACK-WS01.THROWBACK.local
| Not valid before: 2021-05-13T12:27:08
|_Not valid after:  2021-11-12T12:27:08
|_ssl-date: 2021-05-17T16:18:27+00:00; -1s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-05-17T16:18:14
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon May 17 09:18:28 2021 -- 1 IP address (1 host up) scanned in 1353.88 seconds

```